---
type: skill
id: test-reporting
title: Test Reporting
description: "Aggregates test results into a severity-ranked report with failure analysis and recommendations"
tags: [Production, Developer, Reporting]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Takes the full set of test results from the test execution loop and produces a comprehensive test report. Groups results by status and category, analyzes failure patterns, and prioritizes issues by severity and user impact.

## When to Use

- As the post-loop step after the test execution cycle completes
- When you need a summary of test coverage and results for stakeholders
- Before a release decision to assess quality

## Inputs

- All test results from the loop (via `{{loop.test-run.results}}`)
- The original test plan for coverage analysis
- The loop status and iteration count

## Outputs

A structured test report:
- **Summary** — total tests, pass/fail/error/skip counts, overall pass rate
- **Failures by severity** — critical failures first, with root cause analysis
- **Coverage assessment** — which areas are well-tested vs undertested
- **Patterns** — recurring failure types, flaky tests, timeout issues
- **Recommendations** — what to fix first, what needs more testing, release readiness assessment
