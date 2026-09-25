# Setup

This project has two components that run on different machines:

| Component | Runs on | Entry point |
|---|---|---|
| **Controller** | Windows host | `controller/controller.py` |
| **Kali Agent** | Kali Linux (in the SEED VM lab) | `kali_agent/kali_agent.py` |

The controller (LLM orchestrator) sends tool requests over HTTP to the Kali
agent, which executes security tools against the authorized lab target.

```
Windows Host (controller) ──HTTP──> Kali Agent (Flask :5050) ──> Target
```

## Prerequisites

- **Python 3.8+** on both the Windows host and the Kali machine.
- **Kali machine** must have the underlying tools on its `PATH`:
  - `nmap` (used by `scanning_enumeration/nmap_scan.py`)
  - `curl` (used by `reconnaissance/http_probe.py` and
    `vulnerability_analysis/shellshock_check.py`)
- A running **SEED VM** lab with the **Docker target(s)** reachable from Kali.
- An **OpenAI API key** for the controller.

## 1. Kali Agent (run this first)

The controller can't do anything until the agent is listening.

```bash
# On the Kali machine, from the repo root:
cd kali_agent
pip install -r requirements.txt
python kali_agent.py
```

The agent listens on `0.0.0.0:5050` and exposes one endpoint per capability:

- `POST /run_nmap_scan`
- `POST /run_http_probe`
- `POST /run_shellshock_check`

Each request body is `{"target": "<host:port>"}`. Every endpoint rejects
requests whose tool or target is not in the allowlist (see below).

### Authorize the target

The agent only acts on targets explicitly listed in `kali_agent/policy.py`:

```python
ALLOWED_TARGETS = {
    "192.168.185.3:8080",   # update to match your lab target
}
```

Edit `ALLOWED_TARGETS` to match your Docker target's `host:port`. Requests for
anything else return `403 blocked`. See [`SECURITY.md`](../SECURITY.md) for the
rationale behind this allowlist.

## 2. Controller (Windows host)

```bash
# From the repo root:
cd controller
pip install -r requirements.txt
```

### Configure `.env`

Create a `.env` file in the `controller/` directory. It is git-ignored
(`.gitignore` excludes `.env` / `*.env`), so your key never gets committed.

```ini
OPENAI_API_KEY=sk-...                       # required
KALI_AGENT_URL=http://<kali-ip>:5050        # required, e.g. http://192.168.185.2:5050
AUTHORIZED_TARGET=192.168.185.3:8080        # required, must match policy.py allowlist
MODEL=gpt-5.5                               # optional, defaults to gpt-5.5
```

The controller raises a `RuntimeError` on startup if `OPENAI_API_KEY`,
`KALI_AGENT_URL`, or `AUTHORIZED_TARGET` are missing. It also independently
rejects any tool call whose `target` differs from `AUTHORIZED_TARGET` — a second
layer of scope enforcement on top of the agent's `policy.py`.

### Run

```bash
python controller.py
```

You will be prompted for a task:

```
=== Autonomous Cyber Agent Controller ===
Authorized target: 192.168.185.3:8080
Enter the task for the agent.

Task> Assess the web service on the authorized target and report any findings.
```

The controller loops — selecting tools, calling the Kali agent, feeding results
back to the model — until it produces a final evidence-based report.

## Troubleshooting

- **Controller exits immediately with `RuntimeError: Missing ...`** — a required
  `.env` variable is unset. Check `OPENAI_API_KEY`, `KALI_AGENT_URL`,
  `AUTHORIZED_TARGET`.
- **All tool calls return `blocked`** — the `target` doesn't match
  `AUTHORIZED_TARGET` (controller side) or isn't in `ALLOWED_TARGETS`
  (`policy.py`, agent side). Both must agree.
- **Connection refused / timeouts** — the Kali agent isn't running, or
  `KALI_AGENT_URL` points to the wrong IP/port. Confirm the agent is up and the
  Windows host can reach Kali on `5050`.
- **`nmap` / `curl` not found in agent output** — install the tool on the Kali
  machine and ensure it's on `PATH`.
