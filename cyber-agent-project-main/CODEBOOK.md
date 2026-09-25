# CODEBOOK: CYBER-AGENT-PROJECT Golden Rules

This codebook is the default operating policy for all work in this repository.
It is optimized for a one-month delivery window for the SEED lab environment.

## 1) Mission and Scope

- Build a safe, evidence-driven autonomous cyber lab agent for authorized targets only.
- Follow methodology order: Recon -> Enumeration -> Vulnerability Analysis -> Exploitation -> Post-Exploitation -> Reporting.
- Prefer practical, testable increments over broad unfinished features.

## 2) Non-Negotiable Safety Rules

- Lab-only usage. Never run against unauthorized systems.
- Every capability must use central policy checks (target + tool allowlist).
- No persistence, destructive actions, or external downloads.
- Every subprocess execution must include a timeout.
- Every tool result must be JSON-serializable and include enough evidence for reporting.

## 3) Architecture Rules

- `tools.json` is the single source of truth for controller tool schemas.
- Capabilities are auto-discovered from `kali_agent/capabilities/<phase>/` via `@capability`.
- New tools must be wired in exactly three places:
  1. Add capability module with decorated `run(target) -> dict`.
  2. Add tool name to `ALLOWED_TOOLS` in `kali_agent/policy.py`.
  3. Add manifest entry to `tools.json`.
- Do not hardcode routes in `kali_agent/kali_agent.py` for new tools.

## 4) Capability Implementation Contract

Each capability must:

- Accept `target` as `host:port`.
- Return dict fields at minimum:
  - `status`
  - `tool`
  - `target`
  - `command_category`
  - `started_utc`
  - `command`
  - `return_code`
  - `stdout`
  - `stderr`
- Fail safely with explicit error metadata instead of silent failures.
- Keep command execution read-only or minimally invasive by design.

## 5) Coding Standards

- Keep modules small and single-purpose.
- Reuse existing output conventions from current capabilities.
- Write concise docstrings for capability purpose and safety boundaries.
- Avoid introducing new dependencies unless there is clear value.
- Prefer deterministic parsing and explicit evidence extraction.

## 6) Environment Setup Standard (Task 1)

Goal: all contributors can run tests and local development quickly.

Checklist:

1. Use VS Code with Python extension and lint/test integration.
2. Create and activate a Python environment.
3. Install root dev dependencies from `requirements-dev.txt`.
4. Install service-specific dependencies:
   - `controller/requirements.txt`
   - `kali_agent/requirements.txt`
5. Create `controller/.env` with provider/model settings and authorized target.
6. Verify policy allowlists match authorized target(s) in SEED lab.

Exit criteria:

- `pytest` runs from repo root without import failures.
- Controller and Kali agent modules import successfully.

## 7) Codebase Review Protocol (Task 2)

Goal: understand framework behavior before adding new tools.

Review order:

1. `README.md` for architecture and workflow.
2. `CONTRIBUTING.md` for capability contract and wiring points.
3. `tools.json` for current tool schemas.
4. `kali_agent/registry.py` and `kali_agent/kali_agent.py` for discovery/routing.
5. `kali_agent/policy.py` for guardrails.
6. Existing capabilities under each phase for coding patterns.
7. `tests/` to understand expected behavior and constraints.

Deliverable:

- A short design note before implementing each new capability: purpose, evidence trigger, expected output shape, and safety limits.

## 8) New Capability Development Plan (Task 3)

Start with highest-value, lowest-risk wrappers already planned in roadmap:

1. `run_whatweb` (recon)
2. `run_gobuster` (enum)
3. `run_nmap_nse` (vuln)

Per-capability workflow:

1. Create module in proper phase folder.
2. Implement `run(target)` with timeout and structured output.
3. Add to `ALLOWED_TOOLS`.
4. Add manifest entry in `tools.json`.
5. Add tests:
   - Registry discovery
   - Route exposure
   - Policy enforcement
   - Output schema sanity
6. Validate no regression in existing tests.

Definition of done for a capability:

- Capability discovered and routable.
- Policy permits only authorized use.
- Tests pass.
- Roadmap item updated.

## 9) Lab Configuration and IP Governance (Task 4)

Single source for active lab settings:

- `controller/.env` for runtime target/provider URLs.
- `kali_agent/policy.py` for hard allowlists.

Rules:

- `AUTHORIZED_TARGET` must match one allowed target in policy.
- Update both files together when lab IPs change.
- Keep a dated change note in PR description when target ranges are updated.

SEED validation checklist:

1. Confirm Kali IP and host bridge route.
2. Confirm SEED VM IP and service ports.
3. Confirm controller can reach Kali agent URL.
4. Confirm Kali tools can reach authorized target.
5. Confirm blocked response for out-of-scope target.

## 10) One-Month Execution Plan (Task 5)

Week 1:

- Finalize setup and baseline tests.
- Finish codebase review and document design notes.
- Implement 1 capability (`whatweb`) with tests.

Week 2:

- Implement 1 to 2 capabilities (`gobuster`, `nmap_nse`) with tests.
- Harden parsing and evidence normalization.

Week 3:

- Implement next roadmap item (`metasploit` or `redis` check improvements based on lab readiness).
- Improve report synthesis inputs from tool outputs.

Week 4:

- Stabilization, regression testing, and documentation updates.
- Final demo scenario rehearsal in SEED lab.

## 11) Testing Strategy

Run tests at three levels:

1. Unit-style capability tests:
   - Output shape
   - Error handling
   - Timeout behavior
2. Integration-style route tests:
   - Route auto-registration
   - Policy blocking/allow behavior
3. End-to-end controller loop smoke tests:
   - Tool call -> Kali response -> message feedback loop

Minimum command set when validating changes:

- `pytest`
- Focused test runs for touched areas (for example `tests/test_routes.py`).

Quality gates before merge:

- No failing tests.
- No policy bypass pathways.
- Manifest/registry/policy consistency maintained.

## 12) Working Agreement

- Build in small increments and test each increment immediately.
- Prefer explicit evidence and deterministic behavior over heuristic-only logic.
- If a capability cannot be validated safely in the current lab state, mark it blocked with reason and move to next roadmap item.
