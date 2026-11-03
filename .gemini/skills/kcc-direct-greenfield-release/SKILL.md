---
name: kcc-direct-greenfield-release
description: Final step of the KCC Greenfield pipeline. Releases a greenfield Kind by removing it from BLOCKED_KINDS in dev/tasks/build-release-bundle once every fixture has real-GCP, audit-probe and mock logs.
---

# KCC Direct Greenfield Release

Use this skill for a `Greenfield: Release <Kind> (remove it from BLOCKED_KINDS)` issue: the `finally` step of the `.agents/graphs/kcc-greenfield*.yaml` graphs (Step 5 of `.agents/workflows/kcc-greenfield.txt`). Every other step for `<Kind>` is done. The PR is reviewed with `.gemini/skills/reviewgen-greenfield-release/SKILL.md`.

## Workflow

1. **Check the fixture evidence.** Every fixture of `<Kind>` under `pkg/test/resourcefixture/testdata/basic/` must have a real-GCP `_http.log`, an `_audit_probe.log` and an `_http_mock.log`. If any is missing, **don't change anything**: make no commit, and list what's missing.
2. **Unblock the Kind.** Remove `"<Kind>"` from the `BLOCKED_KINDS=( ... )` array in `dev/tasks/build-release-bundle`. Change nothing else.
3. **Open the PR** with this release note block in the description:
   ````
   ```release-note
   New Alpha Resources (Direct Reconciler): `<Kind>`
   ```
   ````
