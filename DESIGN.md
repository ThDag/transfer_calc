---
name: Commission Calculator
description: A quiet, orderly black-and-white offline havale commission calculator with one calm sage accent on the owner's own earnings.
colors:
  void-black: "#000000"
  signal-white: "#ffffff"
  concrete-gray: "#999999"
  calm-sage: "#87a96b"
typography:
  label:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif"
    fontSize: "13px"
    fontWeight: 800
    lineHeight: 1.2
    letterSpacing: "1.5px"
  numeric:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif"
    fontSize: "28px"
    fontWeight: 800
    lineHeight: 1.2
    letterSpacing: "normal"
rounded:
  none: "0px"
spacing:
  sm: "8px"
  md: "14px"
  lg: "24px"
components:
  card:
    backgroundColor: "{colors.void-black}"
    textColor: "{colors.signal-white}"
    rounded: "{rounded.none}"
    padding: "{spacing.lg}"
  input-field:
    backgroundColor: "{colors.void-black}"
    textColor: "{colors.signal-white}"
    typography: "{typography.numeric}"
    rounded: "{rounded.none}"
    padding: "12px 14px 12px 34px"
  total-block:
    backgroundColor: "{colors.calm-sage}"
    textColor: "{colors.void-black}"
    typography: "{typography.numeric}"
    rounded: "{rounded.none}"
    padding: "14px 16px"
---

# Design System: Commission Calculator

## Overview

**Creative North Star: "The Trading Terminal"**

Everything here reads like a clean instrument panel: black field, crisp white rules, tracked uppercase labels, and exactly one calm accent reserved for the number that matters. There's no ornament and no rounded softness, but the tone is measured rather than urgent — a quiet readout, not a warning light. Weight does the talking: nothing on screen is drawn lighter than 700, so even the smallest label stays clear and legible without needing to shout.

The current palette and rates are an explicit first pass (see PRODUCT.md) — the structure and material language below are the durable part; the exact accent hue and rate values may still move.

**Key Characteristics:**
- Void-black field with crisp white 2-3px rules standing in for all depth and separation
- Exactly one calm accent color (Sage), spent only on Our Comm, the owner's own earnings
- All type at 700-900 weight; uppercase, tracked labels; tabular numerals throughout
- Zero border-radius everywhere — every shape is a clean rectangle
- No shadows at all — the border alone defines the card; function over form

## Colors

Two neutrals, one accent, spent once.

### Primary
- **Sage** (#87a96b): a calm, muted green reserved entirely for the Our Comm block — the owner's own earnings, and the one filled surface on the page. Never used for chrome, borders, Received Amount, Comm Main, or Service Comm.

### Neutral
- **Void Black** (#000000): page and card background.
- **Signal White** (#ffffff): primary text, all borders and rules, input value text.
- **Concrete Gray** (#999999): secondary text — field label, row captions, placeholder text.

### Named Rules
**The One Accent Rule.** The accent color appears in exactly one place: Our Comm, the owner's own earnings. Everywhere else — including Received Amount and Comm Main, both larger dollar figures — stays black, white, or gray, so when the accent shows up it simply marks "this is the number that's actually yours," calmly rather than urgently.

## Typography

**Body/Display Font:** system font stack (`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`) — deliberately not a custom webfont, so the service worker never needs a network font fetch to render offline. Revisit with a self-hosted display face via a future typeset pass once the visual world is confirmed beyond this first placeholder build.

**Character:** clear and even-weighted, no soft edges — every size step is bold-to-black weight, uppercase tracking on labels, tabular numerals on every value.

### Hierarchy
- **Label** (800, 13px, 1.5px tracking, uppercase): field name and row captions.
- **Numeric** (800–900, 24–28px, tabular-nums): the price input and every computed value, including the total.

### Named Rules
**The No Regular Weight Rule.** Nothing on this screen is drawn below font-weight 700 — thin or regular type stays out of this system, keeping labels legible without any of them needing to be loud about it.

## Layout

Single centered console, no page grid: a 380px-max-width bordered card sits near the top of an otherwise empty black viewport. Vertical rhythm is deliberately coarse — 24px card padding, 14px gap between breakdown rows, a 2px white rule before the breakdown. Density stays low so the one accented block reads clearly.

Information is arranged as a real tree, not a flat list, because the underlying math is a tree: three amount inputs — USD, AED, Received Amount — are stacked together as one fully bidirectional group (editing any one back-solves the other two), sitting above two adjustable rate inputs (Service Comm %, Our Comm %) side by side. Below the divider, Comm Main is the derived total, and Service Comm is drawn nested *under* it (indented, connected by a left rule) because it is Comm Main's child by construction (Comm Main is defined as the sum of Service Comm and Our Comm, never an independent figure). The nesting is the documentation: a reader should be able to tell the calculation's shape from the indentation alone, without reading a label.

### Named Rules
**The Shape-Is-The-Math Rule.** A value that is derived as the sum of two other values is drawn nested under them, connected by a rule. Flattening a parent/child relationship into a plain list is a layout bug, not a style choice. **Exception:** the One Accent Rule outranks this one for exactly one row — Our Comm is mathematically Comm Main's child, but is promoted out of the nest into its own block because it's the headline number. A promotion needs the One Accent Rule's justification; it is not a second precedent for un-nesting anything else.

## Elevation & Depth

No shadows anywhere, hard or soft. Depth is not simulated — the card is flat black on flat black, defined only by its border. Function over form: nothing is spent on a depth cue that doesn't help the number get read.

### Named Rules
**The No Decoration Rule.** A border defines a boundary; it does not also need a shadow to prove it. If a visual device isn't load-bearing for reading the numbers, it doesn't ship.

## Shapes

Zero border-radius everywhere — the card, the input, and the total block are all clean rectangles. Solid 2–3px borders (white on black for the card and input; black on sage for the total block) stand in for the softness a shadow would otherwise supply.

## Components

### Cards / Containers
- **Corner Style:** square (0px)
- **Background:** Void Black
- **Border:** 3px solid Signal White
- **Shadow Strategy:** none (see Elevation)
- **Internal Padding:** 24px

### Inputs / Fields
- **Style:** 2px solid Signal White border, Void Black fill, 800-weight white numerals, thousands-comma-grouped as you type
- **Focus:** border and outline switch to Sage
- **Placeholder:** Concrete Gray
- **Linked amount group** (USD, AED, Received Amount): three full-size fields stacked together, all fully bidirectional — editing any one recomputes the other two and the whole breakdown below.
- **Rate variant** (Service Comm %, Our Comm %): the same field at a smaller scale (18px value, `%` prefix) — two sit side by side in one row since they're peers, not a sequence.
- **Comma formatting is native to every field here**, not a separate feature: it live-reformats while typing (cursor preserved) and settles to a clean 2-decimal value on blur.

### Nested Breakdown Row
- **Style:** a 2px Signal White left rule with 14px padding, holding one or more standard rows
- **Use:** wraps any figure that is mathematically a component of the row above it (currently: Service Comm, a child of Comm Main — Our Comm is also Comm Main's child but is promoted out per the Shape-Is-The-Math Rule's exception)

### Our Comm Block (the headline total)
- **Style:** solid Sage fill, 2px Void Black border — the one filled surface on the page
- **Weight:** 900 / 28px value, 800 / 14px uppercase label, both in Void Black for readable contrast on sage
- **Why this row:** it's the owner's own earnings, not the largest number on the page — the accent marks ownership, not magnitude, and it does so calmly.

## Do's and Don'ts

### Do:
- **Do** keep the accent confined to the Our Comm block — the One Accent Rule.
- **Do** keep every border and rule hard-edged (0px radius) and at least 2px thick.
- **Do** keep text weight at 700+ everywhere; no thin or regular type.
- **Do** draw a derived value nested under the values it's derived from — the Shape-Is-The-Math Rule.

### Don't:
- **Don't** add any shadow, soft or hard — the border alone defines the card; depth cues are decoration this system doesn't spend on.
- **Don't** load an external webfont; the app must render and cache fully offline.
- **Don't** give Comm Main its own independently-editable rate — it must always equal Service Comm + Our Comm exactly, per PRODUCT.md's calculation model.
- **Don't** reach for an urgent or alarm-style color for the accent — the whole point of this palette is that the one signal color reads as calm, not urgent.
