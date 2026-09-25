# agents.md: Golden Rules for Human and AI Agents

This file defines how contributors (human and AI) execute work in this repository.

## 1) Core Agent Behavior

- Always respect lab-only scope and explicit authorization boundaries.
- Make changes that are minimal, reversible, and testable.
- Keep controller, policy, and manifest aligned.
- Prefer evidence-based tool usage over random tool chaining.

## 2) Role Definitions

### Planner Agent

Responsibilities:

- Translate roadmap items into small implementation tasks.
- Identify dependencies and safety constraints before coding.
- Produce a test checklist per task.

Outputs:

- Task breakdown
- Acceptance criteria
- Test plan

### Capability Agent

Responsibilities:

- Implement one capability at a time using existing patterns.
- Enforce timeout and structured output conventions.
- Wire the capability in policy + manifest.

Outputs:

- New capability module
- Policy update
- Manifest entry
- Tests

### QA Agent

Responsibilities:

- Validate discovery, routing, and policy enforcement.
- Run regression tests and summarize risk.
- Confirm IP/lab config consistency.

Outputs:

- Test results summary
- Risk notes
- Follow-up fixes list

## 3) Mandatory Delivery Workflow

For each roadmap capability:

1. Read existing phase examples in `kali_agent/capabilities/`.
2. Draft a micro-spec (purpose, trigger evidence, command, output fields).
3. Implement module with `@capability` and `run(target) -> dict`.
4. Add tool allowlist entry in `kali_agent/policy.py`.
5. Add matching schema entry in `tools.json`.
6. Add or update tests in `tests/`.
7. Run tests and verify no regressions.

## 4) Safety and Scope Guardrails

- Never bypass policy checks.
- Never add capabilities that perform destructive actions.
- Never add external download behavior by default.
- Reject out-of-scope targets predictably.

## 5) Configuration Control (SEED Lab)

Whenever lab settings change:

1. Update `controller/.env` values.
2. Update `kali_agent/policy.py` allowlists if needed.
3. Validate connectivity path:
   - Controller -> Kali agent
   - Kali agent -> authorized target
4. Confirm blocked behavior for unauthorized targets.

## 6) Project Task Execution Plan

### Task A: Complete local development environment setup

Steps:

1. Prepare Python environment.
2. Install `requirements-dev.txt`.
3. Install `controller/requirements.txt` and `kali_agent/requirements.txt`.
4. Configure `.env` in `controller/`.
5. Confirm imports and test discovery.

Validation:

- `pytest` executes from repository root.

### Task B: Review existing framework

Steps:

1. Read architecture and contribution docs.
2. Inspect registry/discovery and policy flow.
3. Inspect current capabilities for output conventions.
4. Inspect current tests to map expected behavior.

Validation:

- Produce a short summary of how a tool is discovered, exposed, and validated.

### Task C: Develop additional wrappers/capabilities

Initial priority list:

1. `whatweb`
2. `gobuster`
3. `nmap_nse`

Validation:

- Capability is discoverable, routable, policy-allowed, and tested.

### Task D: Verify and update IP/lab configuration

Steps:

1. Confirm live SEED target IP:port.
2. Update controller target env.
3. Verify policy allowlist alignment.
4. Perform route-level validation using authorized and unauthorized targets.

Validation:

- Authorized requests pass, unauthorized requests are blocked.

### Task E: Execute one-month timeline immediately

Week-by-week:

1. Week 1: setup + review + first capability.
2. Week 2: two capability additions + tests.
3. Week 3: higher-complexity capability + report quality improvements.
4. Week 4: stabilization and demo rehearsal.

Validation:

- Weekly goals completed with passing tests and no safety regressions.

## 7) Testing Rules

- Test every new capability before moving to next one.
- Keep focused tests for touched files plus full `pytest` regression runs.
- Treat policy enforcement tests as critical-path tests.
- If tests fail, fix root cause before continuing roadmap work.

## 8) Definition of Done

A task is complete only when:

- Code changes are implemented.
- Tests for changed behavior pass.
- No regression in existing test suite.
- Safety and scope checks are preserved.
- Documentation/roadmap status is updated.
