# Guide Visual Modernization Skill

Modernize one documentation guide at a time by reviewing its visuals in context, deciding which visuals are still necessary, and routing each kept visual to the correct transformation workflow.

This skill is the **orchestrator**. It does not specialize in diagram recreation or screenshot abstraction itself. It decides **what to do**, **what to skip**, and **which specialized skill or tool to invoke**.

## When to Use

Use this skill when working on a documentation guide that contains any combination of:

- old UI screenshots
- conceptual diagrams
- GIFs
- hybrid instructional visuals

Use it when the task is to modernize a guide’s visuals **in context**, not just process isolated image files.

## Core Principle

The goal is to preserve the **instructional value** of the guide, not to preserve every historical image.

A guide should end up:
- clearer
- easier to maintain
- aligned with the current Bkper product
- visually consistent

## Responsibilities

This skill should:

1. Read and understand the guide
2. Inventory all visuals used in the guide
3. Evaluate whether each visual is still necessary
4. Classify each visual by type
5. Decide the modernization path
6. Delegate to the correct specialized skill or tool
7. Update the guide references
8. Verify the guide still renders and explains clearly
9. Report progress clearly for resuming later

## Working Model

### Guide-by-guide execution
Work on **one guide at a time**.

This is important because visual decisions depend on:
- the surrounding text
- the section purpose
- the user task being explained
- nearby visuals that may make another image redundant

### Image-level clarity
Even though execution is guide-by-guide, handle each visual explicitly and report its status clearly.

### Keep original assets unless explicitly asked to clean up
If a visual is removed from the guide, the original file should usually remain in the repository for now.

## Visual Categories

Classify each visual as one of:

1. **GIF**
2. **Diagram**
3. **Real UI screenshot**
4. **Hybrid instructional composition**

### Classification Notes
- Some screenshot-looking visuals are actually diagrams.
- Some diagrams imitate UI structure but are still pedagogical rather than literal screenshots.
- Do not force everything into a screenshot pipeline.
- Do not force screenshot-like hybrids into the diagram pipeline without editorial intent.

## Editorial Decision Gate

Before any transformation work, decide whether the visual should still exist in the guide.

Each visual gets one decision:

- **Remove from guide**
- **Keep and modernize**
- **Replace with text only**
- **Replace with another visual form**
- **Defer**

### Remove when
- it adds little or no explanatory value
- it duplicates another visual nearby
- it shows obsolete UI with no meaningful current equivalent
- it is mostly decorative
- it no longer fits the current PWA workflow
- it costs more to maintain than the explanation value it provides

### Keep when
- it reduces confusion
- it anchors the user in a workflow
- it shows a meaningful state or result
- it clarifies a non-obvious concept
- the explanation would be weaker or more ambiguous without it

### Important warning
Do not over-prune. Some visuals do not add new facts but still reduce user uncertainty. That can still justify keeping them.

## Workflow

### Step 1 — Read the Guide
Understand:
- what the guide teaches
- the main user tasks or concepts
- what role each visual plays in the explanation

### Step 2 — Inventory Visuals
Identify all visuals referenced by the guide:
- imports
- `<img>` usage
- `<Image>` usage
- asset references in MDX or nearby content

### Step 3 — Review Each Visual in Context
For each visual, determine:
- what it explains
- whether it is still necessary
- whether it should be removed, deferred, modernized, or replaced with another format

### Step 4 — Classify Kept Visuals
For each visual that remains in scope, classify it as:
- GIF
- Diagram
- Screenshot
- Hybrid

### Step 5 — Route Each Visual to the Correct Pipeline
Choose the correct specialization path based on type and editorial decision.

### Step 6 — Update the Guide
Remove or replace visual references as needed.

### Step 7 — Verify
Check that:
- the guide renders
- removed visuals are no longer referenced
- new visuals support the explanation
- the guide still makes sense after changes

### Step 8 — Report
Summarize:
- what was removed
- what was modernized
- what was deferred
- what remains ambiguous or blocked

## Delegation Rules

This skill delegates based on visual type and editorial decision.

### If the visual is removed from the guide
- Do **not** invoke transformation skills
- Remove usage from the guide
- Keep the original asset in the repository unless explicitly instructed otherwise

### If the visual is a GIF
- Do **not** modernize in this phase unless explicitly requested
- Mark it as deferred
- Leave current guide usage unchanged unless the user asks for GIF replacement

### If the visual is a real UI screenshot
1. Use browser automation to find the best current PWA equivalent
2. Capture a reference screenshot
3. Invoke **`screenshot-abstraction`**
4. Save or export the resulting croque-style abstraction asset
5. Update the guide to use the new visual

### If the visual is a diagram
1. Invoke **`diagram-excalidraw-conversion`**
2. Refine if needed
3. Run the Excalidraw export tooling
4. Update the guide from PNG to SVG or the new exported asset

### If the visual is a hybrid
1. Decide whether it is better treated as:
   - a diagram
   - or a screenshot abstraction
2. Route it to the corresponding skill
3. If the classification is uncertain, surface the ambiguity rather than forcing a poor choice

## Required Specialized Skills and Tools

### 1. Screenshot abstraction skill
Use for screenshot-derived UI abstractions.

Expected skill:
- `screenshot-abstraction`

### 2. Diagram conversion skill
Use for conceptual diagrams, T-accounts, resource-flow structures, and pedagogical illustrations.

Expected skill:
- `diagram-excalidraw-conversion`

### 3. Browser automation
Use to:
- navigate the current PWA
- find equivalent current states
- capture reference screenshots

Current preferred tool:
- Playwright

### 4. Excalidraw export tooling
Use to export `.excalidraw` outputs into final docs-ready assets.

Current tool:
- `tools/excalidraw-export/`

## Screenshot Mapping Rules

When replacing old screenshots:

### Do
- identify the instructional purpose of the old image
- find the best current PWA state that represents the same task or concept
- use realistic, stable data where possible
- accept approximate equivalence when exact equivalence no longer exists

### Do not
- force a literal legacy-page match
- assume the same UI structure still exists
- preserve historical navigation just because the old image used it

### Mapping confidence labels
When useful, describe the mapping as:
- **Exact equivalent**
- **Approximate equivalent**
- **No direct equivalent**
- **Needs manual editorial decision**

## Hybrid Handling Rules

Treat a visual as **hybrid** when it mixes:
- screenshot-like UI structure
- diagram-like pedagogy
- composed balances, labels, or educational overlays

### Default rule
If the image is instructional rather than product-faithful, prefer the **diagram** path.

### Use screenshot abstraction when
- the guide benefits from grounding in a current PWA state
- the image is still meaningfully a screen, even if slightly stylized

## Completion Criteria

A guide modernization pass is complete when:
- all visuals in the guide were reviewed in context
- unnecessary visuals were removed or deferred
- all kept in-scope visuals were routed through the correct pipeline
- imports and visual references were updated
- the guide renders correctly
- the explanation still works after visual changes

## Reporting Format

At the end of a run, report clearly:

- guide reviewed
- visuals removed from guide
- visuals deferred
- visuals modernized via diagram pipeline
- visuals modernized via screenshot pipeline
- unresolved ambiguities
- next recommended step

## Practical Checklist

Use this checklist while working:

### A. Preparation
- Read the guide fully
- Understand what it teaches
- Identify the visual-heavy sections

### B. Inventory
- List all visuals used in the guide
- Classify each one

### C. Editorial review
- Decide remove / keep / replace / defer for each visual

### D. Route work
- Diagram → `diagram-excalidraw-conversion`
- Screenshot → browser capture + `screenshot-abstraction`
- GIF → defer
- Hybrid → choose path intentionally

### E. Update guide
- remove unused imports
- replace asset references
- keep original unused files unless cleanup is requested

### F. Verify
- run docs locally if available
- confirm rendering and comprehension

### G. Report
- summarize completed work and remaining ambiguities

## Non-Goals

This skill should **not**:
- assume every old image deserves replacement
- automatically modernize GIFs in this phase
- do the detailed diagram drawing logic itself
- do the detailed screenshot abstraction logic itself
- delete original assets unless explicitly requested

## Minimal V1 Success Criteria

Version 1 of this skill is successful if it can reliably:

1. read one guide
2. inventory its visuals
3. decide remove / keep / defer
4. classify kept visuals
5. route each one to the correct pipeline
6. explain clearly what should happen next

## Companion Skills

This orchestrator works best together with:
- `screenshot-abstraction`
- `diagram-excalidraw-conversion`

Use this skill first when the task starts from a **guide**.
Use the companion skills directly when the task starts from a **single already-classified image**.
