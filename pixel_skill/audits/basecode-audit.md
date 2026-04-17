# Audit: Base Code Audit

## Purpose
Check whether TikTok Pixel Base Code is present, correctly initialized, and able to support the target page or flow.

This audit is mandatory for page-level TikTok Pixel implementation tasks.

This audit is not limited to diagnosis.  
If Base Code is missing, incomplete, misplaced, duplicated, or too weak to support reliable event tracking, this audit should also determine whether the implementation must be repaired, supplemented, or replaced with the canonical Base Code template.

## Source of Truth
Use the following resources as the source of truth:

- `resources/tt-guide.md`
  - Use for Base Code initialization expectations, `ttq.load(...)`, `ttq.page()`, and general page coverage expectations.

- `resources/basecode-template.md`
  - Use as the canonical Base Code template source when Base Code is missing, broken, incomplete, or unsuitable for reuse.

## Main Questions
Answer the following:

1. Does TikTok Base Code appear to exist?
2. Is initialization likely placed in a globally effective location?
3. Is the target page or flow actually covered by that initialization?
4. Is `ttq.page()` present or otherwise handled in a valid page-view strategy?
5. Are there risks such as duplicate initialization, partial initialization, page-specific gaps, or weak placement?
6. If Base Code is missing or unreliable, should the current implementation be reused, repaired, supplemented, or replaced with the canonical template?
7. If a canonical template must be used, is the required `pixel_code` already known?

## What to Search
Use repository evidence to search for:
- `ttq`
- `ttq.load`
- `ttq.page`
- `TiktokAnalyticsObject`
- shared layout files
- root application files
- global head injection points
- analytics bootstrap code
- tag manager wrappers
- global script managers

Also search for:
- shared script injection utilities
- analytics bootstrap layers
- application shell files
- route shell files
- existing marketing or analytics initialization code

These may be the correct insertion points if Base Code must be added or repaired.

## What to Check

### A. Base Code Presence
Determine whether TikTok Pixel bootstrap code exists at all.

Signals:
- `ttq.load(...)`
- official TikTok bootstrap snippet
- internal analytics wrapper that ultimately calls TikTok Pixel initialization

If no valid Base Code is found, mark this clearly and prepare to use the canonical template.

### B. Initialization Scope
Determine whether initialization is:
- global,
- page-specific,
- component-specific,
- conditional,
- environment-gated.

Prefer globally stable initialization for site-wide coverage unless the application intentionally scopes Pixel behavior.

If the implementation is too local or conditional to support the target page or flow reliably, treat it as insufficient even if some TikTok code exists.

### C. Placement Correctness
Check whether the implementation is likely inserted in a correct global execution location.

Examples of stronger placements:
- root layout,
- app shell,
- global document head integration,
- shared analytics initializer.

Examples of weaker placements:
- single-page component only,
- lazily mounted UI fragment,
- action-triggered initialization,
- late-loading feature module.

If the placement is weak, determine whether:
- the existing code can be moved,
- the existing code can be wrapped or centralized,
- or the canonical Base Code template should be inserted in a stronger location.

### D. Page Coverage
Determine whether the target page or route is actually covered.

Risks include:
- Base Code only exists in some sub-apps,
- Base Code is only loaded for selected templates,
- Base Code is absent in checkout / confirmation / campaign landing pages,
- Base Code is skipped behind route-specific conditions.

If coverage is partial, decide whether the fix should be:
- expand the current integration,
- relocate the Base Code,
- or install the canonical template at a more global entry point.

### E. PageView Strategy
Check whether page-level tracking exists in a way consistent with the application model.

For traditional multi-page apps:
- `ttq.page()` should usually fire on page load.

For client-side routed apps:
- route transitions may need explicit handling.
- do not assume a single initial page call is enough for all route changes.

If Base Code exists but page-view handling is weak, call this out as a Base Code support problem rather than only an event problem.

### F. Duplicate or Broken Initialization
Check for:
- multiple `ttq.load(...)` calls for the same Pixel,
- repeated bootstrap insertion,
- multiple wrappers competing to initialize the same instance,
- initialization after important user events rather than before them.

If duplication or broken initialization is found, determine whether:
- a local repair is enough,
- the current implementation should be consolidated,
- or the canonical template should replace fragmented initialization patterns.

### G. Template Escalation Decision
If Base Code is missing, clearly broken, weakly placed, or not globally reusable, explicitly decide one of the following:

- **Reuse current Base Code**
- **Repair current Base Code**
- **Supplement current Base Code**
- **Replace with canonical Base Code template**

When replacement or supplementation is recommended, retrieve the canonical template from:
- `resources/basecode-template.md`

If the required `pixel_code` is not already known from the repository, current task context, or prior confirmed user input, ask the user for the TikTok Pixel code before generating the final installable Base Code.

Do not leave the template variable unresolved in the final implementation output.

Do not invent or paraphrase a Base Code snippet when a canonical template resource is available.

## Output Requirements
Summarize findings as:

- **Presence**
- **Scope**
- **Coverage**
- **PageView handling**
- **Risks**
- **Template escalation decision**
- **Pixel code availability**
- **Impact on the target event implementation**

If Base Code is missing or unreliable, also include:
- **recommended insertion location**
- **whether the canonical Base Code template should be used**
- **whether user input is required for `pixel_code`**

## Confidence Labels
Mark conclusions as:
- Confirmed
- Likely
- Needs verification

## Important Rules
- Do not assume Base Code is correct just because `ttq.track(...)` appears somewhere.
- Do not assume global coverage if initialization is only visible in a page or component file.
- Do not treat route-based SPAs as fully covered unless page-view behavior is explicitly considered.
- If Base Code appears weak or partial, call this out before recommending event-level changes.
- If Base Code is missing or not trustworthy, prefer retrieving the canonical Base Code template over improvising a custom bootstrap snippet.
- Do not keep fragmented or low-quality initialization patterns just because some TikTok code already exists.
- If the final code requires `pixel_code` and that value is not known, ask for it before outputting an installable Base Code patch.