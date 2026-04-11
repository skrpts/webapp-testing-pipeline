---
type: skill
id: fix-verification
title: Fix Verification
description: "Verifies that a code fix resolves the failing test and determines whether the fix cycle should continue"
tags: [Production, Developer, Quality]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Acts as the **verifier** in the `until_pass` fix cycle. After each fix attempt, evaluates whether the failing test now passes, whether the fix introduced regressions, and whether the fix cycle should continue.

## When to Use

- As the verifier step in the fix-cycle loop
- When automated test results need human-readable interpretation
- When deciding whether a fix is complete or needs another iteration

## Inputs

- The test execution result (from the previous step)
- The fix that was applied
- The original failure details

## Outputs

A structured JSON verdict:

```json
{
  "status": "pass | continue | fail",
  "reason": "Explanation of what passed or what still fails",
  "instructions": "What to try next (only when status is continue)"
}
```

- `pass` — test passes, no regressions, fix is complete
- `continue` — test still fails or new issues found, with guidance for next attempt
- `fail` — the issue cannot be fixed automatically (e.g. requires architectural change, missing dependency)
