# Screenshot Abstraction Skill

Create simplified, abstract representations of UI screenshots using Excalidraw. Replaces text and content with colored bars while preserving layout, hierarchy, and color semantics.

## When to Use

- Creating documentation illustrations without exposing real data
- Building style guides or UI pattern libraries
- Making privacy-safe representations of interfaces
- Generating consistent visual abstractions for presentations

## What is a Screenshot Abstraction?

A screenshot abstraction is a simplified wireframe that:

- **Replaces text** with solid rectangular bars
- **Preserves layout** and spatial relationships
- **Maintains color semantics** (e.g., blue for links, yellow for assets)
- **Shows hierarchy** through indentation and sizing
- **Strips detail** while keeping structure recognizable

## Workflow

### 1. Receive Screenshot

User provides a PNG/JPG screenshot. Analyze:
- Major regions (sidebar, header, content, footer)
- Visual elements (buttons, chips, text blocks, dividers, icons)
- Color semantics (what colors mean in context)
- Hierarchy and nesting

### 2. Generate Excalidraw JSON

Create an `.excalidraw` file with abstract representations:

```
Output: <name>.excalidraw
Canvas: typically 800×550 (adjust to match aspect ratio)
```

### 3. Export to SVG

```bash
cd /Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export
node server.mjs --dir <path-to-excalidraw-files> --headless
```

This exports all `.excalidraw` files without corresponding `.svg` files.

## Style Guide

### Color Palette

| Purpose | Color | Hex |
|---------|-------|-----|
| Window chrome / structural lines | Gray | `#868e96` |
| Text placeholders (light) | Light gray | `#adb5bd` |
| Text placeholders (lighter) | Lighter gray | `#dee2e6` |
| Section headers / dark text | Dark gray | `#868e96` |
| Links / interactive | Blue | `#228be6` |
| Primary buttons | Blue | `#228be6` |
| Traffic light - close | Red | `#ff5f57` |
| Traffic light - minimize | Yellow | `#ffbd2e` |
| Traffic light - maximize | Green | `#28c940` |
| Asset accounts (Bkper) | Yellow | `#fab005` |
| Liability accounts (Bkper) | Yellow | `#fab005` |
| Income/positive (Bkper) | Green | `#228c33` |
| Expense/negative (Bkper) | Red | `#bf4436` |
| Progress bar fill | Green | `#40c057` |

### Element Conventions

| UI Element | Abstraction |
|------------|-------------|
| Window frame | Rectangle with rounded corners, `strokeColor: #868e96` |
| macOS traffic lights | 3 circles (12×12) at top-left: red, yellow, green |
| Text / labels | Solid filled rectangles, height 8-12px |
| Section headers | Darker gray (`#868e96`), slightly taller |
| Buttons | Filled rectangles with semantic color |
| Input fields | Outlined rectangles with placeholder bar inside |
| Chips / tags | Small outlined rounded rectangles |
| Divider lines | Lines with `strokeColor: #868e96` or `#dee2e6` |
| Row separators | Horizontal lines, lighter gray `#dee2e6` |
| Sidebar divider | Vertical line |
| Hierarchy/nesting | Increasing x-offset (indentation) |
| Colored indicators | Small filled squares (10×10) |

### Element Sizing

- Text bars: height 8-12px, width proportional to perceived text length
- Chips/tags: height 14px
- Buttons: height 20-24px
- Traffic lights: 12×12 circles, spaced 20px apart
- Row height: ~40-50px depending on content

### Excalidraw Element Defaults

```json
{
  "strokeStyle": "solid",
  "roughness": 0,
  "opacity": 100,
  "roundness": { "type": 3 },
  "locked": false,
  "isDeleted": false,
  "groupIds": [],
  "frameId": null,
  "boundElements": [],
  "link": null
}
```

For filled shapes (text placeholders):
```json
{
  "strokeColor": "transparent",
  "backgroundColor": "#adb5bd",
  "fillStyle": "solid"
}
```

For outlined shapes (chips, input fields):
```json
{
  "strokeColor": "#868e96",
  "backgroundColor": "transparent",
  "strokeWidth": 1
}
```

For lines:
```json
{
  "type": "line",
  "points": [[0, 0], [width, 0]],
  "strokeColor": "#dee2e6",
  "strokeWidth": 1
}
```

## Excalidraw JSON Structure

Minimal element template:

```json
{
  "id": "unique-id",
  "type": "rectangle",
  "x": 0,
  "y": 0,
  "width": 100,
  "height": 10,
  "angle": 0,
  "strokeColor": "transparent",
  "backgroundColor": "#adb5bd",
  "fillStyle": "solid",
  "strokeWidth": 2,
  "strokeStyle": "solid",
  "roughness": 0,
  "opacity": 100,
  "seed": 1,
  "version": 1,
  "versionNonce": 1,
  "index": "a0",
  "isDeleted": false,
  "groupIds": [],
  "frameId": null,
  "roundness": { "type": 3 },
  "boundElements": [],
  "updated": 1,
  "link": null,
  "locked": false
}
```

File wrapper:

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://app.excalidraw.com",
  "elements": [ /* elements here */ ],
  "appState": {
    "gridSize": null,
    "viewBackgroundColor": "#ffffff"
  },
  "files": {}
}
```

## Reference Samples

Located at: `/Users/jacobvandenberg/Repositories/bkper-mkt/tools/excalidraw-export/samples/`

| File | Description |
|------|-------------|
| `croque-bkper-transactions.excalidraw` | Transactions list page abstraction |
| `croque-bkper-accounts.excalidraw` | Accounts list page abstraction |
| `screenshot-bkper-transactions-light.png` | Source screenshot (light mode) |
| `screenshot-bkper-transactions-dark.png` | Source screenshot (dark mode) |

Study these for style consistency when creating new abstractions.

## Tips

1. **Start with structure** — frame, dividers, major regions first
2. **Match proportions** — keep relative sizes similar to source
3. **Use semantic colors** — preserve meaning (links blue, errors red, etc.)
4. **Vary bar widths** — suggests different text lengths, adds visual interest
5. **Indent for hierarchy** — nested items get increasing x-offset
6. **Group related elements** — use `groupIds` for traffic lights, etc.
7. **Keep it simple** — abstract away detail, keep only what matters for comprehension
