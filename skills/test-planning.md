---
type: skill
id: test-planning
title: Test Planning
description: "Analyses an application and generates a structured test plan covering pages, flows, and edge cases"
tags: [Production, Developer, Quality]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Examines an application's structure — routes, components, user flows, and API endpoints — and produces a comprehensive test plan. Each test case is defined with a clear description, preconditions, steps, expected outcome, and priority.

## When to Use

- At the start of a testing cycle to define scope and coverage
- When inheriting an untested codebase and need to prioritise what to test first
- Before a release to ensure critical paths are covered

## Inputs

- Application URL or codebase path
- Testing scope (full suite, specific features, regression only)
- Available testing frameworks
- Known problem areas (optional)

## Outputs

A structured test plan as a JSON array of test cases, each with:
- Test ID, title, and description
- Category (unit, integration, e2e, accessibility, performance)
- Priority (critical, high, medium, low)
- Preconditions and test data requirements
- Step-by-step procedure
- Expected result
