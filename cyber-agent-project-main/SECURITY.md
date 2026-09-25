# Security Policy

## Purpose and scope

This repository is an **educational, lab-only** autonomous cybersecurity agent,
built for the school *AI for Cybersecurity* course. It is designed to operate
exclusively against **deliberately vulnerable targets that you own and control**
inside an isolated lab (a SEED VM running Docker targets).

It is **not** intended, authorized, or fit for use against any system you do not
own or do not have explicit, written permission to test. Using offensive
security tooling against systems without authorization is illegal in most
jurisdictions.

## Authorization model

The project enforces scope in two independent layers. Both must agree before any
tool runs:

1. **Controller (`controller/controller.py`)** — Every tool call is checked
   against the `AUTHORIZED_TARGET` from the environment. Calls targeting anything
   else are blocked before they ever reach the agent.
2. **Kali Agent (`kali_agent/policy.py`)** — A hard allowlist of both **targets**
   (`ALLOWED_TARGETS`) and **tools** (`ALLOWED_TOOLS`). Any request outside the
   allowlist returns `403 blocked`.

This defense-in-depth means a misconfigured controller cannot direct the agent
at an unapproved host, and an unexpected agent request cannot run an unapproved
tool.

## Operating guidelines

- Run only inside an **isolated lab network** with no route to production or the
  public internet.
- Keep `ALLOWED_TARGETS` (in `policy.py`) and `AUTHORIZED_TARGET` (in `.env`)
  scoped to the **minimum** set of lab targets you are authorized to assess.
- The system prompt instructs the agent to avoid persistence, destructive
  actions, external downloads, and any activity outside the lab. Do not weaken
  these constraints.
- Add new capabilities to `ALLOWED_TOOLS` deliberately, and only after reviewing
  what they do.

## Handling secrets

- **Never commit secrets.** `OPENAI_API_KEY` and other configuration live in
  `controller/.env`, which is excluded by `.gitignore` (`.env`, `*.env`, `*.key`,
  `controller/api_keys.py`, `controller/private_config.py`).
- If a key is ever committed, **rotate it immediately** — removing it in a later
  commit does not invalidate it in git history.

## Reporting a concern

This is a course project, not a production system, so there is no formal
vulnerability-disclosure process. If you find a safety issue (for example, a way
the scope enforcement can be bypassed), open an issue or contact the maintainers
directly.
