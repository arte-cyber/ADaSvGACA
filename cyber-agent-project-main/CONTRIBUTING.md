# Contributing

How to add a new capability to the agent. See [`ROADMAP.md`](ROADMAP.md) for the
curated list of planned tools and [`SECURITY.md`](SECURITY.md) for scope rules.

> **Note:** This contract is for Kali-side capabilities (recon, enum, vuln,
> exploit, post) that run a tool against a target.

## Branch workflow

Work happens on short-lived feature branches off `main`, one per capability.

```bash
git checkout main
git pull
git checkout -b feat/<phase>-<tool>     # e.g. feat/enum-nikto
# ...build, commit...
git push -u origin feat/<phase>-<tool>
# open a PR into main
```

**Naming convention:** `feat/<phase>-<tool>`, where `<phase>` is one of
`recon`, `enum`, `vuln`, `exploit`, `post`, `report`. The exact branch name for
each planned tool is listed in `ROADMAP.md`.

## Anatomy of a capability

A capability is one module exposing a single `run` function, tagged with the
`@capability` decorator so the agent auto-discovers and routes it:

```python
from registry import capability

@capability(name="run_nikto", phase="scanning_enumeration")
def run(target: str) -> dict:
    ...
```

The tool's `description` and parameter schema live in the shared manifest
(`tools.json`), not the decorator — see step 3.

- **Input:** `target` is a `"host:port"` string (the authorized lab target).
- **Output:** a JSON-serializable `dict` — it is returned straight to the
  controller as the tool result.
- **Routing:** on startup the agent imports every capability module and builds a
  guarded `POST /<name>` route automatically. You do **not** write Flask routes
  or import the module in `kali_agent.py`.

### Return-dict conventions

Follow the shape the existing modules use so the controller (and any reporting
step) can consume results uniformly:

```python
{
    "status": "completed",          # or "error"
    "tool": "nikto",                # the underlying binary
    "target": target,
    "command_category": "web_scan", # short phase/intent tag
    "started_utc": "...Z",          # datetime.utcnow().isoformat() + "Z"
    "command": "nikto -h ...",      # the exact command run
    "return_code": 0,
    "stdout": "...",
    "stderr": "...",
    # add tool-specific fields as needed, e.g. "indicators", "success", "evidence"
}
```

Run external tools with `subprocess.run(..., capture_output=True, text=True,
timeout=N)` and always set a `timeout`. Reference implementations:
`scanning_enumeration/nmap_scan.py`, `reconnaissance/http_probe.py`,
`vulnerability_analysis/shellshock_check.py`.

## The three wiring points

A capability registry handles discovery and routing, so adding a tool now
touches three places (the agent imports and routes are automatic):

1. **Implement the module** — `kali_agent/capabilities/<phase>/<tool>.py` with a
   `@capability(...)`-decorated `run(target) -> dict` (see above). That's all the
   agent side needs: it's auto-discovered and gets a guarded route.

2. **Allowlist the tool** in `kali_agent/policy.py` — add the `name` to
   `ALLOWED_TOOLS`. Registering a capability does **not** auto-allow it; this is
   the deliberate per-tool kill switch:
   ```python
   ALLOWED_TOOLS = { ..., "run_nikto" }
   ```

3. **Add the tool to the manifest** — append an entry to `tools.json` at the
   repo root: `name` (must match the decorator), `description` (when/why to use
   it — the model selects tools by evidence), and the `parameters` JSON schema:
   ```jsonc
   {
     "name": "run_nikto",
     "description": "Run nikto against the target after an HTTP service is found.",
     "parameters": { "type": "object",
                     "properties": { "target": { "type": "string" } },
                     "required": ["target"] }
   }
   ```
   Both sides derive from this single file: the controller builds the LLM tool
   schemas from it, and the agent validates it on startup — if `tools.json`, the
   registry, and `ALLOWED_TOOLS` disagree, the agent refuses to start.

## Safety checklist (per capability)

- [ ] Decorated with `@capability` (so it gets the central target/tool guards).
- [ ] No persistence, destructive actions, or external downloads.
- [ ] Has a `subprocess` timeout.
- [ ] Added to `ALLOWED_TOOLS` and to `tools.json` with a clear `description`.
- [ ] Tested against an authorized lab target only.

## Before opening a PR

- Run the test suite from the repo root: `pytest`
  (one-time setup: `pip install -r requirements-dev.txt`).
- Add or update tests in `tests/` for your capability.
- Update the item's checkbox in `ROADMAP.md`.
