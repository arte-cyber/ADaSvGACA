# cyber-agent-project

## Description

AI-driven autonomous cybersecurity agent for the school *AI for Cybersecurity* course project.

The agent is organized around the standard hacking methodology: Reconnaissance →
Scanning/Enumeration → Vulnerability Analysis → Exploitation →
Post-Exploitation/Privilege Escalation → Documentation/Reporting.

## Getting started

Welcome weary traveller...Pick your path:

- **Run it** → [`docs/setup.md`](docs/setup.md) — install and launch the agent and controller.
- **Understand it** → the [Architecture](#architecture) and [Project layout](#project-layout) sections below.
- **Build a capability** → [`CONTRIBUTING.md`](CONTRIBUTING.md) — the three wiring points.
- **Pick something to build** → [`ROADMAP.md`](ROADMAP.md) — curated tools by phase.
- **Scope & safety** → [`SECURITY.md`](SECURITY.md) — lab-only, authorized targets only.

## Architecture

```
Windows Host
   ↓
Controller (LLM orchestrator)
   ↓
Kali Agent (Flask tool server)
   ↓
Security Tools
   ↓
SEED VM
   ↓
Docker Targets
```

- **`controller/controller.py`** — Runs on the Windows host. Uses an LLM with
  function-calling to select tools, enforces the authorized-target scope, and
  produces an evidence-based report.
- **`tools.json`** — Shared tool manifest (name, description, parameters): the
  single source of truth both the controller and agent derive from.
- **`kali_agent/kali_agent.py`** — Flask server (port 5050) on Kali. Exposes one
  endpoint per capability; every endpoint enforces the target/tool allowlist.
- **`kali_agent/policy.py`** — Hard allowlist of approved targets and tools.
- **`kali_agent/capabilities/`** — Tool implementations, organized by
  methodology phase (see below).

## Project layout

```
cyber-agent-project/
├── controller/          # LLM orchestrator (runs on the Windows host)
│   ├── controller.py
│   └── reporting/       # markdown report generation
├── kali_agent/          # Flask tool server (runs on Kali)
│   ├── kali_agent.py    # auto-discovers capabilities, serves guarded routes
│   ├── registry.py      # @capability registry
│   ├── policy.py        # target/tool allowlists
│   └── capabilities/    # tools, by methodology phase
├── tools.json           # shared tool manifest (single source of truth)
├── tests/               # pytest suite
└── docs/                # setup.md, git-workflow.md
```

## Capabilities by methodology phase

Each capability lives under `kali_agent/capabilities/<phase>/` and exposes a
`run(target) -> dict`.

| Phase | Module | Status |
|---|---|---|
| Reconnaissance | `reconnaissance/http_probe.py` | Working — HTTP headers, body preview, CGI/Apache indicators |
| Scanning / Enumeration | `scanning_enumeration/nmap_scan.py` | Working — `nmap -sV` service/version detection |
| Vulnerability Analysis | `vulnerability_analysis/shellshock_check.py` | Working — Shellshock (CVE-2014-6271) validation |
| Exploitation | `exploitation/` | Working — e.g. Metasploit, full RCE chains |
| Post-Exploitation / Privilege Escalation | `post_exploitation/` | Working |

**Documentation / Reporting** is handled controller-side — the report is
synthesized by the controller from collected evidence, so it lives under
[`controller/reporting/`](controller/reporting/), not `kali_agent/capabilities/`.

## Roadmap & contributing

Planned capabilities (curated by phase) are tracked in
[`ROADMAP.md`](ROADMAP.md). To add a capability, see
[`CONTRIBUTING.md`](CONTRIBUTING.md). For the team's Git/GitHub conventions, see
[`docs/git-workflow.md`](docs/git-workflow.md).

## Setup

See [`docs/setup.md`](docs/setup.md).

## Security

This is a **lab-only** tool, scoped to authorized targets via a two-layer
allowlist. Do not use it against systems you do not own or have permission to
test. See [`SECURITY.md`](SECURITY.md).
