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

4. **Use canonical Bkper semantic colors, not legacy image colors**
   - Blue = Asset
   - Yellow = Liability
   - Green = Incoming
   - Red = Outgoing
   - Gray = structural elements and mixed-type grouping

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
- transaction relationships across accounts
- amount emphasis
- hierarchy and nesting
- semantic account or group role (Asset / Liability / Incoming / Outgoing / mixed-type / neutral)

Do not redraw decorative clutter that does not help explanation.

### 3. Normalize Semantic Colors Before Drawing
Before creating Excalidraw elements, decide the intended semantic color of each meaningful element.

For each account, node, or grouped result, ask:
- Is this an **Asset**?
- Is this a **Liability**?
- Is this **Incoming**?
- Is this **Outgoing**?
- Is this a **mixed-type grouping** such as Equity or Net Profit shown as a grouped result?
- Is this merely **structural / neutral**?

Then apply the canonical Bkper color mapping rather than copying the original PNG tint.

### 4. Recreate in Excalidraw
Create a new `.excalidraw` file that preserves:
- the explanatory layout
- semantic colors
- relative spacing and grouping
- the key directional or structural logic
- the established shape language of related diagrams in the same documentation family

When the surrounding reference diagrams use square blocks, set rectangle `roundness` to `null` instead of using rounded corners.

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

## Mixed-Type Grouping Rules

Use **gray** when a grouped result combines more than one account-type family in a way that Bkper represents as a neutral grouped total.

Typical examples:
- **Equity** shown as a grouped result of Assets + Liabilities
- **Net Profit** shown as a grouped result of Incoming + Outgoing
- cross-type parent groups used to summarize multiple semantic families

Important distinction:
- A normal single-type group should keep its semantic family color where that improves understanding.
- A cross-type result or summary should use **gray** to show it is a grouped neutral outcome rather than a single account type.

## T-Account Rules

T-account diagrams need extra care.

Always preserve:
- header / account title area
- left side = debit
- right side = credit
- vertical center separator when present
- balance accumulation structure where relevant
- entry ordering when it helps explanation

### Transaction-Across-T-Accounts Rule

When the same transaction is represented across two or more T-accounts, think in **participating debit/credit half-cells**, not in total account count.

For each movement:
- identify which half-cell participates in each T-account
- place the entry text fully inside that participating half-cell first
- keep each entry block inside its own debit/credit side and away from the center divider and outer border
- then draw **one gray dashed rounded rectangle per movement** around the union of those related entry blocks
- allow that shared rounded rectangle to span across account gaps when needed
- but keep the shared rounded rectangle inside the participating half-cells only

This rule applies equally whether the diagram uses **2, 3, or 4 T-balances**.

If the shared rounded rectangle does not fit cleanly:
- widen the account boxes
- and/or increase spacing between accounts
- and/or rebalance the composition

Do **not**:
- draw a separate dashed rounded rectangle around each individual entry
- let the shared rounded rectangle spill into non-participating debit/credit halves
- keep a cramped original layout if that forces text or the shared rounded rectangle to overflow

The visual unit is the **movement across participating half-cells**, not the number of accounts on the canvas.

### T-Account Checklist
- Is the account identity clear?
- Is left vs right visually unmistakable?
- Are debit and credit semantics preserved?
- Are entries aligned consistently inside the correct half-cell?
- Does each shared transaction envelope cover the related entries for one movement only?
- Does the shared transaction envelope stay inside the participating half-cells only?
- If the envelope did not fit, was the layout widened instead of allowing overflow?
- Are amounts or emphasis bars placed in the correct side?
- Does the result still teach bookkeeping correctly even without the original PNG?

### T-Account Shared Transaction Envelope Benchmarks

For the preferred transaction-span pattern, study:

- `/Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export/samples/benchmark-t-account-shared-transaction-envelope.excalidraw`
- `/Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export/samples/accounting-principles-2-bad-good.excalidraw`

These benchmarks demonstrate the correct pattern when one movement is represented across T-accounts:

- entry text stays inside the correct T-balance half-cell
- the shared **gray dashed rounded rectangle** spans the related descriptions across participating T-accounts
- the dashed rounded rectangle represents the **shared movement**, not an individual entry box
- use one rounded dashed transaction envelope per movement
- if needed, widen the account layout so the shared rounded rectangle fits correctly

The `accounting-principles-2-bad-good` comparator is especially important because it shows the failure mode to avoid:
- a shared rounded rectangle that overflows into non-participating halves
- entry text placement derived too loosely from the full account instead of the participating half-cell

When in doubt, match these benchmarks more closely than the source PNG.

## Color Rules

Use **Bkper semantic colors** consistently, with the Bkper design system as the source of truth.

Reference palette authority:
- `https://bkper.app/design/v2/style.css`
- local guidance already used in the repo:
  - `/Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export/STYLE-GUIDE.md`

### Semantic Color Priority

Choose colors in this order:

1. **Accounting meaning** of the node / account / group
2. **Guide context** and nearby explanatory text
3. **Canonical Bkper design-token palette**
4. **Original source-image color** only as a fallback hint when meaning is ambiguous

Do **not** preserve legacy PNG colors if they conflict with the intended Bkper semantic color.

### Canonical Mapping

| Meaning | Color | Hex |
|--------|-------|-----|
| Asset | Blue | `#228be6` |
| Liability | Yellow | `#fab005` |
| Incoming | Green | `#2f9e44` or `#228c33` |
| Outgoing | Red | `#e03131` or `#bf4436` |
| Mixed-type grouping (`Asset + Liability`, `Incoming + Outgoing`) | Gray | `#868e96` |
| Structural / neutral text bars | Gray | `#868e96` |
| Secondary / muted text bars | Light gray | `#adb5bd` |
| Borders / dividers | Very light gray | `#dee2e6` |

### Semantic Interpretation Rules

When converting a diagram, infer the role of each element before assigning color.

Examples:
- **Bank**, **Cash**, **Inventory**, **Receivables** → usually **Asset** → blue
- **Loans**, **Providers**, **Payables**, **Taxes payable** → usually **Liability** → yellow
- **Revenue**, **Service**, **Sales**, **Income** → **Incoming** → green
- **Expenses**, **Transport**, **Gasoline**, **Rent**, **Fees** → **Outgoing** → red
- **Equity**, **Net Profit**, or any group mixing two account-type families → gray when shown as cross-type grouping
- Neutral frames, arrows, dividers, label bars, and layout scaffolding → gray / light gray

If semantic meaning is clear from the guide or diagram labels, normalize to the canonical palette even if the original image used a different tint.

If semantic meaning is unclear:
- prefer neutral gray
- or surface the ambiguity rather than guessing aggressively

Notes:
- Prefer consistency within the same guide over mixing near-equivalent shades.
- Use structural gray for neutral shapes, frames, explanatory bars, and mixed-type grouped totals.
- Do not use pure black for structural elements intended to work on dark backgrounds.

## Style Rules

Follow the Excalidraw diagram style already established in:
- `/Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export/STYLE-GUIDE.md`
- `/Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export/samples/`
- `/Users/jacobvandenberg/Repositories/bkper-mkt/web/docs/src/assets/docs/guides/accounting-principles/`

### Style Consistency Rule

When a guide domain already has an established diagram family, match that family’s visual language.

For example, in the accounting-principles guides, files like:
- `advanced-few-accounts.excalidraw`
- `advanced-detailed-accounts.excalidraw`
- `advanced-account-types.excalidraw`

establish a preferred style for account-flow and T-account-adjacent diagrams:
- **square account blocks**
- strong semantic headers
- clearly tinted semantic bodies
- minimal neutral structure

Do **not** introduce a different shape language such as heavily rounded account containers if the surrounding guide family uses square blocks.

### Preferred Characteristics
- simple geometry
- consistent spacing
- minimal visual noise
- semantic colors
- enough text only when concept labels are necessary
- export-friendly structure
- shape consistency with nearby related diagrams

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
