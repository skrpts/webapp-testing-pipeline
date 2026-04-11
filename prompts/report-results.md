---
type: prompt
id: report-results
title: Report Test Results
description: "Aggregates test results into a severity-ranked quality report"
tags: [Production, Developer, Reporting]
inputs:
  testing_scope:
    label: "Testing Scope"
    description: "The original testing scope for coverage assessment"
    example: "Full e2e coverage of authentication and dashboard"
    required: true
    type: text
connections:
  - target: test-reporting
    type: derived_from
metadata:
  output_format: markdown
  prompt_type: synthesis
---

## Purpose

Drives the test reporting skill. Runs after the test execution loop. Aggregates all test results into a comprehensive quality report.

## Prompt

You are a QA lead producing a test report for stakeholders. Analyse the test results below and produce a comprehensive quality assessment.

### Test Results

{{loop.test-run.results}}

### Loop Status

- **Status:** {{loop.test-run.status}}
- **Tests executed:** {{loop.test-run.iterations}}

### Testing Scope

{{input.testing_scope}}

### Instructions

Produce a report with these sections:

**1. Summary**

| Metric | Value |
|--------|-------|
| Total tests | [count] |
| Passed | [count] ([%]) |
| Failed | [count] ([%]) |
| Errors | [count] |
| Skipped | [count] |
| Pass rate | [%] |

**2. Critical Failures**

For each failing test with priority "critical" or "high":
- What failed and why
- Root cause analysis
- User impact
- Recommended fix

**3. All Failures**

| Test ID | Title | Category | Priority | Error | Suggested Fix |
|---------|-------|----------|----------|-------|---------------|

**4. Coverage Assessment**

What areas are well-tested vs undertested relative to the testing scope.

**5. Release Readiness**

Based on the results: ready to release, needs fixes first, or needs more testing. Be specific about what blocks release.

## Formatting Rules

- Use British English throughout
- Lead with the worst news — critical failures first
- Be specific about root causes, not just "test failed"
- Keep the summary readable by non-technical stakeholders
