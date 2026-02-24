---
name: agent-team
description: "Coordinate a multi-agent software delivery workflow (arch/dev/test/review) for feature requests. Use when asked to implement or operate a development team agent workflow: requirements breakdown, dev + whitebox, Jenkins CI gate, blackbox testing, push/PR review, and rework loop."
---

# Agent Team

## Overview
Coordinate a four-role development team agent workflow to turn a feature request into code, CI validation, blackbox testing, code review, and iterative fixes.

## Workflow

### 1) Arch agent: requirement analysis
- Parse the user prompt into:
  - **Functional requirements** (implementation details, edge cases, dependencies)
  - **Test points** (blackbox test cases and acceptance criteria)
- Send **functional requirements** to the dev agent.
- Send **test points** to the test agent.

### 2) Dev agent: implement + whitebox + CI
- Implement the functional requirements.
- Run whitebox tests (unit/integration) until passing.
- Commit changes locally.
- Trigger Jenkins CI gate build (as configured by the user/project).
- When CI passes, notify the test agent to pull the latest CI build for blackbox testing.

### 3) Test agent: blackbox testing
- Pull the latest CI build artifacts.
- Execute blackbox test cases from the arch agent.
- Report results to the dev agent.

### 4) Review agent: code review loop
- After all tests pass, push code to the repo.
- Trigger code review email/notification.
- Notify the review agent to review the changes.
- Dev agent discusses review feedback with review agent; apply fixes if needed.
- If changes are made, repeat **Step 2 → Step 4** until review is satisfied.

## Output Expectations
- Always keep clear handoffs between agents (arch → dev/test, dev → test, dev → review).
- Maintain traceability: map each requirement to dev implementation and test coverage.
- Treat Jenkins CI as the gate before blackbox testing.
- Keep the loop strict: any review-triggered changes must re-run CI + tests.
