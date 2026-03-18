# Diagram Excalidraw Conversion Skill

Convert conceptual diagrams from Bkper guides into clean, reusable `.excalidraw` source files and exported SVG assets.

This skill is for **diagrams**: T-accounts, resource-flow illustrations, grouped-balance structures, simplified bookkeeping schemas, and other instructional compositions. It is **not** for literal product screenshots.

## When to Use

Use this skill when a guide image is primarily:

- a conceptual bookkeeping diagram
- a T-account illustration
- a resource-flow or balance-flow diagram
- a simplified transaction schema
- a pedagogical composition that explains structure more than UI

Do **not** use this skill when the image is primarily:

- a real product screenshot
- a screenshot-derived UI abstraction (use `screenshot-abstraction` instead)
- a GIF workflow capture

## Core Principle

Recreate the **instructional meaning** of the diagram first.

Preserve:
- accounting meaning
- resource movement
- grouping/hierarchy
- semantic color meaning
- spatial relationships that support explanation

Do **not** optimize for pixel-perfect visual imitation if that would weaken clarity or correctness.

## Lessons from `double-entry-bookkeeping`

The `web/docs/src/content/docs/guides/accounting-principles/fundamentals/double-entry-bookkeeping.mdx` guide established important working rules:

1. **Classify before converting**
   - True diagrams convert well to Excalidraw/SVG.
   - Screenshot-like compositions should usually stay out of the diagram pipeline.

2. **Accounting correctness comes before visual resemblance**
   - T-account structure, left/right meaning, and balance logic matter more than matching every original pixel.

3. **Expect refinement passes**
   - First-pass recreation is often good enough to start, but T-account and bookkeeping diagrams usually need at least one semantic review.

4. **Preserve Bkper semantic colors**
   - Blue = Asset
   - Yellow = Liability
   - Green = Incoming
   - Red = Outgoing

5. **Guide integration is part of the work**
   - The job is not complete when the `.excalidraw` file exists.
   - The exported asset must be produced and the guide should be updated to use it.

6. **Do not force hybrid screenshot-style visuals into this skill**
   - If the image behaves like a stylized UI screen instead of a conceptual diagram, it likely belongs in the screenshot abstraction workflow.

## Diagram Types This Skill Handles Well

### 1. T-Account Diagrams
Examples:
- `advanced-t-account.png`
- `advanced-single-entry.png`
- `advanced-expense-entry.png`
- `advanced-hotel-entry.png`
- `advanced-reimbursement.png`

### 2. Resource / Movement Flow Diagrams
Examples:
- `advanced-few-accounts.png`
- `advanced-detailed-accounts.png`
- `advanced-account-types.png`
- `advanced-equity.png`
- `advanced-net-profit.png`

### 3. Simplified Ledger / Combined Structures
Examples:
- `advanced-overview.png`
- `advanced-bkper-combined.png`
- `advanced-grouped-accounts.png`
- `advanced-simplified-view.png`

## Do Not Use for These Without Explicit Decision

These are often screenshot-like or hybrid and usually belong to the screenshot abstraction pipeline instead:

- `advanced-transactions-view.png`
- `advanced-accounts-view.png`
- `advanced-typed-transactions.png`
- `advanced-typed-accounts.png`
- `advanced-grouped-transactions.png`
- `advanced-grouped-accounts2.png`
- `advanced-final-transactions.png`
- `advanced-final-accounts.png`

## Workflow

### 1. Inspect the Source Image
Analyze the original PNG and determine:
- what concept the diagram teaches
- what structure must remain visible
- what colors are semantic vs decorative
- whether the image is truly a diagram or actually screenshot-like

Ask these questions:
- Is this explaining a bookkeeping concept?
- Is left/right orientation meaningful?
- Are arrows, grouping, or account categories part of the explanation?
- Would converting this into an abstract diagram preserve meaning better than replacing it with a modern screenshot?

If the answer is mostly yes, use this skill.

### 2. Identify the Minimal Semantic Structure
Before drawing, reduce the diagram to its essential teaching elements:

- accounts / nodes
- arrows / movement directions
- debit vs credit sides
- grouped sections
- headings / labels
- transaction blocks
- amount emphasis
- hierarchy and nesting

Do not redraw decorative clutter that does not help explanation.

### 3. Recreate in Excalidraw
Create a new `.excalidraw` file that preserves:
- the explanatory layout
- semantic colors
- relative spacing and grouping
- the key directional or structural logic

Use simple Excalidraw primitives:
- rectangles
- rounded rectangles
- lines
- arrows
- circles / dots where needed
- minimal text only when instructional meaning requires real labels

### 4. Refine for Semantic Accuracy
Review the draft and correct:
- T-account left/right correctness
- movement direction
- grouping logic
- account type color meaning
- spacing/hierarchy
- ambiguity caused by overly literal or overly abstract rendering

This review is mandatory for bookkeeping diagrams.

### 5. Save as `.excalidraw`
Save the source file alongside the original image when appropriate.

Typical pattern:

```text
<name>.png
<name>.excalidraw
<name>.svg
```

### 6. Export Final Asset
Use the export tooling:

```bash
cd /Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export
node server.mjs --dir <path-to-files> --headless
```

Or run the local export server and export through the browser workflow described by the tool docs.

### 7. Update the Guide
If the guide still imports the PNG:
- switch the guide import to the exported SVG
- keep the original PNG in the repo unless explicitly asked to delete it

### 8. Verify
Check that:
- the guide renders correctly
- the diagram still explains the intended concept
- the SVG output is legible
- no screenshot-like visuals were mistakenly forced through the diagram workflow

## Diagram Decision Rules

### Use This Skill When
- the visual teaches accounting structure
- the visual can be understood as a schematic
- semantic relationships matter more than UI fidelity
- the source is already somewhat abstract or pedagogical

### Escalate or Redirect When
- the image is mostly a UI screen
- the image requires realistic current PWA state
- the image is better represented as a croque
- the image is a GIF

## T-Account Rules

T-account diagrams need extra care.

Always preserve:
- header / account title area
- left side = debit
- right side = credit
- vertical center separator when present
- balance accumulation structure where relevant
- entry ordering when it helps explanation

### T-Account Checklist
- Is the account identity clear?
- Is left vs right visually unmistakable?
- Are debit and credit semantics preserved?
- Are entries aligned consistently?
- Are amounts or emphasis bars placed in the correct side?
- Does the result still teach bookkeeping correctly even without the original PNG?

## Color Rules

Use Bkper semantic colors consistently.

| Meaning | Color | Hex |
|--------|-------|-----|
| Asset | Blue | `#228be6` |
| Liability | Yellow | `#fab005` |
| Incoming | Green | `#2f9e44` or `#228c33` |
| Outgoing | Red | `#e03131` or `#bf4436` |
| Structural / neutral text bars | Gray | `#868e96` |
| Secondary / muted text bars | Light gray | `#adb5bd` |
| Borders / dividers | Very light gray | `#dee2e6` |

Notes:
- Prefer consistency within the same guide over mixing near-equivalent shades.
- Use structural gray for neutral shapes, frames, and explanatory bars.
- Do not use pure black for structural elements intended to work on dark backgrounds.

## Style Rules

Follow the Excalidraw diagram style already established in:
- `/Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export/STYLE-GUIDE.md`
- `/Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export/samples/`
- `/Users/jacobvandenberg/Repositories/bkper-mkt/web/docs/src/assets/docs/guides/accounting-principles/`

### Preferred Characteristics
- simple geometry
- consistent spacing
- minimal visual noise
- semantic colors
- enough text only when concept labels are necessary
- export-friendly structure

## Use of Text

Unlike screenshot abstraction, diagrams may require **real text** when the meaning depends on explicit labels.

Examples where real text is appropriate:
- `Debit`
- `Credit`
- `Account`
- `Asset`
- `Liability`
- `Incoming`
- `Outgoing`
- `Equity`
- `Net Profit`

When the label is essential to the concept, keep it.
When the exact text is not important, abstract bars are acceptable.

## File Placement

Default to saving the `.excalidraw` next to the original diagram asset.

Example:

```text
web/docs/src/assets/docs/guides/accounting-principles/
  advanced-overview.png
  advanced-overview.excalidraw
  advanced-overview.svg
```

## Completion Criteria

A diagram conversion is complete when:
- the image was correctly classified as diagram work
- the `.excalidraw` source exists
- semantic meaning is preserved
- the exported SVG was generated
- the guide uses the new output where appropriate
- the result was verified in context

## References

### Primary examples
- `/Users/jacobvandenberg/Repositories/bkper-mkt/web/docs/src/assets/docs/guides/accounting-principles/advanced-overview.excalidraw`
- `/Users/jacobvandenberg/Repositories/bkper-mkt/web/docs/src/assets/docs/guides/accounting-principles/advanced-t-account.excalidraw`
- `/Users/jacobvandenberg/Repositories/bkper-mkt/web/docs/src/assets/docs/guides/accounting-principles/advanced-equity.excalidraw`
- `/Users/jacobvandenberg/Repositories/bkper-mkt/web/docs/src/content/docs/guides/accounting-principles/fundamentals/double-entry-bookkeeping.mdx`

### Supporting docs
- `/Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export/STYLE-GUIDE.md`
- `/Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export/samples/`

## Summary Decision Rule

If the image teaches a concept through a **diagrammatic structure**, use this skill.

If the image teaches through a **current or screenshot-like interface state**, use `screenshot-abstraction` instead.
