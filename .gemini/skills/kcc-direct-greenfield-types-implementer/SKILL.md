---
name: kcc-direct-greenfield-types-implementer
description: Entry point for KCC greenfield types generation and judgement-queue triage. Routes between Step 1a (`prepopulate/SKILL.md` for initial mechanical `--prepopulate-resource` scaffolding) and Step 1b (`judgement/SKILL.md` for `judgement_queue.yaml` triage and finalization).
---

# KCC Direct Greenfield Types Implementer (Router)

This skill is the entry point for **Greenfield Step 1a (Prepopulate)** and **Greenfield Step 1b (Judgement Queue Triage)**. It routes to one of two sub-workflows depending on whether `<Kind>` has already been prepopulated in `master` (`apis/<service>/judgement_queue.yaml` and `apis/<service>/<version>/<kind_lowercase>_types.go`).

## Prerequisites
You **must** always apply the baseline KRM standards in:
- [`.gemini/skills/kcc-direct-base-types-implementer/SKILL.md`](file:///usr/local/google/home/lmadariaga/github/k8s-config-connector/.gemini/skills/kcc-direct-base-types-implementer/SKILL.md)
- [`.gemini/skills/kcc-identity-reference/SKILL.md`](file:///usr/local/google/home/lmadariaga/github/k8s-config-connector/.gemini/skills/kcc-identity-reference/SKILL.md)

## Detect Mode and Route

**If the issue has a step label, route by it** (the `kcc-greenfield` issue graph sets one on every step):
- `step/gen-types` (Step 1a): read and follow **[`prepopulate/SKILL.md`](prepopulate/SKILL.md)**. Its Phase 0 handles a `<Kind>` that is already prepopulated; do **not** switch to Step 1b in that case.
- `step/types-judgement` (Step 1b): read and follow **[`judgement/SKILL.md`](judgement/SKILL.md)**. Its Phase 0 handles a `<Kind>` already finalized by a legacy Step 1 PR.

**Otherwise**, check whether `<Kind>` already has an entry in `apis/<service>/judgement_queue.yaml` and `apis/<service>/<version>/<kind_lowercase>_types.go` on `master`:

```bash
git show origin/master:apis/<service>/judgement_queue.yaml 2>/dev/null | grep -q "kind: <Kind>"
```

- **If `<Kind>` is NOT yet in `apis/<service>/judgement_queue.yaml` on `master` (Step 1a / `step/gen-types`: Prepopulate Resource & Judgement Queue)**:
  - Read and follow **[`.gemini/skills/kcc-direct-greenfield-types-implementer/prepopulate/SKILL.md`](prepopulate/SKILL.md)** using `view_file`.
- **If `<Kind>` ALREADY exists in `apis/<service>/judgement_queue.yaml` on `master` (Step 1b / `step/types-judgement`: Triage Judgement Queue & Finalize KRM Types)**:
  - Read and follow **[`.gemini/skills/kcc-direct-greenfield-types-implementer/judgement/SKILL.md`](judgement/SKILL.md)** using `view_file`.

## Before Opening the PR (Step 1a and Step 1b)

1. Validate locally with `scripts/validate-prereqs.sh`. If it fails, its output contains hints for fixing the errors.
2. Journal per `.gemini/skills/kcc-agentic-journaler/SKILL.md`: don't edit any `SKILL.md`; general learnings go in a "Workflow feedback" section of the PR description, service-specific notes in `.gemini/journals/<service_name>.md`.
3. Include the release note block set to NONE in the PR description:
   ````
   ```release-note
   NONE
   ```
   ````

