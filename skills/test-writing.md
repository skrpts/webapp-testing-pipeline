---
type: skill
id: test-writing
title: Test Writing
description: "Writes executable test code for a single test case using the project's testing framework"
tags: [Production, Developer, Code]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Takes a single test case from the test plan and writes executable test code using the project's testing framework. Follows existing test patterns in the codebase. Handles setup, assertions, teardown, and test data.

## When to Use

- As the first step in each iteration of the test execution loop
- When a test case has been defined but not yet implemented
- When rewriting a test after a fix to verify the change

## Inputs

- The test case definition (from the test plan or loop item)
- The testing framework and conventions
- The codebase path and relevant source files

## Outputs

- Complete test code ready to execute
- The file path where the test should be saved
- Any test fixtures or data required
