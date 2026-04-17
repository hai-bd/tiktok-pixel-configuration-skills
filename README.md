# Directory Structure
```
├── pixel_skill/
│   ├── SKILL.md
│   ├── resources/
│   │   ├── tt-guide.md
│   │   ├── meta-map.md
│   │   └── basecode-template.md
│   ├── workflows/
│   │   └── implement-page-event.md
│   ├── audits/
│   │   ├── basecode-audit.md
│   │   ├── event-audit.md
│   │   └── meta-alignment-audit.md
│   ├── modules/
│   │   ├── repo-search-strategy.md
│   │   ├── trigger-timing-reasoning.md
│   │   ├── parameter-mapping.md
│   │   ├── code-patch-strategy.md
│   │   └── confidence-classification.md
│   └── outputs/
│       └── implementation-output.md
```

# TikTok Pixel Implementation Skill

## What this skill does
This skill helps analyze and implement TikTok Pixel in a code repository.

It is designed for tasks such as:

- installing TikTok Pixel Base Code,
- validating whether TikTok Pixel is correctly implemented,
- configuring TikTok Pixel events for a page or business flow,
- checking event names, parameters, and trigger timing,
- migrating or aligning Meta Pixel logic to TikTok Pixel,
- generating concrete code-level implementation guidance.

This skill is intended for implementation work in code, pages, or business flows.
It is not intended as a generic conceptual reference unless the task clearly involves implementation.

## Core Design
This skill does not treat Base Code checking, TikTok event checking, and Meta alignment as separate scenarios.

Instead, it uses:

- one main workflow,
- three coupled audit tracks,
- reusable reasoning modules,
- canonical resource files,
- one final implementation output template.

For page-level or flow-level TikTok Pixel tasks, these three checks should normally run together:

1. Base Code audit
2. TikTok event audit
3. Meta alignment audit

The goal is not only to identify what is missing or incorrect, but to turn the findings into a technically grounded implementation plan.

## Directory Structure

### `SKILL.md`
Main skill entry point.

Defines:

- when the skill should trigger,
- the main workflow,
- required execution order,
- final output requirements,
- implementation constraints.

This file should remain the routing and execution-control document, not a long knowledge dump.

### `resources/`
Knowledge sources.
These files act as the rule base for implementation judgments.

- `tt-guide.md`
  - TikTok Pixel usage rules and event configuration guidance.
- `meta-map.md`
  - Meta-to-TikTok migration and parameter mapping guidance.
- basecode-template.md
  - Canonical TikTok Base Code template.
  - Used when Base Code is missing, broken, incomplete, or unsuitable for reuse.
  - Contains a required runtime placeholder: {{PIXEL_CODE}}.
  - If pixel_code is not already known, the agent should ask for it before generating final installable Base Code.


### `workflows/`
Main workflow definitions.

- `implement-page-event.md`
  - Main execution flow for implementing or validating TikTok Pixel for a page or business flow.
  - Controls the high-level sequence:
    - understand the target page or flow,
    - identify the target event,
    - run Base Code audit,
    - run TikTok event audit,
    - run Meta alignment audit,
    - merge findings,
    - provide code-level guidance,
    - include validation and uncertainty notes.

### `audits/`
Mandatory coupled audits.

- `basecode-audit.md`
  - Checks whether TikTok Base Code exists, is globally valid, and covers the target page or flow.
  - Decides whether the current Base Code should be reused, repaired, supplemented, or replaced with the canonical template.
  - If canonical Base Code must be used and pixel_code is unknown, the agent should ask for it before producing final installable Base Code.
- `event-audit.md`
  - Checks whether the target TikTok event is correctly implemented.
  - Includes event presence, event naming, parameters, trigger timing, deduplication, and identify relevance when needed.
- `meta-alignment-audit.md`
  - Checks whether existing Meta Pixel implementation can be reused as a migration source or implementation clue.
  - Includes trigger point reuse, parameter reuse, API differences, and migration safety.

### `modules/`
Reusable reasoning modules.

- `repo-search-strategy.md`
  - Defines how to search the repository for TikTok code, Meta code, analytics abstractions, business trigger points, and parameter sources.
- `trigger-timing-reasoning.md`
  - Defines how to judge whether an event fires at the correct business moment rather than only at a UI interaction point.
- `parameter-mapping.md`
  - Defines how repository-visible data and Meta Pixel parameters should be mapped into TikTok-compatible payloads.
- `code-patch-strategy.md`
  - Defines how to convert audit findings into maintainable code-level implementation guidance.
  - Enforces an important dependency rule: if Base Code is not trustworthy, fix Base Code first before treating event-level patches as implementation-ready.
- `confidence-classification.md`
  - Defines how to classify findings as:
    - Confirmed
    - Likely
    - Needs verification

### `outputs/`
Final output structure.

- `implementation-output.md`
  - Defines the default final answer structure.
  - The final output should usually contain:
    - task understanding,
    - recommended TikTok event,
    - Base Code findings,
    - TikTok event findings,
    - Meta alignment findings,
    - implementation gaps,
    - recommended code changes,
    - example code or patch sketch,
    - validation steps,
    - uncertainty or risk notes.

## Expected Skill Behavior
When the user asks to configure, validate, repair, migrate, or implement TikTok Pixel for a page or business flow, the skill should:

- understand the page, business action, and target business moment,
- determine the most appropriate TikTok event,
- check Base Code,
- check TikTok event correctness,
- inspect existing Meta implementation,
- merge the findings into one implementation view,
- provide concrete code-level guidance,
- explain validation steps and uncertainty.

If Base Code must be installed or replaced and the required pixel_code is not yet known, the skill should ask for it before generating final installable Base Code.

## Important Notes
- This skill should not stop at conceptual advice when code-level guidance is possible.
- This skill should distinguish confirmed findings from inferred ones.
- This skill should prefer maintainable patch locations over scattered UI-only tracking code.
- This skill should use resource documents as the rule source of truth.
- This skill should not treat event patching as complete if Base Code is still unresolved.
- This skill should not output final installable Base Code with unresolved {{PIXEL_CODE}}.
- This skill should not mechanically copy Meta implementation when TikTok-specific differences matter.