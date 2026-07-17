# Release Notes

## v1.0.21
GH#845 — republish with American English (en-US) content, completing the source-only GH#805 flip that never reached the Hub. Copy only — no functional or behaviour change.

## v1.0.20
GH#745 — declare per-step `output: {name, type}` on every execution step (test_plan/text, tests/text, test_output/text, test_report/text, fix_verdict/decision). Lights up the #744 rich flow-map. Content-only; no bindings or logic changes.

## v1.0.19
GH#655 audit fix — rewrites `{{steps.Test Planning.output}}` → `{{steps.Plan Tests.output}}` in the workflow's `inputExpression` for the for-each loop. The `test-planning` slug was mirror-dropped to `plan-dev-tests` (title "Plan Tests") in Step 2; the Row 3b helper rewrote slug refs but missed display-name uses in this consumer. Engine validator would not have caught this (loop's inputExpression is runtime-substituted, not statically validated against priorTitles).

## v1.0.18
Fix-forward after Row 3b v1.0.17 publish failure. The v1.0.17 per-skrpt CI's "Register version with Hub API" step failed because the consumer's source `manifest.id` (b8e4f21a…) did not match the D1 catalog row's id (41ed5303…) — a legacy drift from before Action 6 (`0bcc5ae0`) made publish-skrpt.mjs Step 2 INSERT use `manifest.id` for the D1 id column. v1.0.18 reconciles the source `manifest.id` to the catalog authoritative value (Row-5-equivalent for consumers) and republishes. Per Adj-1: no re-tag of v1.0.17; the orphaned GitHub release artefact stays inert (no D1 versions row, no consumer pinned it).

## v1.0.17
GH#645 Row 3b — migrate to K-037 dep-referenced schema. Strip 4 inline shared-content files and declare 3 hub-shared deps (UUID id + slug name + version + checksum from `gen-dep-checksums.mjs`). Internal slug references rewritten for E2 rename/mirror-drop pair(s): test-planning→plan-dev-tests, test-execution→run-test, plan-tests→plan-dev-tests. Closes pre-Step-3 inline-vendoring for this bundle.

## v1.0.16
Wave 2: re-signed with canonical engine signing pipeline.

## v1.0.15
Tags migrated inline into manifest (GH#586). tags.yaml retired.

## v1.0.14
Bundle re-signed with canonical engine signing pipeline (Wave 2 migration).

## v1.0.13
Signature fix — RELEASE_NOTES.md now included in integrity checksum.

## v1.0.12
Initial catalog release with full structural and content-quality validation. All scanner checks pass.
