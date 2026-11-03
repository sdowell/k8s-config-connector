---
name: kcc-agentic-journaler
description: Provides a structured logic for capturing and routing agentic learnings to prevent knowledge pollution.
---

# KCC Agentic Journaler

## Overview
As an agent, you must record "tribal knowledge" and technical breakthroughs. This skill ensures that your learnings are stored in the most contextually relevant location.

This is the single journaling policy for every per-resource PR, including every step of the greenfield (`.agents/graphs/kcc-greenfield.yaml`) and brownfield (`.agents/graphs/kcc-brownfield.yaml`) issue graphs.

## Before You Start

Read `.gemini/journals/<service_name>.md` (if it exists) for learnings from earlier resources of the same service. Older per-kind journals under `.gemini/skills/*/journal/` may also be grepped, but do not add new entries there.

## Routing Logic

When you have a new learning, follow this hierarchy to choose the destination:

### Tier 1: General Mechanics
**Scenario**: You found a better way to do something that applies to *all* KCC resources (e.g., "A more efficient way to resolve Project IDs"), or a shortcoming in a skill.
**Destination**: A **"Workflow feedback"** section of your PR description. **Do NOT edit any `SKILL.md` in a per-resource PR**: many agents run the same skill in parallel, and maintainers fold the feedback into the skills in a separate PR.

### Tier 2: Domain and Service Tribal Knowledge
**Scenario**: You found a trick for a specific implementation area, or a quirk unique to a specific GCP service.
**Destination**: **ALWAYS** create or append to `.gemini/journals/<service_name>.md`. 

**CRITICAL**: Do NOT append to any `journal.md` or `journal/` file inside a `.gemini/skills/` directory. Multiple agents working in parallel on different services will cause merge conflicts if they all write to a centralized skill journal. By keeping journals scoped to the `<service_name>`, we eliminate cross-service merge conflicts.

## Journal Entry Template
Use this format for your entries in `.gemini/journals/<service_name>.md`:

```markdown
### [YYYY-MM-DD] <Brief Title>
- **Context**: <What were you implementing? Link to Kind/PR>
- **Problem**: <What was the unexpected behavior or hurdle?>
- **Solution**: <How did you solve it? Provide code snippets if relevant.>
- **Impact**: <Why should the next agent care about this?>
```

## Validation
In your final turn of the task, you MUST state which knowledge was captured and provide the file path.
