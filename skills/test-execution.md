---
type: skill
id: test-execution
title: Test Execution
description: "Runs a test case and records the result with pass/fail status, error output, and timing"
tags: [Production, Developer, Automation]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Executes a written test using the project's test runner. Captures the full output — pass/fail status, error messages, stack traces, assertion failures, and execution time. Produces a structured result that can be aggregated across all test cases.

## When to Use

- As the second step in each iteration of the test execution loop
- After writing or rewriting a test
- When re-running a test after a code fix

## Inputs

- The test file to execute
- The test command and framework
- The project path

## Outputs

A structured test result:
- Test ID and title
- Status (pass, fail, error, skip)
- Execution time
- Error output (if failing) — full message, stack trace, assertion details
- Screenshots or logs (if applicable)
