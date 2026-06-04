# Flow Diagram Skill Guide

Use this guide to create professional, consulting-style data flow and process flow diagrams as a single self-contained HTML file.

This skill is suitable for:
- Data flow diagrams showing movement of information across systems
- Process flow diagrams showing ordered business, operational, or technical steps
- Hybrid diagrams where a process step also represents a system, service, queue, database, or control point

## Core Requirements

Generate one complete `.html` file using only:
- Pure HTML
- Inline SVG
- Embedded CSS
- Vanilla JavaScript

Do not use:
- Mermaid
- Canvas
- External JavaScript libraries
- External CSS frameworks
- External SVG `<image href="...">` references
- External icon registries, CDNs, image URLs, fonts, or stylesheets

All diagram visuals, components, process steps, decision points, connectors, arrows, and flow effects must be implemented with inline SVG.

## Visual Style

Treat visual style as input-driven. Honor any user-provided theme, brand colors, font, density, layout direction, annotation style, and alignment requirements.

If the user does not provide styling inputs, default to a light theme with a polished consulting presentation style:
- Page background: `#F7F8FC`
- Diagram surface: `#FFFFFF`
- Component fill: white or very light tinted fills
- Text: `#111827`
- Secondary text: `#5B6170`
- Borders: `#DDE2EB`
- Primary highlight: blue, teal, or another restrained professional accent
- Flow highlight: a stronger accent color used only for active paths and animated strokes

Expose styling through CSS custom properties instead of hardcoded values:

```css
:root {
  --primary: #2563EB;
  --accent: #0F766E;
  --page-bg: #F7F8FC;
  --canvas-bg: #FFFFFF;
  --surface: #FFFFFF;
  --ink: #111827;
  --muted: #5B6170;
  --line: #DDE2EB;
  --warning: #B45309;
  --success: #047857;
  --radius: 12px;
  --node-width: 210px;
  --node-height: 120px;
  --flow-speed: 1.2s;
}
```

Avoid:
- Overriding explicit user-provided colors with defaults
- Neon palettes
- Decorative gradients as the main visual language
- Overly rounded cards
- Excessive shadows
- One-color themes where everything is a variation of the same hue, unless the user explicitly asks for monochrome

## Input Options

The generated diagram should adapt to structured or plain-language inputs.

Supported input-driven options:
- `theme`: `light`, `dark`, `brand`, `monochrome`, or explicit color tokens
- `orientation`: `left-to-right`, `right-to-left`, `top-to-bottom`, or `bottom-to-top`
- `alignment`: `grid`, `lanes`, `phases`, `centerline`, `timeline`, or explicit coordinates
- `density`: `compact`, `standard`, or `spacious`
- `nodeShape`: rounded rectangles by default, with optional diamonds or terminal pills when allowed
- `annotations`: callouts, notes, risks, assumptions, metrics, ownership, SLA, controls, or numbered markers
- `connectionStyle`: curved, orthogonal, straight, dashed, batch, stream, control, exception, or feedback
- `detailsPanel`: right, left, bottom, collapsible, or hidden when the user requests static output
- `export`: none or SVG
- `animation`: on, off, reduced, or active-path-only

Recommended configuration shape:

```js
const config = {
  theme: {
    primary: "#2563EB",
    accent: "#0F766E",
    pageBg: "#F7F8FC",
    canvasBg: "#FFFFFF",
    surface: "#FFFFFF",
    ink: "#111827",
    muted: "#5B6170",
    line: "#DDE2EB"
  },
  orientation: "left-to-right",
  alignment: "lanes",
  density: "standard",
  annotations: "callouts",
  connectionStyle: "curved",
  detailsPanel: "right",
  export: "SVG",
  animation: "active-path-only"
};
```

When no option is supplied, choose conservative defaults that fit the content. Do not make the generated diagram depend on one fixed color palette, one fixed orientation, or one fixed alignment model.

## Diagram Mode

Choose the diagram mode from the user's request:
- Use `data-flow` mode when the diagram emphasizes systems, platforms, stores, queues, APIs, pipelines, and movement of information.
- Use `process-flow` mode when the diagram emphasizes sequential activities, decisions, approvals, handoffs, exceptions, and outcomes.
- Use `hybrid-flow` mode when both system components and process steps matter.

If the user does not specify the mode, infer it from the nouns:
- "Source", "API", "database", "warehouse", "stream", "file", "service", "event", "payload", and "sink" usually imply data flow.
- "Step", "review", "approval", "decision", "handoff", "exception", "SLA", "owner", and "outcome" usually imply process flow.

Keep the generated HTML dynamic by storing node and connection metadata in JavaScript arrays or objects. The SVG elements may be written directly in HTML for simple diagrams, but the details panel, active state, and connection highlighting should be driven from structured data.

Recommended metadata shape:

```js
const diagram = {
  mode: "process-flow",
  nodes: {
    intake: {
      type: "start",
      title: "Request Intake",
      description: "Collect the submitted request and required context.",
      responsibilities: ["Capture request", "Validate required fields"],
      inputs: ["Submitted form"],
      outputs: ["Qualified request"],
      owner: "Operations"
    },
    review: {
      type: "decision",
      title: "Review Required?",
      description: "Determine whether manual review is needed.",
      outcomes: ["Auto-approve", "Route to reviewer"]
    }
  },
  connections: [
    { from: "intake", to: "review", label: "Validated request", kind: "primary" }
  ]
};
```

## Layout

Use the orientation requested by the user. If no orientation is supplied, use a left-to-right flow for both data flow and process flow diagrams.

Recommended structure for a right-side details panel:
- Header with title and short subtitle
- Main content area with the SVG diagram on the left
- Details side panel on the right
- Responsive collapse where the side panel moves below the diagram on narrow screens

If the user requests a different panel position or static diagram, adapt the layout:
- `detailsPanel: left`: place the inspector before the diagram.
- `detailsPanel: bottom`: place the inspector below the diagram.
- `detailsPanel: collapsible`: add a vanilla JavaScript toggle.
- `detailsPanel: hidden`: keep node metadata in JavaScript only if interactions are still needed.

The first viewport should show the actual diagram, not a landing page or marketing hero.

Use stable SVG dimensions with a responsive `viewBox`. Size the viewBox from the number of nodes, lanes, phases, annotations, and branches. Example:

```html
<svg viewBox="0 0 1200 620" role="img" aria-labelledby="diagramTitle diagramDesc">
```

Keep the diagram readable at desktop and tablet widths. On mobile, allow horizontal scrolling for wide diagrams if preserving the requested orientation is clearer than compressing labels.

Alignment rules:
- Use grid alignment for system-heavy data flows with comparable components.
- Use lanes when ownership, team, actor, environment, or system boundary matters.
- Use phases for process diagrams with lifecycle stages.
- Use a centerline or timeline when sequence is more important than ownership.
- Use explicit coordinates only when the user provides a specific layout or sketch.
- Align node edges and connector anchors consistently; avoid small visual offsets that make the diagram feel accidental.
- Leave enough whitespace for connector labels and annotations.

For process flow diagrams:
- Use lanes only when ownership, team, system, or phase boundaries are important.
- Keep the primary path visually dominant.
- Place exception paths below the primary path.
- Place terminal outcomes at the far right.
- Use short connector labels for conditions such as `Approved`, `Rejected`, `Retry`, or `Escalate`.

For data flow diagrams:
- Put sources on the far left and consumers on the far right.
- Put transformation, orchestration, validation, or storage nodes in the middle.
- Keep control, governance, and monitoring nodes visually distinct from the main payload path.

For non-left-to-right orientations, transpose these rules:
- Top-to-bottom: sources or start states go at the top; outcomes or consumers go at the bottom.
- Right-to-left: sources or start states go at the right only when the user explicitly requests that reading direction.
- Bottom-to-top: use only when explicitly requested or when matching a provided visual convention.

## Component Design

Each component or process step must be an SVG group containing:
- Rounded rectangle
- Icon placeholder or inline real SVG icon
- Title
- Short description

Use a consistent component size unless the content requires otherwise.
Use input-provided sizing and density when available. Otherwise calculate node dimensions from label length, number of lines, and diagram density.

For process flow diagrams, use these node semantics:
- `start`: rounded rectangle or pill-like rounded rectangle
- `step`: rounded rectangle
- `decision`: diamond only when it improves readability; otherwise use a rounded rectangle with a decision icon and outcome labels
- `handoff`: rounded rectangle with owner/lane emphasis
- `exception`: rounded rectangle with warning accent
- `end`: rounded rectangle or terminal pill

When the user explicitly requires every component to be a rounded rectangle, keep decision nodes as rounded rectangles and communicate decisions through title, icon, and connector labels.

Recommended node markup:

```html
<g class="node" data-node="ingest" tabindex="0" role="button" aria-label="Ingestion layer">
  <rect class="node-card" x="80" y="180" width="210" height="120" rx="12"></rect>
  <rect class="icon-box" x="102" y="202" width="34" height="34" rx="8"></rect>
  <path class="icon-mark" d="M113 219h12m-6-6v12"></path>
  <text class="node-title" x="150" y="216">Ingestion</text>
  <text class="node-desc" x="150" y="238">Batch and streaming inputs</text>
</g>
```

Keep descriptions short enough to fit inside the node. Use SVG `<text>` with separate lines rather than relying on HTML wrapping inside SVG.
If the user provides long descriptions, summarize inside the node and put the full detail in the side panel or annotation.

Recommended decision node as rounded rectangle:

```html
<g class="node node-decision" data-node="review" tabindex="0" role="button" aria-label="Review required decision">
  <rect class="node-card" x="420" y="180" width="210" height="120" rx="12"></rect>
  <rect class="icon-box" x="442" y="202" width="34" height="34" rx="8"></rect>
  <path class="icon-mark" d="M459 210l10 9-10 9-10-9z"></path>
  <text class="node-title" x="490" y="216">Review Required?</text>
  <text class="node-desc" x="490" y="238">Route by policy result</text>
</g>
```

## Icons

Use simple inline SVG marks for all icons. Icons must be drawn directly in the generated SVG or embedded as inline SVG markup in the same HTML file.

Icons should be abstract and neutral:
- Input arrows
- Database cylinders
- Gear outlines
- Shield outlines
- Document shapes
- Chart marks

Do not use external platform, cloud, product, or service icon references. Do not fetch icon registries, hotlink hosted SVGs, or reference local files outside the generated HTML.

```html
<!-- Wrong: external image reference -->
<image href="external-icon.svg" x="18" y="16" width="28" height="28"></image>
```

Use inline icon geometry instead:

```html
<svg x="18" y="16" width="28" height="28" viewBox="0 0 28 28" aria-label="Database">
  <ellipse cx="14" cy="7" rx="9" ry="4"></ellipse>
  <path d="M5 7v12c0 2.2 4 4 9 4s9-1.8 9-4V7"></path>
  <path d="M5 13c0 2.2 4 4 9 4s9-1.8 9-4"></path>
</svg>
```

If the user asks for brand or product-specific icons, use neutral inline SVG symbols with text labels rather than recreating logos or relying on external assets.

## Connections

Represent connections with SVG paths.

Every connection should include:
- A visible base path
- An animated overlay path
- Arrow marker
- Optional small label

Recommended pattern:

```html
<defs>
  <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
    <path d="M 0 0 L 10 5 L 0 10 z"></path>
  </marker>
</defs>

<path class="flow-line" d="M290 240 C340 240 350 240 400 240" marker-end="url(#arrow)"></path>
<path class="flow-line-active" d="M290 240 C340 240 350 240 400 240"></path>
```

Use cubic Bezier paths for clean horizontal routing by default. If the user requests orthogonal, straight, swimlane-style, or timeline routing, use that routing instead. Avoid crossing lines where possible.

For process flow:
- Use connector labels for decisions and branch outcomes.
- Keep `Yes` / `No` labels close to the outgoing path, not inside the node.
- Route exceptions beneath the main path.
- Use a subtler stroke for optional or fallback paths.

For data flow:
- Label flows with payload or protocol when useful, such as `Events`, `Parquet`, `REST`, `CDC`, or `Aggregates`.
- Use line style to distinguish batch, stream, control, and error flows if the distinction matters.

## Annotations

Support annotations when the user provides notes, assumptions, risks, metrics, ownership, SLA, controls, or design comments.

Annotation types:
- `callout`: short note connected to a node or path
- `badge`: compact status, SLA, risk, or priority label
- `marker`: numbered or lettered reference marker
- `boundary-note`: note attached to a lane, phase, platform, or trust boundary
- `path-label`: label placed directly on or near a connector
- `footnote`: compact note outside the diagram canvas or below the SVG

Rules:
- Keep annotations visually subordinate to nodes and primary flow.
- Do not block connector paths or overlap node titles.
- Use callout leader lines only when the relationship is otherwise ambiguous.
- Put long explanations in the details panel instead of inside the SVG.
- Place annotations outside major boundaries unless the annotation applies specifically to that boundary.
- If annotations are numerous, use numbered markers in the SVG and a compact notes list beside or below the diagram.

Recommended annotation markup:

```html
<g class="annotation" data-for="review">
  <line class="annotation-line" x1="610" y1="172" x2="690" y2="126"></line>
  <rect class="annotation-box" x="690" y="102" width="180" height="48" rx="8"></rect>
  <text class="annotation-title" x="704" y="122">Policy Gate</text>
  <text class="annotation-desc" x="704" y="140">Manual review above threshold</text>
</g>
```

Annotation visibility may be static or interactive. If the user asks for dynamic annotations, show related annotations when a node is active and dim unrelated annotations.

## Animated Flow Effect

Animate only connector or flow lines.

Use CSS stroke animation:

```css
.flow-line-active {
  fill: none;
  stroke: var(--accent);
  stroke-width: 3;
  stroke-linecap: round;
  stroke-dasharray: 10 12;
  animation: flow var(--flow-speed, 1.2s) linear infinite;
}

@keyframes flow {
  to { stroke-dashoffset: -22; }
}
```

Do not animate decorative dots, pulsing badges, floating blobs, or unrelated background elements.

Respect reduced-motion preferences:

```css
@media (prefers-reduced-motion: reduce) {
  .flow-line-active {
    animation: none;
  }
}
```

## Interactions

Clicking a component or process step must update a details panel unless the user explicitly requests a static diagram.

Required behaviors:
- Hover glow on node
- Pointer cursor on clickable nodes
- Click sets active node
- Active node receives a stronger border or glow
- Details panel updates from available metadata
- Keyboard activation with `Enter` and `Space`
- Optional: emphasize incoming and outgoing connections for the active node
- Optional: reveal or emphasize annotations associated with the active node

Recommended data model:

```js
const nodes = {
  ingest: {
    title: "Ingestion",
    description: "Receives source system events and files.",
    responsibilities: ["Validate payloads", "Normalize schemas", "Route records"],
    inputs: ["Application events", "Partner files"],
    outputs: ["Validated raw data"],
    owner: "Data Platform",
    conditions: ["Schema must be recognized"],
    risks: ["Malformed records are quarantined"]
  }
};
```

Recommended interaction:

```js
function setActiveNode(id) {
  document.querySelectorAll(".node").forEach(node => {
    node.classList.toggle("is-active", node.dataset.node === id);
  });

  const item = nodes[id];
  document.querySelector("[data-detail-title]").textContent = item.title;
  document.querySelector("[data-detail-description]").textContent = item.description;

  document.querySelectorAll(".connection").forEach(path => {
    path.classList.toggle(
      "is-related",
      path.dataset.from === id || path.dataset.to === id
    );
  });

  document.querySelectorAll(".annotation").forEach(annotation => {
    annotation.classList.toggle("is-related", annotation.dataset.for === id);
  });
}

document.querySelectorAll(".node").forEach(node => {
  node.addEventListener("click", () => setActiveNode(node.dataset.node));
  node.addEventListener("keydown", event => {
    if (event.key === "Enter" || event.key === " ") {
      event.preventDefault();
      setActiveNode(node.dataset.node);
    }
  });
});
```

Initialize the first or most important node as active.

## Details Panel

The details panel should feel like an operational inspector, not marketing copy. Its placement and presence should follow the user's input.

Include:
- Component title
- Short description
- Responsibilities
- Inputs
- Outputs
- Optional status, owner, latency, SLA, decision conditions, security, governance, exception behavior, or risks when relevant

Use concise labels and dense but readable spacing.

For data flow diagrams, prioritize:
- Inputs
- Outputs
- Payloads or schemas
- Systems touched
- Security or governance notes
- Latency, cadence, or throughput when supplied

For process flow diagrams, prioritize:
- Owner
- Trigger
- Actions
- Decision conditions
- Handoffs
- SLA or timing
- Exceptions and outcomes

Example panel structure:

```html
<aside class="details-panel" aria-live="polite">
  <p class="eyebrow">Selected Component</p>
  <h2 data-detail-title></h2>
  <p data-detail-description></p>
  <div data-detail-sections></div>
</aside>
```

## Accessibility

Use:
- `role="img"` and `aria-labelledby` on the SVG
- `role="button"` and `tabindex="0"` on interactive SVG groups
- Visible focus styles
- `aria-live="polite"` on the details panel
- Sufficient color contrast

Do not rely on color alone to show active state. Combine color with border width, shadow, or label treatment.

## Responsive Behavior

Use CSS grid for the main layout, with columns driven by details panel placement.

```css
.workspace {
  display: grid;
  grid-template-columns: minmax(0, 1fr) var(--panel-width, 320px);
  gap: 20px;
}

@media (max-width: 900px) {
  .workspace {
    grid-template-columns: 1fr;
  }

  .diagram-scroll {
    overflow-x: auto;
  }
}
```

The SVG should scale within its container:

```css
.diagram svg {
  display: block;
  width: 100%;
  height: auto;
  min-width: var(--diagram-min-width, 860px);
}
```

Adjust `--diagram-min-width` from the chosen orientation, density, and node count. Do not use the fallback `860px` when the diagram is materially smaller or larger.

## Export Options

Export is optional unless the user asks for SVG export, download, sharing, or presentation-ready output.

When export is requested, include only a built-in `Download SVG` control implemented with vanilla JavaScript. Do not include PNG, PDF, copy-to-clipboard, raster capture, print export, CDN scripts, or external libraries.

Every export-enabled diagram must ship with a single unobtrusive `Download SVG` button in the header.

The export should serialize the primary diagram `<svg>` element, inject required computed styles or include the internal `<style>` definitions needed by the SVG, create a Blob with MIME type `image/svg+xml;charset=utf-8`, and trigger a download through an object URL.

Required export-enabled HTML:
- `id="report-container"` on the outermost `.container` element that should be captured
- `id="diagram-svg"` on the primary SVG element
- `.toolbar` markup with a `Download SVG` button
- `.toolbar` CSS
- `@media print { .toolbar { display: none !important; } }`
- `downloadSVG()` before `</body>`

Export caveats:
- SVG `<foreignObject>` can break portability. Use plain SVG shapes and `<text>`.
- External SVG files referenced through `<image href="...">` can export as broken images. Do not use them.
- External fonts, stylesheets, images, scripts, or symbol sprites can make the downloaded SVG incomplete. Keep all export-relevant styling and geometry inline or embedded in the generated file.
- Include `xmlns="http://www.w3.org/2000/svg"` on the exported SVG.

Recommended export helper:

```js
function downloadSVG() {
  const svg = document.getElementById("diagram-svg");
  const clone = svg.cloneNode(true);
  clone.setAttribute("xmlns", "http://www.w3.org/2000/svg");

  const style = document.createElementNS("http://www.w3.org/2000/svg", "style");
  style.textContent = `
    .node-card { fill: #fff; stroke: #DDE2EB; }
    .node-title { fill: #111827; font: 700 14px Arial, sans-serif; }
    .node-desc { fill: #5B6170; font: 12px Arial, sans-serif; }
    .flow-line { fill: none; stroke: #AAB4C3; stroke-width: 2; }
    .flow-line-active { fill: none; stroke: #0F766E; stroke-width: 3; stroke-dasharray: 10 12; }
  `;
  clone.insertBefore(style, clone.firstChild);

  const source = new XMLSerializer().serializeToString(clone);
  const blob = new Blob([source], { type: "image/svg+xml;charset=utf-8" });
  const url = URL.createObjectURL(blob);
  const link = document.createElement("a");
  link.href = url;
  link.download = "flow-diagram.svg";
  document.body.appendChild(link);
  link.click();
  link.remove();
  URL.revokeObjectURL(url);
}
```

Export preflight before final output:
- Search the generated HTML for external references: `<image`, `href="http`, `src="http`, `xlink:href="http`, `<script src=`, `<link`, `@import`, and `url(http`
- If any are present, replace them with inline HTML, CSS, SVG, or vanilla JavaScript
- For export-enabled diagrams, `document.querySelectorAll("svg image").length` should be `0`
- Confirm there are no external scripts or stylesheets
- Confirm the only export control is `Download SVG`

## Output Contract

When using this skill, output only the complete HTML unless the user explicitly asks for explanation.

The generated HTML must:
- Be a single file
- Open directly in a browser
- Contain no Mermaid
- Contain no Canvas
- Contain no external libraries
- Contain no external references
- Use inline SVG for all diagram visuals
- Include animated arrows between components
- Include hover effects
- Include click-to-update details panel behavior unless static output is requested
- Include active node highlighting when interactivity is enabled
- Include responsive CSS
- Honor user-provided theme, annotation, orientation, alignment, density, export, and interaction inputs
- Use a light theme only as the fallback when no theme is provided

Before finalizing, inspect the HTML for:
- `<canvas`
- `mermaid`
- `<link rel="stylesheet"`
- `<image`

Also inspect external scripts:
- Remove all `<script src=...>` tags.

If any forbidden element exists, remove it or replace it with inline HTML, CSS, SVG, or vanilla JavaScript.
