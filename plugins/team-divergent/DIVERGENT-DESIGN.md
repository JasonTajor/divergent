---
# ============================================================================
#  DIVERGENT DESIGN SYSTEM  —  Socia Design Tokens (Reusable)
#  v1.0 · Canonical token + UX contract for every project you revamp.
#
#  HOW TO USE THIS FILE
#  1. Copy this file into a new project's root as `DIVERGENT-DESIGN.md`.
#  2. Fill in the `project:` block below (name, north star).
#  3. Resolve the DECISION GATE (§0). Every `ask: true` decision must be
#     confirmed by a human before any UI is built — the agent MUST NOT guess.
#  4. The `config:` block is the single switchboard. Flip a setting there and
#     the whole system re-resolves (borders, accent, font, density, theme).
#  5. Everything below `---` is the human-readable spec. Everything inside
#     these `---` fences is the machine-readable token sheet.
# ============================================================================

project:
  name: "<PROJECT NAME>"                 # e.g. "DB Auto Trading", "Atlas"
  northStar: "<ONE-LINE CREATIVE NORTH STAR>"
  domain: "<what this product is / who uses it>"
  canonical: false                       # true = this repo is the source of truth others inherit

# ----------------------------------------------------------------------------
# §0  DECISION GATE  —  validate BEFORE building. `ask: true` = must confirm.
#     `default:` is what ships if the human says "just use the default".
# ----------------------------------------------------------------------------
decisions:
  surfaceMode:
    question: "Do we add a stroke/border to cards, inputs, and tables — or go borderless (tone + shadow only)?"
    options: [borderless, hairline, hybrid]
    default: hairline
    ask: true
    note: >
      borderless = calm, soft, consumer-leaning (DB Auto "Calm Instrument").
      hairline   = crisp, scannable, data-dense (Atlas "Operations Instrument"). Safest default.
      hybrid     = borderless cards, but hairline tables + inputs for scannability.
  accent:
    question: "What accent/brand color anchors the system? (the ≤10%-of-screen 'important thing')"
    options: [red, gold, blue, violet, green, mono, custom]
    default: gold
    ask: true
    note: "Pick ONE. It resolves through the --brand token only — never inline a hex."
  paletteInspiration:
    question: "What palette inspiration/mood are we drawing from?"
    options: ["figma-style", "linear", "vercel", "notion", "stripe", "warm-neutral graphite", custom]
    default: "warm-neutral graphite (Linear/Vercel/Notion restraint)"
    ask: true
    note: >
      Name the reference so tone stays intentional, not defaulted. "figma-style" = clean neutral
      canvas + a vivid multi-hue accent set reserved for data/charts (see §CHARTS), UI stays restrained.
  fontFamily:
    question: "Which type family carries the whole system? (one family, weights for contrast)"
    options: ["Geist", "Bricolage Grotesque", "Inter", "system-ui", custom]
    default: Geist
    ask: true
  themeDefault:
    question: "Is the product dark-first, light-first, or system?"
    options: [dark, light, system]
    default: dark
    ask: false
  density:
    question: "Baseline density?"
    options: [comfortable, compact, dense]
    default: comfortable
    ask: false
  radiusScale:
    question: "Corner personality — soft (Apple), medium, or sharp?"
    options: [soft, medium, sharp]
    default: medium
    ask: false
  motion:
    question: "How expressive is motion?"
    options: [restrained, standard, playful]
    default: standard
    ask: false
  codeSplitting:
    question: "How aggressively do we code-split? (bundle strategy for reusable components)"
    options: [route, component, aggressive, off]
    default: component
    ask: false
    note: >
      route      = split per page/route only.
      component  = route + lazy-load heavy reusable components (charts, tables, editors, modals). Default.
      aggressive = component + granular chunks per primitive group + prefetch on intent.
      off        = single bundle (only for tiny apps). Reusable components are the split unit — see §11.
  deadCodeScan:
    question: "Run a dead-code / unused-export scan before shipping (optimize + clean)?"
    options: [on, off]
    default: on
    ask: false
    note: "MUST pass before UI ships. Prunes unused exports, orphan files, dupe components, unreachable branches — see §11."

# ----------------------------------------------------------------------------
# §CONFIG  —  the live switchboard. These values drive everything below.
#            Set them from the resolved decisions above.
# ----------------------------------------------------------------------------
config:
  surfaceMode: hairline          # borderless | hairline | hybrid
  accent: gold                   # active key from `accents` map below
  themeDefault: dark             # dark | light | system
  density: comfortable           # comfortable | compact | dense
  radiusScale: medium            # soft | medium | sharp
  motion: standard               # restrained | standard | playful
  codeSplitting: component       # route | component | aggressive | off
  deadCodeScan: on               # on | off  (gate: must pass before ship)
  accentBudgetPct: 10            # accent may cover ≤ this % of any screen
  a11yTarget: "WCAG 2.1 AA"

# ----------------------------------------------------------------------------
# §UX LAWS  —  the non-negotiable constitution. EVERY feature obeys all ten.
#     Not preferences — laws. They are the shared decision framework that makes
#     independently-built features feel like ONE product. Full spec in §12.
# ----------------------------------------------------------------------------
uxLaws:
  - id: L1  law: "Every page answers Where am I? · What can I do? · What's next?"
  - id: L2  law: "Never make users lose their work."
  - id: L3  law: "Every data view supports search and filtering."
  - id: L4  law: "Prefer inline editing over unnecessary navigation."
  - id: L5  law: "Never ship a component without loading, empty, and error states."
  - id: L6  law: "Remember user preferences whenever it improves efficiency."
  - id: L7  law: "Make the primary action visible without scrolling whenever practical."
  - id: L8  law: "Every interaction provides immediate feedback."
  - id: L9  law: "Optimize for keyboard users as well as mouse users."
  - id: L10 law: "Consistency takes precedence over novelty."

# ----------------------------------------------------------------------------
# §ACCENTS  —  swappable brand accents. `config.accent` selects one.
#             Each resolves to the single --brand token at build time.
# ----------------------------------------------------------------------------
accents:
  red:    { base: "#EF4444", hover-light: "#DC2626", hover-dark: "#F05252", on: "#FFFFFF" }
  gold:   { base: "#C7934A", hover-light: "#B07D38", hover-dark: "#B5823C", on-light: "#231A0E", on-dark: "#FFFFFF" }
  blue:   { base: "#2563EB", hover-light: "#1D4ED8", hover-dark: "#3B82F6", on: "#FFFFFF" }
  violet: { base: "#7C3AED", hover-light: "#6D28D9", hover-dark: "#8B5CF6", on: "#FFFFFF" }
  green:  { base: "#16A34A", hover-light: "#15803D", hover-dark: "#22C55E", on: "#FFFFFF" }
  mono:   { base: "#1B1C1F", hover-light: "#000000", hover-dark: "#FAFAFA", on-light: "#FFFFFF", on-dark: "#191919" }

# ----------------------------------------------------------------------------
# §COLORS  —  warm-neutral graphite foundation (theme-paired: light / dark).
#             Superset of DB Auto + Atlas. brand-* is filled from the active accent.
# ----------------------------------------------------------------------------
colors:
  brand: "{accents[config.accent].base}"
  brand-hover-light: "{accents[config.accent].hover-light}"
  brand-hover-dark: "{accents[config.accent].hover-dark}"
  brand-foreground-light: "{accents[config.accent].on-light | on}"
  brand-foreground-dark: "{accents[config.accent].on-dark | on}"

  bg-light: "#F5F6F8"          # Canvas — most-recessed page well
  bg-dark: "#191919"
  surface-light: "#FFFFFF"     # Surface — rounded content container
  surface-dark: "#202020"
  surface-2-light: "#F1F3F5"   # Surface-2 — nested fills, input hovers, insets
  surface-2-dark: "#2C2C2C"
  card-light: "#FFFFFF"        # Card — panels sitting above surface
  card-dark: "#202020"
  muted-light: "#F3F4F6"       # Muted — recessed insets, table header wash, field fills
  muted-dark: "#262626"

  border-light: "#E9EAEE"      # default hairline (used when surfaceMode ≠ borderless)
  border-dark: "#2A2A2A"
  border-strong-light: "#D7DAE0"  # stronger dividers, outline buttons
  border-strong-dark: "#373737"

  ink-light: "#1B1C1F"         # primary text
  ink-dark: "#FAFAFA"
  muted-ink-light: "#5B5E66"   # secondary text (≥4.5:1 body)
  muted-ink-dark: "#A1A1AA"
  subtle-ink-light: "#898C95"  # tertiary / hint
  subtle-ink-dark: "#71717A"

  # Semantic — soft-fill tints, never color-only (always pair with icon/text)
  success-light: "#16A34A"
  success-dark: "#4ADE80"
  warning-light: "#D97706"
  warning-dark: "#FBBF24"
  destructive-light: "#DC2626"
  destructive-dark: "#F87171"
  info-light: "#0891B2"
  info-dark: "#22D3EE"
  violet-light: "#7C3AED"
  violet-dark: "#A78BFA"
  # chart series: chart-1 tracks --brand, then step around the wheel
  chart: ["{colors.brand}", "#0891B2", "#16A34A", "#D97706", "#7C3AED"]

# ----------------------------------------------------------------------------
# §TYPOGRAPHY  —  one family, contrast by weight/size. Family from config.
# ----------------------------------------------------------------------------
typography:
  fontFamily: "{config.fontFamily}, ui-sans-serif, system-ui, sans-serif"
  fontFamilyMono: "Geist Mono, ui-monospace, monospace"   # figures / IDs
  weights: [400, 500, 600, 700]
  # role → { px, weight, line-height, tracking }
  display:  { px: 24, weight: 600, leading: 1.2,  tracking: "-0.02em" }   # page hero
  section:  { px: 20, weight: 600, leading: 1.2,  tracking: "-0.015em" }  # section header
  cardTitle:{ px: 16, weight: 600, leading: 1.35, tracking: "-0.01em" }   # card / widget title
  body:     { px: 14, weight: 400, leading: 1.5,  tracking: "-0.005em" }  # DEFAULT body, inputs, buttons, cells
  label:    { px: 12, weight: 500, leading: 1.4,  tracking: "0" }         # table headers, form labels, chips, captions
  helper:   { px: 11, weight: 400, leading: 1.5,  tracking: "0" }         # helper / hint text
  badge:    { px: 10, weight: 600, leading: 1.1,  tracking: "+0.01em" }   # count badges only

# ----------------------------------------------------------------------------
# §RADIUS  —  three personalities; config.radiusScale selects the column.
# ----------------------------------------------------------------------------
radius:
  soft:   { xs: "6px", sm: "8px",  md: "12px", lg: "16px", xl: "22px", 2xl: "28px", full: "9999px" }
  medium: { xs: "6px", sm: "8px",  md: "10px", lg: "12px", xl: "16px", 2xl: "20px", full: "9999px" }
  sharp:  { xs: "3px", sm: "4px",  md: "6px",  lg: "8px",  xl: "10px", 2xl: "12px", full: "9999px" }

# ----------------------------------------------------------------------------
# §SPACING  —  8pt grid (4px base unit).
# ----------------------------------------------------------------------------
spacing:
  4: "4px"
  8: "8px"
  16: "16px"
  24: "24px"
  32: "32px"
  40: "40px"
  48: "48px"
  56: "56px"
  64: "64px"
  80: "80px"
  96: "96px"

# ----------------------------------------------------------------------------
# §ELEVATION  —  tonal + (border if surfaceMode ≠ borderless) + shadow.
# ----------------------------------------------------------------------------
elevation:
  shadow-soft: "0 1px 2px rgb(0 0 0 / 0.04), 0 1px 1px rgb(0 0 0 / 0.03)"  # resting card/panel
  shadow-md:   "0 4px 12px rgb(0 0 0 / 0.08), 0 2px 4px rgb(0 0 0 / 0.04)"  # dropdowns, popovers
  shadow-lg:   "0 12px 32px rgb(0 0 0 / 0.14), 0 4px 8px rgb(0 0 0 / 0.06)" # dialogs, palette, sheets
  darkStrategy: "lean on tone; shadows near-invisible in dark"

# ----------------------------------------------------------------------------
# §MOTION  —  config.motion scales these.
# ----------------------------------------------------------------------------
motion:
  fast: "120ms"
  base: "200ms"
  slow: "320ms"
  ease-standard: "cubic-bezier(0.2, 0, 0, 1)"
  ease-spring: "cubic-bezier(0.34, 1.56, 0.64, 1)"
  reducedMotion: "honor prefers-reduced-motion AND a manual [data-reduce-motion] toggle → 0.01ms"
  # Smooth-but-light dashboard presets. GPU-friendly props ONLY (transform/opacity). Never animate layout.
  presets:
    page-enter:   { from: "opacity:0; translateY(8px)", to: "opacity:1; translateY(0)", duration: "base", ease: "ease-standard" }
    card-enter:   { from: "opacity:0; translateY(6px)", to: "opacity:1; translateY(0)", duration: "base", ease: "ease-standard" }
    stagger:      { step: "40ms", max: "6 items", note: "cap stagger so a grid never feels slow; beyond 6, fade the group as one" }
    hover-lift:   { transform: "translateY(-2px)", shadow: "shadow-soft→shadow-md", duration: "fast" }
    press:        { transform: "scale(0.98)", duration: "fast" }
    row-hover:    { fill: "muted/50", duration: "fast" }
    number-count: { note: "count-up stat figures over 'base'; snap to final on reduced-motion" }
    chart-draw:   { note: "line/area path draws L→R once on mount over 'slow'; bars grow from baseline, staggered 'step'" }
    skeleton-shimmer: { duration: "1200ms", ease: "linear", note: "subtle sheen; disabled on reduced-motion (static muted block)" }
  budget: "≤ 'slow' (320ms) for any single transition; no parallax, no infinite bounce, no full-page motion"

# ----------------------------------------------------------------------------
# §SURFACE RULES  —  the border option, resolved per mode. THIS IS THE SWITCH.
#     Read config.surfaceMode, then apply the matching row to card/input/table.
# ----------------------------------------------------------------------------
surfaceRules:
  borderless:
    card:  { border: "none",                       fill: "{colors.card}",  shadow: "shadow-soft" }
    input: { border: "none",                       fill: "{colors.muted}", focus: "bg-surface + ring-2 ring-brand/70" }
    table: { wrapperBorder: "none", rowSeparator: "divide-y divide-border", wrapperShadow: "shadow-soft" }
    dividers: "tone steps + hairline dividers only (never a full card outline)"
  hairline:
    card:  { border: "1px solid {colors.border}",  fill: "{colors.card}",  shadow: "shadow-soft" }
    input: { border: "1px solid {colors.border}",  fill: "transparent",    focus: "border-brand + ring-3 ring-brand/25" }
    table: { wrapperBorder: "1px solid {colors.border}", rowSeparator: "border-b + border-r (gridded)", headerWash: "{colors.muted}/30" }
    dividers: "hairline border on every element as the default edge"
  hybrid:
    card:  { border: "none",                       fill: "{colors.card}",  shadow: "shadow-soft" }
    input: { border: "1px solid {colors.border}",  fill: "transparent",    focus: "border-brand + ring-3 ring-brand/25" }
    table: { wrapperBorder: "1px solid {colors.border}", rowSeparator: "border-b (gridded)", headerWash: "{colors.muted}/30" }
    dividers: "cards float on tone+shadow; data surfaces (tables, inputs) get hairlines for scannability"
  # Floating chrome ALWAYS keeps a border + larger shadow, in every mode:
  floatingChrome: { border: "1px solid {colors.border}", shadow: "shadow-md | shadow-lg" }

# ----------------------------------------------------------------------------
# §COMPONENTS  —  base specs. `border` fields defer to §surfaceRules.
# ----------------------------------------------------------------------------
components:
  button-primary:   { bg: "{colors.brand}", text: "{colors.brand-foreground}", rounded: "{radius.md}", padding: "8px 16px", height: "40px", weight: 600, shadow: "shadow-soft" }
  button-secondary: { bg: "{colors.surface-2}", text: "{colors.ink}", rounded: "{radius.md}", padding: "8px 16px", height: "40px", border: "→surfaceRules" }
  button-outline:   { bg: "transparent", text: "{colors.ink}", rounded: "{radius.md}", padding: "8px 16px", height: "40px", border: "1px solid {colors.border-strong}" }
  button-ghost:     { bg: "transparent", text: "{colors.ink}", hover: "{colors.muted}", rounded: "{radius.md}", height: "40px" }
  button-destructive:{ bg: "{colors.destructive}", text: "#FFFFFF", rounded: "{radius.md}", height: "40px" }
  buttonShape:      "borderless→pill (rounded-full) · hairline/hybrid→rounded-md — one shape system per project"
  card:             { bg: "{colors.card}", text: "{colors.ink}", rounded: "{radius.lg}", padding: "24px", border: "→surfaceRules.card", shadow: "→surfaceRules.card" }
  input:            { text: "{colors.ink}", rounded: "{radius.md}", padding: "0 12px", height: "40px", border: "→surfaceRules.input", fill: "→surfaceRules.input" }
  status-chip:      { bg: "{status}-tint (13% color-mix)", text: "{status}", rounded: "{radius.full}", padding: "4px 10px", rule: "always lead with icon — never color alone" }
  badge:            { bg: "{colors.muted}", text: "{colors.muted-ink}", rounded: "{radius.full}", padding: "2px 10px" }
  tab-active:       { bg: "{colors.surface}", text: "{colors.ink}", rounded: "{radius.md}", shadow: "shadow-soft", note: "raised neutral pill, NOT a brand fill" }
  sidebar-active:   { bg: "{colors.surface-2}", text: "{colors.ink}", rounded: "{radius.md}", note: "raised pill; brand reserved for the accent indicator" }

# ----------------------------------------------------------------------------
# §LAYOUT  —  the app shell. Header + content live in ONE rounded container;
#            header stays fixed on top while content scrolls under it.
# ----------------------------------------------------------------------------
layout:
  appShell: "sidebar (left) + main region (right). Main region = ONE rounded container holding a fixed header + scrolling content."
  well:            { bg: "{colors.bg}" }                      # the canvas the container floats on
  container:       { bg: "{colors.surface}", rounded: "{radius.3xl}", inset: "8px", shadow: "shadow-soft", clip: "overflow-hidden" }
  header:          { position: "sticky top-0 z-20", bg: "{colors.surface}/85 + backdrop-blur", height: "56px", padding: "0 24px", separator: "hairline bottom (or shadow-soft on scroll)" }
  content:         { scroll: "y", padding: "24px", gap: "24px" }
  headerOnScroll:  "header gains shadow-soft / stronger blur once content scrolls > 8px (condensed state)"
  note: "Header and content share the container's top corners — the header does NOT sit in its own separate box."

# ----------------------------------------------------------------------------
# §SIDEBAR  —  resizable + collapsible-by-group nav.
# ----------------------------------------------------------------------------
sidebar:
  width:           { default: "248px", min: "200px", max: "360px", rail: "72px" }   # rail = icon-only collapsed
  resize:
    handle: "4px hit-area on the right edge (12px cursor zone); cursor: col-resize; grabbed handle tints brand"
    behavior: "drag to set width between min/max; double-click handle = reset to default; below min snaps to rail"
    persist: "remember width + collapsed state per user (localStorage/prefs)"
    a11y: "handle is focusable; ←/→ nudge 8px, Home=min, End=max; aria-valuenow/min/max on a separator role"
    motion: "live-follow drag (no transition while dragging); animate only the snap-to-rail over 'base'"
  groups:
    pattern: "accordion — each group = a header row (icon + label + chevron) that expands/collapses its items"
    header:  { text: "{colors.muted-ink}", weight: 600, size: "label", chevron: "rotates 180° over 'fast'" }
    item:    { rounded: "{radius.md}", padding: "8px 12px", height: "36px", hover: "{colors.muted}" }
    active:  "→components.sidebar-active (raised pill) + a 2px brand indicator bar on the leading edge"
    subItems:"nested items indent 12px with a vertical guide line ({colors.border}); active sub-item = brand text/left-rule"
    rules:
      - "one group open by default (the active route's group auto-opens); others collapsed"
      - "collapsed rail shows group icons only; hovering a group opens a flyout with its items"
      - "expanded/collapsed state per group is remembered"
      - "section labels (Overview / CRM / Inventory / Reports / System / Help) are group headers, not dead text"

# ----------------------------------------------------------------------------
# §SKELETON  —  loading placeholders that MIRROR the component they replace.
# ----------------------------------------------------------------------------
skeleton:
  rule: "A skeleton must match the real component's box: same size, radius, padding, and internal layout slots."
  fill: "{colors.muted}"
  radius: "inherit the target's radius (card→radius.lg, chip→full, avatar→full, text-line→radius.sm)"
  shimmer: "→motion.presets.skeleton-shimmer (static muted block on reduced-motion)"
  shapes:
    text-line: "height = line-height of the text it stands in; last line 60% width"
    stat-tile: "small label bar + large number bar + a faint sparkline block — same grid as the real tile"
    chart:     "axis ghost + a low-contrast area/bar silhouette at the chart's real height"
    table-row: "one bar per column, widths matching the real column widths; repeat for N rows"
    avatar:    "circle at the real avatar size"
  antipattern: "NO generic full-width gray rectangles that don't match the final layout (causes layout shift on load)"

# ----------------------------------------------------------------------------
# §CHARTS  —  modern, not boring. Vivid multi-hue series on the restrained shell.
#            Charts are the ONE place the ≤10% accent budget is lifted — data may
#            be colorful, but chrome (axes/grid/labels) stays quiet.
# ----------------------------------------------------------------------------
charts:
  palette:                                         # categorical wheel — high-chroma, dark-tuned (figma-style)
    - "#3B82F6"   # blue
    - "#EC4899"   # pink/magenta
    - "#F59E0B"   # amber
    - "#8B5CF6"   # violet
    - "#10B981"   # emerald
    - "#22D3EE"   # cyan
    - "#F43F5E"   # rose
  gradientFill: "series color → transparent, top→bottom (e.g. color/35 → color/0) for area fills"
  grid:    { lines: "horizontal only, {colors.border} dashed at ~30% opacity", axis: "no hard axis lines", ticks: "{colors.subtle-ink}, label size" }
  line:    { width: "2.5px", cap: "round", smoothing: "monotone (soft curves, no sharp elbows)", draw: "→motion.presets.chart-draw" }
  area:    { stroke: "2.5px series color", fill: "gradientFill", stack: "layered translucent for compare (see reference dashboards)" }
  bar:     { radius: "6px top corners (rounded caps)", gap: "~35%", grow: "from baseline, staggered on mount" }
  donut:   { thickness: "18–22% of radius", rounded: "rounded segment caps + small gaps", centerLabel: "big figure + caption (e.g. '1,125 / Visitors')" }
  radial:  { track: "{colors.muted}", value: "brand or series gradient, rounded cap", centerLabel: "figure + caption (gauge style)" }
  sparkline: { height: "28–40px", inline: "lives inside stat tiles (no axes/labels)", fill: "faint gradient under a 2px line" }
  point:   { show: "on hover / last point only", halo: "2px surface ring so it reads on any fill" }
  tooltip: "→floating chrome (bg-surface, border, shadow-md, radius.md); show series swatch + label + tnum value"
  legend:  "chips with a color swatch; top-right or bottom; toggling a series dims it (never removes silently)"
  states:  { loading: "→skeleton.shapes.chart", empty: "centered icon + 'No data' + one-line CTA", error: "inline retry" }
  numbers: ".tnum tabular figures everywhere; abbreviate (1.2k, 3.4M) on axes"
  rules:
    - "colorful DATA, quiet CHROME — never rainbow the gridlines/labels"
    - "one gradient family per chart; don't mix 5 unrelated gradients in a single plot"
    - "always pair color with a label/legend — never rely on hue alone (a11y)"
    - "prefer donut-with-center-metric and sparkline-in-tile over flat, label-less mystery charts"

# ----------------------------------------------------------------------------
# §ARCHITECTURE  —  code-splitting around reusable components + dead-code hygiene.
#     Reusable components are the unit of reuse AND the unit of the split.
# ----------------------------------------------------------------------------
architecture:
  reuseFirst:
    principle: "Build once in components/ui/*, import everywhere. One primitive, many props — never a copy."
    structure:
      - "components/ui/*        — atoms/molecules (button, card, input, chip, table, chart, skeleton)"
      - "components/patterns/*  — composed reusables (data-table, stat-tile, page-header, empty-state)"
      - "features/<domain>/*    — feature code that COMPOSES the above; holds no restyled primitives"
      - "lib/ · hooks/ · types/ — shared logic; no UI"
    barrels: "one index barrel per folder for clean imports; but import heavy items directly to keep them tree-shakeable"
  codeSplitting:
    strategy: "{config.codeSplitting}"   # route | component | aggressive | off
    splitUnit: "the reusable component — heavy primitives are lazy chunks, shared across every feature that uses them"
    routeLevel: "every page/route is its own lazy chunk (React.lazy/Suspense · next/dynamic · defineAsyncComponent)"
    componentLevel:                        # lazy-load these heavy reusables behind Suspense + a shape-matched skeleton
      - "charts (§CHARTS) — chart lib is large; load only on views that render one"
      - "data-table (sort/filter/virtualization engine)"
      - "rich editors / date & time pickers / file-upload / command palette"
      - "modals, drawers, sheets — mount on open, not on page load"
    fallback: "every lazy boundary shows a §SKELETON that matches the component (no spinner, no layout shift)"
    prefetch: "prefetch a chunk on intent (hover/focus/viewport) for aggressive; never eager-load below the fold"
    sharedChunks: "ui/* primitives + vendor go in a stable shared chunk so they cache across routes"
    icons: "import icons per-name (tree-shakeable), never the whole set"
    budgets: { initialJS: "≤ 200KB gzip", route: "≤ 120KB gzip", chart-chunk: "lazy (excluded from initial)" }
  deadCode:
    scan: "{config.deadCodeScan}"          # on | off — gate item, must pass before ship
    targets:
      - "unused exports & files (orphans)"
      - "unreferenced components / duplicate or forked primitives (fold into the canonical one)"
      - "unused deps, unreachable branches, commented-out blocks"
      - "unused design tokens / CSS (purge), unused Tailwind classes"
    tools: "knip or ts-prune (unused exports/files) · depcheck (deps) · eslint no-unused-vars · Tailwind/PurgeCSS · bundle analyzer"
    rule: "scan reports ZERO unused exports/orphans before UI is marked done; deleting dead code is part of the task, not a follow-up"
    caution: "verify a symbol is truly unreferenced (grep + tool agree) before deleting; keep intentional public API via an allowlist"

# Divergent Design System — `<PROJECT NAME>`

> A single, reusable token + UX contract for every Socia project. Combines and
> improves the **DB Auto ("Calm Instrument", borderless)** and **Atlas
> ("Operations Instrument", hairline)** systems into one switchboard. The border
> question — and every other project-shaping choice — is a **configurable option
> with a validated default**, not a hardcoded decision.

---

## 0. The Decision Gate (validate before you build)

**Rule for the agent:** Before writing *any* UI for this project, resolve every
`ask: true` decision in the `decisions:` block above. For each unresolved one,
**ask the human a short, plain question and wait** — do not assume, do not
default silently. Once answered, write the choices into the `config:` block and
proceed. Decisions marked `ask: false` may take their default unless the human
raises them.

The questions you must confirm up front (the "tweak design" gate):

| # | Decision | What you're really asking | Default |
|---|----------|---------------------------|---------|
| 1 | **Stroke on cards?** | "Do we add a border/stroke to each card, input, and table — or go borderless (tone + shadow only)?" | `hairline` |
| 2 | **Accent color** | "What one accent anchors the product (the ≤10%-of-screen 'important thing')?" | `gold` |
| 3 | **Palette inspiration** | "What mood/reference — e.g. `figma-style`, Linear, Vercel, Notion, Stripe, or warm-neutral graphite?" | warm-neutral graphite |
| 4 | **Type family** | "Which single family carries everything?" | `Geist` |
| 5 | **Theme default** | "Dark-first, light-first, or system?" | `dark` |
| 6 | **Density** | "Comfortable, compact, or dense?" | `comfortable` |
| 7 | **Radius personality** | "Soft (Apple), medium, or sharp corners?" | `medium` |
| 8 | **Motion** | "Restrained, standard, or playful?" | `standard` |
| 9 | **Code-splitting** | "How do we split the bundle around reusable components — route, component, aggressive, or off?" | `component` |
| 10 | **Dead-code scan** | "Run the unused-export / orphan / dupe scan before shipping (optimize + clean)?" | `on` |

Items 9–10 are `ask: false` (sensible defaults) but are **enforced gates** — the
build is not "done" until code-splitting is wired around the reusable components
and the dead-code scan reports clean (see §11).

> **Why a gate, not defaults-only:** the two source systems were 90% identical
> and diverged on exactly these axes (Atlas bordered + gold + Geist; DB Auto
> borderless + red + Bricolage). Surfacing them as an explicit, validated choice
> is what makes this file reusable instead of a copy of one product.

### How to run the gate (agent playbook)
**Entry gate (before build):**
1. Read `decisions:` → collect every `ask: true` item still unset for this project.
2. Ask them together in one pass (batch the questions; don't drip one at a time).
3. Echo back the resolved `config:` block for a final confirm.
4. Only then build. If a human later says "actually, add strokes" — flip
   `config.surfaceMode` and the whole system re-resolves via §Surface Rules.

**Exit gate (before "done"):** the enforced-default items (9–10) must clear —
5. **Code-split** the reusable components per `config.codeSplitting` — heavy
   primitives (charts, data-table, editors, modals) are lazy chunks behind a
   shape-matched skeleton; verify the initial bundle is within budget (§11).
6. **Dead-code scan** (`config.deadCodeScan`) reports **zero** unused
   exports / orphan files / duplicate primitives; delete what it finds — cleaning
   is part of the task, not a follow-up (§11).

### The Unify-First Rule (do this before applying tokens)
**Unify components BEFORE re-skinning them with the design tokens.** On any revamp,
the fastest, safest path is:

1. **Inventory** — list every variant of each primitive already in the codebase
   (every button, card, input, modal, table). Note the duplicates and one-offs.
2. **Consolidate** — collapse each set into a **single** canonical `components/ui/*`
   primitive with variants/props. Delete the forks. One Button, one Card, one Input.
3. **Then tokenize** — point that one primitive at the design tokens (`--brand`,
   scales, `surfaceMode`). Because there's now one source, the token pass is a
   handful of edits, not hundreds.

> Re-skinning first and unifying later means re-skinning the same button five
> times. Unify first → theme once → done. This is the "easy process".

### Law Zero — the UX Laws bind every feature
Beyond the per-project `config`, **ten UX Laws (§12) apply to every feature,
always** — they are not part of the switchboard and cannot be toggled off. Before
a feature is "done", check it against all ten. If a design choice conflicts with a
law, the law wins. They are the shared framework that keeps independently-built
features feeling like one product.

---

## 1. Design Philosophy — the North Star

Fill `project.northStar`. Every project on this system shares one spine:

**"The Calm Instrument."** The interface is an instrument, not a showpiece:
fast, precise, quiet, and it gets out of the way of the task. Confidence comes
from restraint — these are systems where money, stock, and work are tracked, so
they must read as accurate and dependable, never flashy. A warm-neutral graphite
scene with **one** accent used sparingly; separation carried by tone (and, if
enabled, a hairline) plus a soft shadow — never a gradient.

**This system explicitly rejects:** the generic AI-slop dashboard (purple→blue
gradients, gradient text, glassmorphism, identical icon-card grids, the
big-number hero-metric template), the cramped spreadsheet look, the
consumer/playful toy look, and the dated Bootstrap-gray enterprise admin.

**Universal characteristics** (true regardless of config):
- Warm-neutral graphite surfaces; one accent on ≤10% of any screen.
- Depth from tone + soft shadow (+ hairline when `surfaceMode ≠ borderless`) — never a gradient.
- **One** unified component system (`components/ui/*`); never fork a component.
- 8pt grid; ≥44px touch targets on mobile.
- Theme-paired light/dark; `WCAG 2.1 AA`; honors reduced motion.

---

## 2. The Border Option (the headline setting)

This is the difference between the two source systems, promoted to a single
switch: **`config.surfaceMode`**. Set it once; it cascades to every card, input,
and table through §Surface Rules. Floating chrome (menus, dialogs, tooltips,
toasts, palettes) **always** keeps a border + larger shadow in every mode.

| Mode | Cards | Inputs | Tables | Feels like | Use when |
|------|-------|--------|--------|-----------|----------|
| **`borderless`** | no stroke · tone + `shadow-soft` | recessed `bg-muted` fill, no stroke | no wrapper stroke · `divide-y` rows | Calm, soft, modern-consumer (DB Auto) | Marketing-adjacent, lighter data density, a softer brand |
| **`hairline`** *(default)* | `1px border` + tone + `shadow-soft` | `bg-transparent` + `1px border` | gridded · header wash · `border-b/-r` | Crisp, scannable, enterprise (Atlas) | Dense tables, finance/ops, "canonical" apps |
| **`hybrid`** | borderless (tone + shadow) | `1px border` | gridded | Calm shells, scannable data | You want soft cards but legible tables/forms |

**Named rule — The Surface-Mode Rule.** Never mix modes ad hoc. A card either
follows the project's `surfaceMode` or it's a bug. The only always-bordered
surface is floating chrome.

---

## 3. Design Tokens

All tokens live in the front-matter above and should be emitted to
`globals.css`/`App.css` as CSS variables (`:root` = light, `.dark` = dark), with
a shadcn-name bridge so shadcn/ui primitives theme correctly.

- **Colors** — §COLORS. Warm-neutral graphite tiers (Canvas → Surface →
  Surface-2 → Card → Muted), Ink/Muted-Ink/Subtle-Ink text ramp, hairline +
  strong borders, seven semantic kinds as soft-fill tints. `--brand` is filled
  from the **active accent** (§ACCENTS) — swap `config.accent` to repaint.
- **Typography** — §TYPOGRAPHY. One family (from `config.fontFamily`), weights
  400/500/600/700; contrast from weight + size, never a second typeface. Default
  body is 14px. Optional mono for figures/IDs. Tabular-nums in tables/stat tiles.
- **Radius** — §RADIUS. `config.radiusScale` picks soft / medium / sharp.
- **Spacing** — §SPACING. 8pt grid, 4px base unit.
- **Elevation** — §ELEVATION. `shadow-soft` (resting) · `shadow-md` (popovers) ·
  `shadow-lg` (dialogs/sheets). Dark leans on tone.
- **Motion** — §MOTION. `fast/base/slow` + two easings; reduced motion honored
  two ways (media query + manual toggle).

### Named token rules
- **Token-Only Rule.** Accent is always the `--brand` token (`bg-brand`,
  `text-brand`, `ring-brand`). A hardcoded accent hex is forbidden — it breaks
  the instant accent switch and cross-app consistency.
- **Token-Scale Rule.** Use the type/space/radius scales — never arbitrary
  `text-[Npx]`, `p-[13px]`, `rounded-[7px]` values.
- **One Voice Rule.** The accent covers ≤`config.accentBudgetPct`% of any
  screen: the primary action, the active state, the focus ring, one indicator.

---

## 4. Components

One unified, reusable set (`components/ui/*`). Never fork a component. Every
interactive component ships **all** states: `default · hover · focus · active ·
disabled · loading · success · warning · error · empty · selected · read-only`.

- **Buttons** — variants: primary (one per surface) · secondary · outline ·
  ghost · destructive · link · icon · split · loading · disabled. Shape follows
  `buttonShape` (pill in borderless, `rounded-md` otherwise). Focus = brand ring.
- **Status chips** — soft-fill pills; background = status `-tint`, text = status
  color; **always lead with an icon — never color alone**. Provide a
  `statusFromDomain()` mapper so any domain status resolves to the palette.
- **Cards** — `bg-card`, `rounded-lg`, padding ≥`p-5`/`p-6`; border + shadow per
  §Surface Rules. A panel nested in a card steps **down** to `bg-surface-2`
  (inset) — never a card-in-a-card seam.
- **Inputs / fields** — border + fill per §Surface Rules; focus = brand ring;
  error = persistent ring **paired with inline text**; disabled = 50% opacity.
  Applies to inputs, textareas, selects, and shared pickers.
- **Tabs** — filled-pill segmented control: `bg-muted` track, active trigger
  raises to `bg-surface` + `shadow-soft` with `text-foreground` — **not** a
  brand fill. Underline variant for page/section archetype.
- **Tables** — see §Surface Rules for the border treatment; header `text-xs`
  muted, cells `py-3`, hover row wash, selected row = `bg-muted`.
- **Navigation** — **resizable, collapsible-by-group** left sidebar (see §6) +
  the app-shell header (see §5). Active item = a **raised** neutral pill (surface,
  not a color wash) with a 2px brand edge indicator; accent reserved for that
  indicator. Mobile swaps to bottom nav; command palette for jump-to.
- **Charts** — modern, colorful-data / quiet-chrome (see §7). Gradient area fills,
  rounded bars, donut/radial with a center metric, sparklines inside stat tiles.
- **Skeletons** — shape-matched to the component they replace (see §8).

---

## 5. App Shell — header + content in one rounded container

The main region (everything right of the sidebar) is **one rounded container**
floating on the canvas well. The **header is fixed to the top of that container**
and the content scrolls underneath it — the header is *not* a separate box above
a separate content box; they share the container's top corners.

```
┌ well (bg) ──────────────────────────────────────────────┐
│  ┌ container (bg-surface · rounded-3xl · shadow-soft) ─┐ │
│  │  ▓ header  (sticky top-0 · blur · hairline bottom) ▓ │ │  ← fixed
│  │   · content (scrolls · p-6 · gap-6) ················ │ │  ← scrolls
│  │  · · · · · · · · · · · · · · · · · · · · · · · · · · │ │
│  └─────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
```

- **Container** — `bg-surface`, `rounded-3xl`, `overflow-hidden`, `shadow-soft`,
  a small inset (`~8px`) from the well so the rounded corners read.
- **Header** — `sticky top-0 z-20`, translucent `bg-surface/85` + `backdrop-blur`,
  `h-14`, `px-6`; a hairline bottom (or a `shadow-soft` that appears only once the
  content scrolls past ~8px — the *condensed* state). Holds page title / search /
  quick actions / theme / notifications / profile.
- **Content** — the only scroll container; `p-6`, `gap-6`. The page-layout order
  from §9.3 lives here.
- **Rule — One-Container Rule.** Header and content live in the same rounded
  surface. Never split them into two stacked boxes with a gap between.

_(Tokens: §LAYOUT.)_

---

## 6. Sidebar — resizable width + collapsible groups

### Draggable width controller
A drag handle on the sidebar's right edge lets the user set its width live.

- **Handle** — a `4px` hit-area on the right edge (≈12px grab zone), `cursor:
  col-resize`; the handle tints `--brand` while grabbed.
- **Range** — drag between `min 200px` and `max 360px` (default `248px`); dragging
  below the min **snaps to the 72px icon rail**. Double-click the handle resets to default.
- **Persist** — remember the chosen width *and* each group's expanded/collapsed
  state per user.
- **Motion** — the panel follows the cursor with **no transition while dragging**
  (feels direct); only the snap-to-rail animates (over `base`).
- **A11y** — the handle is a focusable `separator`; `←/→` nudge 8px, `Home`=min,
  `End`=max, with `aria-valuenow/min/max`.

### Collapsible-by-group navigation (accordion)
For products with many menus, group the nav into **accordion sections** — each a
header row that expands/collapses its items (matching the reference: *Overview ·
CRM · Inventory · Sales & Accounting · Reports & Analytics · System · Help*).

- **Group header** — icon + label (`muted-ink`, `font-semibold`) + a chevron that
  rotates 180° on toggle (`fast`). The section labels are the headers — not dead
  captions.
- **Items** — `rounded-md`, `h-9`, `hover:bg-muted`. **Active item** = the raised
  `sidebar-active` pill + a 2px brand indicator on the leading edge.
- **Sub-items** — nested items indent `12px` behind a vertical guide line
  (`border`); the active sub-item gets brand text + a brand left-rule.
- **Behavior** — the active route's group **auto-opens**; others start collapsed.
  In the **collapsed rail**, groups show icons only and hovering opens a **flyout**
  with that group's items. Per-group open state is remembered.

_(Tokens: §SIDEBAR.)_

---

## 7. Charts — modern, not boring

Charts are the **one** place the ≤10% accent budget is lifted: **data may be
colorful, chrome stays quiet.** Axes, gridlines, and labels remain restrained
(`subtle-ink`, dashed grid at ~30%); the *series* carry the color and gradients.

- **Categorical palette** (§CHARTS.palette) — a vivid, dark-tuned wheel
  (blue · pink · amber · violet · emerald · cyan · rose). One gradient family per chart.
- **Area / line** — 2.5px rounded-cap line, **monotone smoothing** (soft curves),
  a **gradient fill** fading `color/35 → color/0`; the path **draws in once** on
  mount (`chart-draw`). Layer translucent areas to compare two series.
- **Bars** — **rounded top corners (6px)**, ~35% gap, grow from the baseline with
  a light stagger. Stacked + legend for composition.
- **Donut / radial** — thick ring with **rounded segment caps** and a **center
  metric** (big `.tnum` figure + caption, e.g. *"1,125 — Visitors"*); radial gauges
  use a `muted` track + a gradient value arc.
- **Sparklines in stat tiles** — a 28–40px inline mini-chart under the number, a
  faint gradient beneath a 2px line, **no axes/labels** (see the reference tiles).
- **Tooltip** — floating chrome (`bg-surface`, border, `shadow-md`); shows a series
  swatch + label + `.tnum` value. **Legend** — swatch chips; toggling dims a series.
- **States** — loading → shape-matched chart skeleton (§8); empty → centered icon +
  "No data" + one-line CTA; error → inline retry.
- **Rules** — colorful data / quiet chrome; never rainbow the gridlines; always
  pair color with a label/legend (never hue alone); prefer donut-with-center-metric
  and sparkline-in-tile over flat, label-less mystery charts.

_(Tokens: §CHARTS.)_

---

## 8. Motion & Skeletons

### Smooth, light motion
Motion should make the dashboard feel **smooth, never heavy**. GPU-friendly
properties only (`transform` / `opacity`) — never animate layout (width/height/top).

- **Enter** — pages and cards fade + rise `8px/6px` over `base`; grids **stagger
  ~40ms** per item, capped at 6 (beyond that, fade the group as one).
- **Micro-interactions** — `hover-lift` (`-2px` + `shadow-soft→md`, `fast`),
  `press` (`scale 0.98`), `row-hover` (muted wash). Numbers **count up** into stat
  tiles; charts draw in once (above).
- **Budget** — no single transition exceeds `slow` (320ms); no parallax, no
  infinite bounce, no full-page motion. All of it collapses to `0.01ms` under
  `prefers-reduced-motion` (or the manual toggle).

_(Tokens: §MOTION.presets.)_

### Shape-matched skeletons
A skeleton **mirrors the component it replaces** — same box, radius, padding, and
internal slots — so nothing shifts when real data lands.

- A **stat tile** skeleton = label bar + big-number bar + a faint sparkline block,
  on the tile's real grid. A **chart** skeleton = axis ghost + a low-contrast
  silhouette at the chart's real height. A **table row** = one bar per column at
  the real column widths. Cards/chips/avatars inherit the target's radius.
- Subtle 1200ms shimmer; a **static muted block** on reduced-motion.
- **Anti-pattern** — no generic full-width gray rectangles that don't match the
  final layout (they cause a layout jump on load).

_(Tokens: §SKELETON.)_

---

## 9. Dashboard UX Standards

_Enterprise UX contract for every component. Inspired by Notion, Linear, GitHub,
Stripe, Polaris, Atlassian, Figma, Airtable, Slack, Vercel, Retool, and
Microsoft/Google Workspace._

### 9.1 Core principles
1. **Clarity** — a component's purpose is immediately legible; primary actions are visually obvious; avoid needless complexity.
2. **Consistency** — search, save, filters, table toolbars, and modal button placement live in the same place on every page. Users never relearn an interaction.
3. **Efficiency** — minimize clicks: inline editing, keyboard shortcuts, bulk actions, smart defaults, recents, autocomplete, autosave where safe.
4. **Feedback** — every action gets immediate feedback (`Loading → Saving… → Saved`, or `Error → Retry/Undo`). Never leave the user guessing.
5. **Accessibility** — keyboard navigation, screen readers, visible focus, contrast, touch targets, reduced motion, high-zoom.
6. **Forgiveness** — recovery is always possible: undo, restore, autosave, confirmation dialogs, version history, draft recovery.
7. **Performance** — button feedback <100ms, page transitions <300ms; skeletons over spinners; lazy loading; virtual scrolling; optimistic UI where safe.

### 9.2 Universal component requirements
- **States (every interactive component):** default · hover · focus · active · disabled · loading · success · warning · error · empty · selected · read-only.
- **Keyboard:** Tab / Shift+Tab · Enter · Esc · arrows · Space · Home/End (lists) · Ctrl/Cmd shortcuts where applicable.
- **Responsive:** desktop / tablet / mobile — layouts **adapt**, they don't merely shrink.
- **Theme:** light · dark · high-contrast.
- **Motion:** fast, meaningful, interruptible; respects reduced-motion.

### 9.3 Page layout standard
Every page reads top to bottom: **Breadcrumb → Page title → Description (optional)
→ Primary action → Toolbar (Search · Filters · View options · Export) → Content →
Pagination / Footer.** The user always knows *where they are*, *what they're
viewing*, and *what they can do next*.

### 9.4 Tables (the most important dashboard component)
**Required toolbar:** Search · Filters · Sort · Column visibility · Refresh ·
Export · Density switch · Rows-per-page · Pagination.

- **Search** — instant + debounced, result highlighting, clear button, optional recent searches.
- **Filters** — status, tags, date range, owner, custom, saved filters, Clear All; show active count (e.g. `Filters (3)`).
- **Sorting** — single + multi-column, asc/desc, reset.
- **Column management** — show/hide (checkbox list with Select/Deselect all + search), reset to default, auto-saved prefs, drag-reorder, resize, pin left/right. **Never hide all columns; always keep ≥1 identifier column visible.**
- **Row selection** — checkbox, select page / all / filtered results; bulk actions appear only after selection.
- **Bulk actions** — delete · archive · assign · export · move · duplicate · tag · bulk edit.
- **Row actions** — ≤3 inline (e.g. View · Edit · ⋮ overflow: duplicate, archive, delete, history, permissions).
- **Density switch** — comfortable / compact / dense; remembered.
- **Export** — CSV · Excel · PDF (optional) · Copy · Print · Export Selected · Export Filtered.
- **States** — loading (skeleton) · no data · error · no search results · permission restricted · offline.
- **Remembered preferences** — columns · sort · filters · rows-per-page · density · page · pinned columns.

### 9.5 Forms
Title · description · grouped fields · section headers · required indicators ·
helper text · validation · optional autosave · Cancel · Save · success message.
**Large forms:** progress indicator · sticky save bar · autosave · warn before
leaving · draft recovery. **Validation:** real-time · inline messages · highlight
invalid fields · scroll to first error.

### 9.6 Buttons
Variants primary · secondary · ghost · outline · danger · link · icon · split ·
loading · disabled. **Only one primary action per screen. Danger actions require
confirmation** (or an Undo affordance).

### 9.7 Modals & drawers
- **Modal:** title · description · close · Esc · focus trap · sticky footer · Cancel · primary · loading/error/success states. Reach for inline/progressive alternatives before a modal.
- **Drawer:** edit without leaving context — sticky header + footer · Save/Cancel · autofocus · responsive width.

### 9.8 Other patterns
- **Cards:** title · subtitle · actions · status badge · menu · loading · empty · hover · clickable area.
- **Search (global):** recents · pinned · keyboard shortcut · suggestions · empty state.
- **Navigation:** collapsible/nested sidebar with tooltips, favorites, recents, org switcher, profile, logout; top nav with search, notifications, help, theme switch, profile; command palette.
- **Notifications:** unread badge · grouping · mark-all-read · filters · deep links · archive.
- **Activity timeline:** created · updated · deleted · comments · mentions · attachments · user actions.
- **File upload:** drag & drop · browse · progress · preview · retry · cancel · replace · validation.
- **Charts:** tooltip · legend · export · empty · loading · fullscreen · responsive.
- **Empty states:** illustration · explanation · CTA · docs link. _e.g._ "No invoices yet. Create your first invoice to get started. [ Create Invoice ]".
- **Error states:** what happened · why · how to fix · Retry · optional Error ID. Prefer skeletons over spinners.
- **Confirmations:** required before delete · archive · remove · transfer ownership · reset settings · clear data. **Prefer Undo over a confirmation dialog** where safe — _e.g._ "Deleted project — Undo (10s)".

---

## 10. Do's and Don'ts

### Do
- **Do obey the ten UX Laws (§12) on every feature** — they're non-negotiable and beat any preference or novelty; check a feature against all ten before calling it done.
- **Do UNIFY components first, THEN apply tokens.** On any revamp, consolidate every
  duplicate/one-off into a single `ui/*` primitive before re-skinning — theme once, not five times (see §0 Unify-First Rule).
- **Do** carry the accent through the `--brand` token only, on ≤10% of a screen (charts are the sanctioned exception — see §7).
- **Do** obey the project's `surfaceMode` on every card, input, and table — one border story, no ad-hoc mixing.
- **Do** define depth with tone + soft shadow (+ hairline when enabled) — keep surfaces flat, never a gradient.
- **Do** keep the header + content in one rounded container with a fixed header (§5).
- **Do** make the sidebar resizable (drag handle, remembered width) and group its menus into collapsible accordion sections (§6).
- **Do** keep dashboard motion smooth-but-light — `transform`/`opacity` only, ≤`slow`, staggered enters, no heavy effects (§8).
- **Do** shape-match every skeleton to the component it replaces so nothing shifts on load (§8).
- **Do** make charts modern — gradient fills, rounded bars, center-metric donuts, sparklines in tiles; colorful data, quiet chrome (§7).
- **Do** reuse `ui/*` primitives; one button shape, one field vocabulary, one status-chip system everywhere.
- **Do** code-split around the reusable components — lazy-load heavy primitives (charts, data-table, editors, modals) behind a shape-matched skeleton, keep `ui/*`+vendor in a cached shared chunk (§11).
- **Do** run the dead-code scan and delete what it finds — zero unused exports / orphan files / duplicate primitives before "done" (§11).
- **Do** use the real type/space/radius scales and comfortable 8pt spacing; ≥44px touch targets on mobile.
- **Do** ship every interactive state + skeleton loaders; give every action immediate feedback.
- **Do** honor reduced motion (query + manual toggle) and hit `WCAG 2.1 AA` (body ≥4.5:1).
- **Do** resolve the Decision Gate (§0) before building, and echo the final `config:` back for confirmation.

### Don't
- **Don't** re-skin components before unifying them — that just re-themes the same duplicated button five times.
- **Don't** ship the generic AI-slop dashboard — no purple→blue gradients, gradient text, glassmorphism, identical icon-card grids, or the big-number hero-metric template.
- **Don't** split the header and content into two stacked boxes — they share one rounded container (§5).
- **Don't** put a stroke on a card/field/table when `surfaceMode: borderless`, or remove one when `hairline` — follow the mode.
- **Don't** overdo motion — no parallax, infinite bounce, full-page animation, or animating layout (width/height/top).
- **Don't** use generic full-width gray skeleton rectangles that don't match the final layout.
- **Don't** rainbow chart chrome (gridlines/axes/labels) or rely on hue alone — always pair color with a label/legend.
- **Don't** use a colored `border-left/right > 1px` accent stripe on cards/alerts.
- **Don't** inline a brand hex or any raw color/spacing/radius — always the token.
- **Don't** convey status with color alone — status chips always carry an icon + label.
- **Don't** fork a `ui/*` component or reinvent standard affordances (custom scrollbars, weird form controls).
- **Don't** reach for a modal before exhausting inline/progressive alternatives.
- **Don't** duplicate/fork a primitive or leave dead code, orphan files, or unused deps in the tree — the scan must come back clean.
- **Don't** ship the chart library, editor, or other heavyweight in the initial bundle — lazy-load it (§11).
- **Don't** break a UX Law (§12) for the sake of a novel or "cleaner-looking" idea — the law wins.
- **Don't** go consumer/playful (cartoon rounding, rainbow, emoji-as-icons) or dated Bootstrap-gray.

---

## 11. Code Architecture — reusable-component code-splitting & dead-code hygiene

Two enforced exit-gate concerns that keep every project fast and clean. The
reusable component is both the **unit of reuse** and the **unit of the split**.

### 11.1 Reuse-first structure
- `components/ui/*` — atoms/molecules (button, card, input, chip, table, chart, skeleton).
- `components/patterns/*` — composed reusables (data-table, stat-tile, page-header, empty-state).
- `features/<domain>/*` — feature code that **composes** the above; it holds no restyled primitives.
- `lib/ · hooks/ · types/` — shared logic, no UI.
- One index barrel per folder for clean imports — but import heavy items **directly** so they stay tree-shakeable.

### 11.2 Code-splitting (`config.codeSplitting`)
- **`route`** — every page/route is its own lazy chunk (`React.lazy`/`Suspense`, `next/dynamic`, `defineAsyncComponent`).
- **`component`** *(default)* — route-level **plus** lazy-loading the heavy reusable
  components on the views that use them: **charts** (the chart lib is large),
  **data-table**, rich **editors**, **date/time pickers**, **file-upload**, the
  **command palette**, and **modals/drawers/sheets** (mount on open, not on load).
- **`aggressive`** — component-level plus granular chunks per primitive group and
  **prefetch on intent** (hover/focus/viewport).
- **`off`** — single bundle; only for tiny apps.
- **Every lazy boundary** shows a **shape-matched §8 skeleton** — never a spinner, never a layout shift.
- `ui/*` primitives + vendor sit in a **stable shared chunk** so they cache across routes; **import icons per-name**, never the whole set.
- **Budgets:** initial JS ≤ ~200KB gzip · per-route ≤ ~120KB gzip · chart chunk excluded from initial.

### 11.3 Dead-code scan (`config.deadCodeScan` — must pass before "done")
Scan and **delete**, don't defer. Targets:
- unused **exports & files** (orphans), unreferenced components, **duplicate/forked primitives** (fold into the canonical one);
- unused **dependencies**, unreachable branches, commented-out blocks;
- unused **design tokens / CSS** and unused Tailwind classes (purge).

**Tools:** `knip` or `ts-prune` (unused exports/files) · `depcheck` (deps) ·
`eslint` `no-unused-vars` · Tailwind/PurgeCSS · a bundle analyzer.
**Rule:** the scan reports **zero** unused exports/orphans before the UI is marked
done. **Caution:** confirm a symbol is truly unreferenced (grep **and** tool agree)
before deleting, and keep intentional public API on an allowlist.

_(Tokens: §ARCHITECTURE.)_

---

## 12. UX Laws — the non-negotiable constitution

Components and tokens decide how the product *looks*; the **UX Laws** decide how it
*behaves*. These ten are **non-negotiable** — every new feature must satisfy all of
them regardless of `config`. They are a shared decision-making framework for
designers **and** developers: when two approaches are on the table, the one that
honors the laws wins, and **a law always beats a preference or a novelty**. This is
what makes features built by different people, at different times, feel like they
belong to the same product — and it is what protects the **user's journey** end to
end. Treat each law as an acceptance criterion, not an aspiration.

| # | Law | What it means for the user journey | How to honor it |
|---|-----|-----------------------------------|-----------------|
| **L1** | **Every page answers *Where am I? · What can I do? · What's next?*** | The user is never lost or stuck guessing the next step. | Breadcrumb + page title + description (where), a visible primary action + toolbar (what), and a clear next step / follow-on CTA or empty-state guidance (next). See §9.3. |
| **L2** | **Never make users lose their work.** | Trust — no dread that a misclick, refresh, or timeout wipes effort. | Autosave / draft recovery, "warn before leaving" on dirty forms, **Undo** over destructive confirms, optimistic UI with rollback, restore & version history. |
| **L3** | **Every data view supports search and filtering.** | Users find the row/record they came for instead of scrolling. | The §9.4 table toolbar (instant+debounced search, filters with a count, saved filters, Clear All) on every list/grid — not just the "main" table. |
| **L4** | **Prefer inline editing over unnecessary navigation.** | Fewer detours; the user stays in context and in flow. | Edit in place / in a drawer; reserve full-page navigation for genuinely separate contexts. Don't send someone to a new screen to change one field. |
| **L5** | **Never ship a component without loading, empty, and error states.** | The journey holds together on slow networks, first-run, and failure — not just the happy path. | Every data component ships shape-matched **loading** (§8 skeleton), a helpful **empty** state (illustration + explanation + CTA), and a recoverable **error** state (what/why/how + Retry). No happy-path-only screens. |
| **L6** | **Remember user preferences whenever it improves efficiency.** | The product adapts to the person; repeat work gets faster. | Persist columns, sort, filters, density, rows-per-page, pinned columns, **sidebar width + collapsed groups** (§6), theme, and last-used values. Return them to where they left off. |
| **L7** | **Make the primary action visible without scrolling whenever practical.** | The main thing to do is obvious and reachable immediately. | Put the one primary action in the header / above the fold; on long forms use a **sticky save bar**. One primary per screen (§9.6). |
| **L8** | **Every interaction provides immediate feedback.** | The user always knows their action registered — no dead clicks, no doubt. | <100ms visual response; `Loading → Saving… → Saved`; toasts for background results; hover/press/active states; count-up + skeletons (§8). Never leave them wondering whether it worked. |
| **L9** | **Optimize for keyboard users as well as mouse users.** | Power users and assistive-tech users move fast; the journey isn't mouse-only. | Full keyboard paths (Tab/Shift+Tab, Enter, Esc, arrows, Space, Home/End), visible focus, shortcuts, a **command palette**, and a focusable resize handle (§6). Meets `WCAG 2.1 AA`. |
| **L10** | **Consistency takes precedence over novelty.** | Learn an interaction once, reuse it everywhere — no relearning per page. | Search, save, filters, toolbars, and modal button placement live in the **same place** on every page; reuse `ui/*` primitives (Unify-First). Don't invent a clever one-off when a standard pattern exists. |

**How to use the laws**
- **Design & build:** treat the table above as a checklist for every feature — a
  feature isn't done until it passes all ten (add them to PR/review templates).
- **Deciding between options:** when preferences clash, pick the option that
  satisfies more laws; if a novelty breaks L10, drop the novelty.
- **They compound with the gate:** the §0 Decision Gate tunes *this project's* look
  and process; the UX Laws are the constant that never changes between projects.

---

## Appendix A — Presets (proven configs)

Copy one into `config:` as a starting point, then run the Decision Gate to adjust.

**Preset "Calm" (DB Auto Trading)** — softer, consumer-leaning:
```yaml
config: { surfaceMode: borderless, accent: red, fontFamily: "Bricolage Grotesque",
          themeDefault: dark, density: comfortable, radiusScale: medium, motion: standard }
```

**Preset "Instrument" (Atlas)** — crisp, data-dense, canonical:
```yaml
config: { surfaceMode: hairline, accent: gold, fontFamily: "Geist",
          themeDefault: dark, density: comfortable, radiusScale: soft, motion: standard }
```

## Appendix B — Emit checklist (when standing up a project)
_Entry gate:_
- [ ] Decision Gate resolved + `config:` confirmed with the human.
- [ ] Components unified into canonical `ui/*` primitives **before** tokenizing (Unify-First Rule).
- [ ] Tokens emitted to `globals.css` (`:root` light / `.dark` dark) + shadcn bridge.
- [ ] `--brand` wired from the active accent; theme + accent switch verified live.
- [ ] `surfaceMode` applied uniformly to card / input / table primitives.
- [ ] Type/space/radius scales wired to Tailwind theme; no arbitrary values.
- [ ] App shell = one rounded container + fixed header; sidebar resizable + collapsible groups.
- [ ] Charts modernized (gradient fills, rounded bars, center-metric donuts, sparklines).
- [ ] All interactive states + shape-matched skeletons implemented; reduced-motion honored.
- [ ] Contrast audited to `WCAG 2.1 AA`; ≥44px touch targets on mobile.

_Exit gate (§11 + §12):_
- [ ] Code-splitting wired around reusable components; heavy primitives lazy; bundle within budget.
- [ ] Dead-code scan clean — zero unused exports / orphan files / duplicate primitives / unused deps.
- [ ] **All ten UX Laws (§12) satisfied** — L1 wayfinding · L2 no lost work · L3 search/filter · L4 inline edit · L5 loading/empty/error · L6 remembered prefs · L7 primary above fold · L8 immediate feedback · L9 keyboard parity · L10 consistency.

---

_This Divergent design system merges and improves `socia-dbauto/DESIGN.md` (borderless "Calm
Instrument") and `socia-atlas/Design.md` (hairline "Operations Instrument") into
one reusable, configurable token + UX contract. Set `config:`, resolve the
Decision Gate, and build._
