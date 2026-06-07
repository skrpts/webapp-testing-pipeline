# Release Notes

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
Initial catalogue release with full structural and content-quality validation. All scanner checks pass.
