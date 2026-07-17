---
type: asset
id: test-plan-template
title: Test Plan Template
description: "Structured template for organizing test cases by category and priority"
tags: [Production, Developer, Quality]
connections: []
metadata:
  asset_type: "template"
  format: "markdown"
---

# Test Plan

## Project

- **Application:** [Name and URL/path]
- **Scope:** [What is being tested and why]
- **Framework:** [Testing tools and versions]
- **Date:** [When the plan was created]

## Test Case Categories

| Category | Description | Tools |
|----------|-------------|-------|
| **Unit** | Individual function/component logic | Vitest, Jest |
| **Integration** | Component interactions, API contracts | Testing Library, Supertest |
| **E2E** | Full user flows through the browser | Playwright, Cypress |
| **Accessibility** | WCAG compliance, screen reader compatibility | Axe, Lighthouse |
| **Performance** | Load times, bundle size, API latency | Lighthouse, k6 |

## Priority Levels

| Priority | Meaning | Example |
|----------|---------|---------|
| **Critical** | Blocks core functionality — users cannot complete primary tasks | Login fails, payment broken |
| **High** | Significantly degrades experience — workaround may exist | Search returns wrong results, slow page load |
| **Medium** | Noticeable issue but doesn't block workflows | Formatting inconsistency, missing validation message |
| **Low** | Edge case or cosmetic issue | Tooltip misaligned on one browser |

## Test Cases

| ID | Title | Category | Priority | Status |
|----|-------|----------|----------|--------|
| TC-001 | [Test description] | e2e | critical | pending |
| TC-002 | [Test description] | unit | high | pending |

## Coverage Targets

- All critical user flows: 100% coverage
- API endpoints: at least one happy path + one error case each
- Form inputs: valid, invalid, empty, and boundary values
- Authentication: login, logout, session expiry, unauthorised access
- Responsive: at least desktop + mobile breakpoints for key pages
