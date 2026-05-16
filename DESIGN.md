# DESIGN — The Ledger

A working design document for the dashboard. Captures the aesthetic direction, the type and colour systems, the component patterns, and the explicit anti-patterns the design pushes against.

## 1 · Direction

> A personal finance dashboard, set as if your money kept its own newspaper.

The Ledger treats Anna Hartmann's finances as a workspace she visits every morning, not a feed she scrolls. The visual language borrows from three sources:

1. **Financial newspapers** (FT, Bloomberg op-eds, The Economist) for typographic hierarchy, hairline rules, and editorial restraint.
2. **Victorian double-entry ledgers** for the transactions table, the running balance column, and the parchment-and-ink palette.
3. **Modern productivity apps** (Linear, Cursor, Things) for the persistent sidebar, sticky status bar with shortcut hints, and density.

The intent is editorial substance without editorial passivity. The Coach issues actionable nudges, not essays. Marginalia explain the data inline rather than in a separate "insights" tab. Sections are numbered § i through § viii in the sidebar so the structure of the app reads like a table of contents.

## 2 · Typography

Two free Google Fonts, both variable, both with strong italic and optical-size axes.

| Role | Family | Notes |
|------|--------|-------|
| Display | **Fraunces** | High-contrast modern serif. Used with `opsz` 24–144 and `SOFT` 30–100 for tone shifts. Italic is critical: used for emphasis (`<em>`), eyebrows, marginalia. |
| Body | **Newsreader** | Editorial serif designed for screen reading. Optical size axis lets body and small captions render correctly at different sizes. |

### Scale

Fluid, declared with `clamp()`. The hero metric scales from 56 px on phone to 116 px on widescreen.

```
hero number       clamp(56px, 8.5vw, 116px)   Fraunces, opsz 144, SOFT 30
section heading   clamp(20px, 1.9vw, 26px)    Fraunces, opsz 60, SOFT 30
coach headline    clamp(22px, 2.1vw, 27px)    Fraunces, opsz 48, SOFT 35
body              13.5–14 px                  Newsreader, opsz 16–18
metadata          10.5–11 px, all-smallcaps   Newsreader, letter-spacing 0.18em
marginalia        12.5 px italic              Fraunces, opsz 14, SOFT 80
```

### Numerals

All financial figures use `font-variant-numeric: lining-nums tabular-nums` so columns of money line up. The cents portion is rendered at `0.42em` of the integer size with a muted colour to keep the eye on the integer.

### Rules

- Body text never goes below 13 px.
- Smallcaps reserved for labels, never body.
- Italic is meaningful — never decorative. Eyebrow and marginalia italics signal a different register.
- No system fonts, no Inter, no monospace as a "technical" affectation.

## 3 · Colour

Two tinted neutrals and three accents, all defined in `oklch()` for perceptually uniform tints. No pure black, no pure white.

```css
--paper:       oklch(94% 0.018 80);   /* warm parchment */
--paper-deep:  oklch(91% 0.022 76);   /* sidebar, status bar */
--paper-sunk:  oklch(88% 0.024 74);   /* hover background, badges */

--ink:         oklch(18% 0.020 50);   /* primary text, deep warm */
--ink-soft:    oklch(36% 0.014 50);   /* secondary text, body */
--ink-mute:    oklch(54% 0.012 55);   /* labels, captions */

--red:         oklch(40% 0.13 30);    /* oxblood — debit, alert */
--blue:        oklch(38% 0.06 225);   /* ledger blue — credit, positive */
--gold:        oklch(60% 0.10 78);    /* warning, near-limit */
```

### Semantic mapping

| Token | Used for |
|-------|----------|
| `--red` | Debits in the ledger, over-budget bars, alert notes, contribute-now CTAs, the active-nav left bar |
| `--blue` | Credits, positive deltas, income line in cashflow, on-pace pills |
| `--gold` | Warning state (88 % budget used), tariff change notice |

### Discipline

- Tint your neutrals toward the brand hue. The paper neutrals are oklch hue 76–80 (warm), the ink neutrals 50–55 (warm). Cool greys would fight the warm accents.
- Never use gradient text or rainbow gradients. Money is serious; the typography is the impact.
- Avoid red for ordinary spending — only for *alerts* and *debits*. A line item being a debit is structural information, not a warning.

## 4 · Layout

A two-column grid:

```
┌────────────────┬────────────────────────────────────────┐
│                │ topbar  (sticky, 56 px)                │
│ sidebar        ├────────────────────────────────────────┤
│ (sticky,       │                                        │
│  256 px,       │ main scrollable content                │
│  full-height)  │                                        │
│                ├────────────────────────────────────────┤
│                │ status bar  (sticky bottom, 38 px)     │
└────────────────┴────────────────────────────────────────┘
```

### Sidebar

- 256 px wide, sticky, full viewport height.
- Brand at top with version stamp.
- Section nav (§ i Today → § vii Reports), each row showing roman number, name, and a status hint (date, count, alert).
- Accounts list — 4 connected accounts with name, last four, balance, and trend mark (▲ ▼ —).
- User card pinned at the bottom with settings icon.

### Topbar

Carries the section title with a "since you last looked" sub-line, search (⌘K focused), notifications, and the primary "+ New" CTA.

### Main

Sections separated by 1 px hairlines, never cards. Each section has its own `§` number marker in the heading, echoing the sidebar.

### Status bar

The signature app element. Three zones:

1. **Sync pill** — pulsing dot, "4 accounts synced · 09.42 CET".
2. **Live ticker** — net worth + delta, spending pace, next bill due.
3. **Shortcuts** — `⌘K Search` `⌘N New` `G Go to` `? Help`.

The status bar is the part that says "this is an app, not a webpage."

### Responsive

| Width | Adjustment |
|-------|-----------|
| ≤ 1240 px | Hero collapses to single column; marginalia hidden (too cramped to land) |
| ≤ 1080 px | Middle/double sections stack; coach + goals stack vertically |
| ≤ 960 px  | Sidebar becomes an off-canvas drawer; status bar hides ticker and shortcuts |
| ≤ 720 px  | Hero-split becomes single column; alerts wrap; topbar search hidden |

## 5 · Density & rhythm

Density sits between Linear (tight) and Stripe (airy). The principles:

- **Section padding 20 px**, not 32 px. App, not landing page.
- **Hairlines do the work of borders**. Cards almost never appear. When a card-like enclosure is necessary (Coach notes), it's drawn with a 2 px coloured top rule and no other edges.
- **Drop shadows are forbidden**. The only depth comes from paper tone differences between `--paper`, `--paper-deep`, and `--paper-sunk`.
- **Vertical rhythm comes from typography**, not from margins. A section heading sets its own air via line-height and the hairline rule below it.

## 6 · Components

### Marginalia (signature)

Small italic notes set in Fraunces with a left hairline, separated from the data by 14 px. The arrow glyph `→` in oxblood. Used for context the user couldn't trivially compute themselves.

```html
<aside class="marginalia">
  <strong>Ahead of trend.</strong>
  Your 6-month average net-worth growth is €1,210/mo;
  you are at €1,420 with three weeks to go.
</aside>
```

Hidden on screens below 1240 px to avoid cramping.

### Hero metric

Large Fraunces number with a smaller muted cents fraction, a kicker eyebrow with "since you last looked" diff, a one-line trend delta, and a three-up Assets / Liabilities / Savings split below it. The marginalia sits on the right at desktop.

### Section heads

```
§ i  Today
     Monday morning, 11 May
─────────────────────────────────────────
```

The `§` number sits left of the title in Fraunces italic muted ink. Heavy 1 px ink rule below. Right-aligned controls (tabs, chips, link action).

### Buttons

| Variant | Use |
|---------|-----|
| `.btn-primary` | The single hero action per surface. Fills with `--ink`, paper text, hovers to oxblood. |
| `.btn` | Ghost button. 1 px ink border, paper background, hover fades to paper-soft. |
| `.link-action` | Underlined uppercase link with oxblood arrow. The default "open detail" affordance. |
| `.btn-icon` | 32 px square with hairline border. Notification bell with corner red badge. |

Buttons are never rounded beyond 0 px. The square corners reinforce the printed-page metaphor.

### The Ledger (transactions table)

Real double-entry layout: Date · Entry · Debit · Credit · Balance. Debits in oxblood, credits in ledger blue, balance in Fraunces. Row-hover surfaces three quick actions (Categorise / Split / Cancel) that fade in via opacity. The newest entry since last view carries a 4 × 4 px red square in the gutter.

```
Date     Entry                              Debit    Credit   Balance
─────    ─────────────────────────────────  ──────   ──────   ─────────
10 May   Spotify Premium  · Subscription    −9.99    —        €12,408.71
09 May   Rewe Supermarket · Groceries · ca −48.20    —        €12,418.70
09 May   Employer payroll · Salary           —     +4,820.00  €12,466.90
```

### Budgets

Each row carries a 2 px ink bar that fills horizontally. When over budget, the bar turns oxblood and grows to 3 px, and a tick marker at the original budget position appears with a tiny "budget" label above it. The "Adjust" link fades in on row hover.

### Goals

Three columns separated by hairlines, no enclosing card. Each goal shows § roman, name (with italic word), progress fraction, hairline bar, ETA, and a one-line pace verdict ("on pace" or "behind"). "+ Contribute" link aligned to bottom-left.

### Coach notes

Three columns. Each has a 2 px coloured top rule (oxblood for Alert, blue for Action, gold for Heads-up), a small uppercase kicker with a roman number, a Fraunces headline, body prose, a `coach-stat` panel showing the headline figure in big serif, and two CTAs (primary + ghost).

The coach is the only place where 3-line body copy is allowed. The discipline elsewhere: data first, never prose.

### Notices feed

Date-labelled alert rows with a left "kind" column (uppercase coloured), a body cell with bold figures, and a right action link. Four kinds: `info`, `warn`, `due`, `win`.

## 7 · Motion

Restraint and orchestration over micro-interactions.

- **Page load** — staggered fade-up reveal of each section (60 ms increments, `cubic-bezier(0.16, 1, 0.3, 1)`).
- **Bars** — Distribution and budget bars draw left-to-right via `transform: scaleX()` over 800–1200 ms on the same curve.
- **Sync dot** — `box-shadow` pulse, 2.6 s ease-in-out, infinite. The only continuous motion.
- **Hover states** — 180 ms ease-out colour / background changes. Never animated layout properties.
- **Bounce / elastic easing forbidden**. Real objects decelerate smoothly.
- **`prefers-reduced-motion: reduce`** disables every transition and animation globally.

## 8 · Accessibility

- SVG charts carry `<title>` and `<desc>` for screen readers.
- `aria-current="page"` on the active sidebar nav.
- `aria-live="polite"` on the status-bar ticker.
- Filter chips and period tabs are real `<input type="radio">` with associated `<label>` elements.
- Keyboard: `⌘K` focuses search.
- Colour contrast — ink on paper is 12.4 : 1, ink-soft on paper is 7.1 : 1. Oxblood on paper is 6.4 : 1.

## 9 · Anti-patterns

What this design explicitly does not do. These are the fingerprints of generic AI-generated dashboards from 2024–2025, and avoiding them is part of the design.

| Forbidden | Why |
|-----------|-----|
| Drop shadows | Print metaphor; depth comes from paper tones |
| Rounded corners on rectangles | The page is set, not cushioned |
| Gradient text / metric gradients | Decoration without meaning |
| Glassmorphism, frosted cards | "Cool without being designed" |
| Generic SaaS card grids | Repetition disguised as structure |
| Big emoji or rounded-corner icons above headings | Templated 2024 default |
| Inter as a body font | Overused, signals "AI made this" |
| Monospace for "technical" feel | Cheap shorthand |
| Bouncing easing curves | Toy-like; real objects don't bounce |
| Dark-mode-by-default with neon accents | The "didn't decide" default |
| Red for spending decreases | Red is alert, blue is credit / good |

## 10 · References

- Massimo Vignelli, *The Subway Map* (1972) — hairline rules and disciplined typography
- Financial Times back page editorials — multi-column hairline grids
- Linear app — sidebar density, command palette discipline, status indicators
- Bloomberg terminal — accepted information density in finance UIs
- Erik Spiekermann, *Stop Stealing Sheep* — for the type-pairing logic
- *The Manual of Style and Usage* (NYT) — for tone of voice in marginalia and coach copy

## 11 · Tokens (one-line summary)

```
type        Fraunces (display) · Newsreader (body)
palette     parchment + ink + oxblood / blue / gold
unit        2 px hairlines, 1 px rule-soft, 20 px section padding
radius      0 across the board, no exceptions
motion      cubic-bezier(0.16, 1, 0.3, 1), 180–1200 ms
shadow      none — depth from paper tones only
```
