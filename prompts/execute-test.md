---
type: prompt
id: execute-test
title: Execute Test
description: "Runs a test case and records the result with structured pass/fail output"
tags: [Production, Developer, Automation]
inputs:
  app_path:
    label: "Application Path"
    description: "Path to the application codebase"
    example: "/home/user/my-webapp"
    required: true
    type: text
  test_command:
    label: "Test Command"
    description: "The command to run tests"
    example: "npx playwright test or npm test"
    required: true
    type: text
connections:
  - target: run-test
    type: derived_from
metadata:
  output_format: json
  prompt_type: core
---

## Purpose

Drives the test execution skill. Runs the test written by the previous step and records the result.

## Prompt

You are a test runner. Execute the test from the previous step and record the result.

### Project

- **Path:** {{input.app_path}}
- **Test command:** {{input.test_command}}

### Test to Run

{{steps.previous.output}}

### Instructions

1. **Run the test** — execute the test file using the project's test runner
2. **Capture output** — record stdout, stderr, exit code, and execution time
3. **Parse results** — determine pass/fail status from the runner output
4. **Record details** — if failing, capture the assertion error, stack trace, and any screenshots

### Output Format

Produce a structured JSON result:

```json
{
  "test_id": "TC-001",
  "title": "User can log in with valid credentials",
  "status": "pass | fail | error | skip",
  "duration_ms": 1234,
  "error": null,
  "error_message": "Expected 200 but received 401",
  "stack_trace": "...",
  "output": "Test runner stdout"
}
```

### Rules

- Run the test exactly as written — do not modify the test code
- If the test fails, report the failure accurately — do not retry or fix
- If the test cannot run (missing dependency, syntax error), report status as "error"
- Capture the full error output, not just the first line

## Formatting Rules

- Output must be valid JSON
- Use British English in any narrative text
