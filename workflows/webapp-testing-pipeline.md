---
type: workflow
id: webapp-testing-pipeline
title: Webapp Testing Pipeline
description: "Plan test cases, write and execute tests, report failures, and fix issues in an automated cycle"
tags: [Production, Customer-Facing, Developer, Loop]
connections:
  - target: test-planning
    type: uses
  - target: test-writing
    type: uses
  - target: test-execution
    type: uses
  - target: test-reporting
    type: uses
  - target: fix-verification
    type: uses
  - target: llm-service
    type: runs_on
  - target: test-plan-template
    type: references
metadata:
  estimated_duration: "10-60 minutes"
  trigger: manual
  loop_modes: ["for_each", "until_pass"]
loops:
  - id: "test-run"
    mode: "for_each"
    inputExpression: "{{steps.Test Planning.output}}"
    steps:
      - "test-writing"
      - "test-execution"
    maxIterations: 50
  - id: "fix-cycle"
    mode: "until_pass"
    steps:
      - "test-writing"
      - "test-execution"
      - "fix-verification"
    verifier: "fix-verification"
    maxIterations: 5
    freshContextPerIteration: true
output_step: "test-reporting"
composite_steps:
  - "test-planning"
  - "test-writing"
  - "test-execution"
  - "test-reporting"
  - "fix-verification"
execution:
  - skill: "test-planning"
    prompt: "plan-tests"
    step_type: "generation"
  - skill: "test-writing"
    prompt: "write-test"
    step_type: "generation"
  - skill: "test-execution"
    prompt: "execute-test"
    step_type: "synthesis"
  - skill: "test-reporting"
    prompt: "report-results"
    step_type: "synthesis"
  - skill: "fix-verification"
    prompt: "verify-fix"
    step_type: "validation"
---

## Overview

This workflow automates webapp testing through a structured pipeline. It analyses your application, generates a test plan, writes and executes each test case individually, produces a severity-ranked quality report, and optionally enters a fix cycle for failing tests.

The pipeline demonstrates both loop modes: **for_each** iterates over test cases (write + execute per case), and **until_pass** drives the fix-verify cycle for failures.

## Pipeline Stages

### Stage 1: Test Planning

The test planning step examines your application and produces a structured test plan — a JSON array of test cases. Each case has an ID, title, category, priority, preconditions, steps, and expected result.

The plan covers critical user flows first, then edge cases and error handling. Test cases are categorised (unit, integration, e2e, accessibility, performance) and prioritised (critical, high, medium, low).

### Stage 2: Test Execution Loop (for_each over test cases)

The loop iterates over each test case from the plan. For each:

**Step 1 — Write Test**

Reads the test case definition (via `{{loop.item}}`), examines the relevant source code, and writes an executable test using the project's testing framework. Follows existing test patterns and conventions.

**Step 2 — Execute Test**

Runs the written test and captures the result — pass/fail status, error output, execution time, and any screenshots or logs. The result is added to the loop's results array.

Each iteration is independent — test cases are isolated from each other.

### Stage 3: Test Report (post-loop)

After all test cases have been executed, the reporting step receives the full results array via `{{loop.test-run.results}}`. It produces:

- Summary statistics (pass rate, failure count by severity)
- Critical failures with root cause analysis
- Coverage assessment against the testing scope
- Release readiness recommendation

### Stage 4: Fix Cycle (optional, until_pass)

For critical failures identified in the report, the fix cycle runs an `until_pass` loop:

- Rewrite the failing test with the fix applied
- Execute the test
- Verify the fix — if it passes with no regressions, the cycle ends. If it still fails, the verifier provides guidance for the next attempt.

Maximum 5 fix attempts per failure.

## Inputs

| Name | Required | Description | Example |
|------|----------|-------------|---------|
| `{{input.app_path}}` | Yes | Path to the application codebase or URL | `/home/user/my-webapp` |
| `{{input.testing_scope}}` | Yes | What to test and at what depth | "Full e2e coverage of authentication and user dashboard" |
| `{{input.test_framework}}` | Yes | Testing tools available | "Playwright for e2e, Vitest for unit tests" |
| `{{input.test_command}}` | Yes | Command to run tests | `npx playwright test` |
| `{{input.known_issues}}` | No | Problem areas needing extra attention | "Login flow was refactored last sprint" |

## Outputs

| Name | Description |
|------|-------------|
| Test plan | Structured JSON array of test cases |
| Individual test results | Per-case pass/fail with error details |
| Quality report | Aggregated results with severity ranking and release readiness |
| Fixed tests | Tests rewritten after fix cycle (if applicable) |

## Setup

1. Ensure the project has a working test runner configured
2. If testing e2e, the application should be runnable locally (or accessible at a staging URL)
3. Identify the testing scope — full suite or specific features
4. The AI needs filesystem access and shell execution permission to write and run tests

## Provider Notes

- Test planning works well with any model — it's analysing code structure, not writing complex logic
- Test writing benefits from stronger models (Opus/Sonnet) for accurate assertions
- Each test case in the for_each loop runs 2 AI calls (write + execute). 20 test cases = 40 calls plus planning and reporting
- The fix cycle adds 2-3 calls per attempt, up to 5 attempts per failure
- Shell execution permission is required for running tests

## Example Input

```
app_path: "/home/user/my-express-app"
testing_scope: "Full e2e coverage of the user authentication flow — registration, login, password reset, and session management"
test_framework: "Playwright"
test_command: "npx playwright test"
known_issues: "Password reset email sending is flaky in the test environment — mock the email service"
```
