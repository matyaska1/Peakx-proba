# The Ledger

A personal finance management dashboard, set as if your money kept its own newspaper.

Editorial typography, ledger-paper density, marginalia. Built as an app, not a magazine: persistent sidebar, sticky bottom status bar with a live ticker, double-entry transactions table, and a Coach panel that issues actionable nudges rather than essays.

## Live

https://ledger-test.peakx.ai

## At a glance

- **Sidebar (§ i–§ vii)** holds the section nav and the four connected accounts with running balances and trend marks.
- **Sticky status bar** carries a live ticker (net worth, spending pace, next bill) and keyboard shortcut hints (⌘K, ⌘N, G, ?).
- **Hero** treats Net Worth as the primary number with editorial marginalia annotating it ("ahead of trend; six-month average is €1,210/mo").
- **The Ledger** (transactions) is laid out as actual double-entry bookkeeping — Date · Entry · Debit · Credit · Running Balance — with row-hover Categorise / Split / Cancel.
- **Budgets** show projected month-end positions, not just bars: "projecting €52 under by month end" / "first overshoot since February".
- **Goals** carry pace verdicts ("On pace · €475/mo at current rate" vs "Behind · +€85/mo to land on time").
- **Coach** is three colour-coded action notes (alert / sweep / heads-up) with a primary CTA each, not a column of prose.
- **Notices** are dated alerts (tariff changes, upcoming renewals, milestones).

## Type & colour

- **Display**: Fraunces (variable, italic + SOFT axis)
- **Body**: Newsreader (variable, optical size axis)
- **Numerals**: tabular lining figures throughout
- **Palette**: parchment paper + warm ink + three accents — oxblood (debit / alert), ledger blue (credit / positive), gold (warn), olive (confirmation). All defined in `oklch()` for perceptually uniform tints; no pure black or white.

## Stack

- Single HTML file, no build step, no framework
- Modern CSS: `oklch()`, `color-mix()`, `clamp()`, container queries, `prefers-reduced-motion`
- Vanilla JS (~40 lines): filter chips, period tabs, ⌘K search focus
- Pure SVG charts with proper `<title>` / `<desc>` for accessibility

## Local

```bash
open the-ledger.html
```

No server, no install. The file is self-contained except for two Google Fonts.

## Background

Built as a redesign exercise against a prior generic SaaS PFM prototype. The brief asked for an audit + Figma rebuild; this repository captures the design direction taken in HTML so the typography and motion read correctly in motion, not just in static frames.
