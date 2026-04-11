---
type: prompt
id: plan-tests
title: Plan Tests
description: "Analyses an application and generates a structured test plan with prioritised test cases"
tags: [Production, Developer, Quality]
inputs:
  app_path:
    label: "Application Path"
    description: "Path to the application codebase or URL to the running app"
    example: "/home/user/my-webapp"
    required: true
    type: text
  testing_scope:
    label: "Testing Scope"
    description: "What to test — full suite, specific features, regression only, or a particular area"
    example: "Full e2e coverage of the authentication flow and user dashboard"
    required: true
    type: text
  test_framework:
    label: "Testing Framework"
    description: "The testing framework and tools available in the project"
    example: "Playwright for e2e, Vitest for unit tests, Testing Library for components"
    required: true
    type: text
  known_issues:
    label: "Known Issues"
    description: "Problem areas or recently changed features that need extra attention"
    example: "Login flow was refactored last sprint. Payment form has intermittent failures."
    required: false
    type: text
connections:
  - target: test-planning
    type: derived_from
metadata:
  output_format: json
  prompt_type: analysis
---

## Purpose

Drives the test planning skill. Analyses the application and produces a structured, prioritised test plan.

## Prompt

You are a senior QA engineer. Analyse the application below and produce a comprehensive test plan.

### Application

- **Path/URL:** {{input.app_path}}
- **Testing scope:** {{input.testing_scope}}
- **Framework:** {{input.test_framework}}
- **Known issues:** {{input.known_issues}}

### Instructions

1. **Examine the application** — read the source code, identify routes/pages, user flows, API endpoints, and key components
2. **Identify test cases** — for each area, define specific test scenarios covering happy paths, edge cases, error handling, and boundary conditions
3. **Categorise** — label each test as unit, integration, e2e, accessibility, or performance
4. **Prioritise** — rank by business impact: critical (blocks users), high (degrades experience), medium (cosmetic/minor), low (edge case)

### Output Format

Produce a JSON array of test cases:

```json
[
  {
    "id": "TC-001",
    "title": "User can log in with valid credentials",
    "category": "e2e",
    "priority": "critical",
    "description": "Verify the login flow works end-to-end",
    "preconditions": "Test user account exists in the database",
    "steps": [
      "Navigate to /login",
      "Enter valid email and password",
      "Click Submit",
      "Verify redirect to /dashboard"
    ],
    "expected_result": "User is authenticated and sees the dashboard"
  }
]
```

### Rules

- Cover all critical user flows first — login, core features, payment (if applicable)
- Include at least one negative test per flow (invalid input, unauthorised access)
- Include accessibility checks for all interactive elements
- Keep each test case atomic — one scenario per test
- Aim for 15-30 test cases depending on scope

## Formatting Rules

- Use British English throughout
- Test IDs should be sequential: TC-001, TC-002, etc.
- Output must be valid JSON
