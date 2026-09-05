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
---

# Design System: Commission Calculator

## Overview

**Creative North Star: "The Trading Terminal"**

Everything here reads like a clean instrument panel: black field, crisp white rules, tracked uppercase labels, and exactly one calm accent reserved for the number that matters. There's no ornament and no rounded softness, but the tone is measured rather than urgent — a quiet readout, not a warning light. Weight does the talking: nothing on screen is drawn lighter than 700, so even the smallest label stays clear and legible without needing to shout.

The current palette and rates are an explicit first pass (see PRODUCT.md) — the structure and material language below are the durable part; the exact accent hue and rate values may still move.

**Key Characteristics:**
- Void-black field with crisp white 2-3px rules standing in for all depth and separation
- Exactly one calm accent color (Sage), spent only on Our Comm's row text, the owner's own earnings
- All type at 700-900 weight; uppercase, tracked labels; tabular numerals throughout
- Zero border-radius everywhere — every shape is a clean rectangle
- No shadows at all — the border alone defines the card; function over form
- Every percentage names what it applies to, and every sum is drawn as a literal addition (a "+" between its parts, a rule, then the total) rather than left implicit

## Colors

Two neutrals, one accent, spent once.

### Primary
- **Sage** (#87a96b): a calm, muted green reserved entirely for the Our Comm row's label and value text — the owner's own earnings. Never used for chrome, borders, Received Amount, Comm Main, or Service Comm.

### Neutral
- **Void Black** (#000000): page and card background.
- **Signal White** (#ffffff): primary text, all borders and rules, input value text.
- **Concrete Gray** (#999999): secondary text — field label, row captions, placeholder text.

### Named Rules
**The One Accent Rule.** As a *standing* mark of ownership, the accent color lives in exactly one place: Our Comm's own label and value text, the owner's own earnings. Everywhere else — including Received Amount and Comm Main, both larger dollar figures — stays black, white, or gray. The one exception is *momentary*: any copy button's brief post-click confirmation also turns Sage, because that's feedback about an action just taken, not a standing claim about a number's importance — it fades in about a second, on whichever button was pressed, not just Our Comm's.

## Typography

**Body/Display Font:** system font stack (`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`) — deliberately not a custom webfont, so the service worker never needs a network font fetch to render offline. Revisit with a self-hosted display face via a future typeset pass once the visual world is confirmed beyond this first placeholder build.

**Character:** clear and even-weighted, no soft edges — every size step is bold-to-black weight, uppercase tracking on labels, tabular numerals on every value.

### Hierarchy
- **Label** (800, 13px, 1.5px tracking, uppercase): field name and row captions.
- **Numeric** (800–900, 24–28px, tabular-nums): the price input and every computed value, including the total.

### Named Rules
**The No Regular Weight Rule.** Nothing on this screen is drawn below font-weight 700 — thin or regular type stays out of this system, keeping labels legible without any of them needing to be loud about it.

## Layout

Single centered console, no page grid: a 380px-max-width bordered card sits near the top of an otherwise empty black viewport. Vertical rhythm is deliberately coarse — 24px card padding, 14px gap between breakdown rows, a 2px white rule before the breakdown. Density stays low so the ledger reads clearly.

Information is arranged to make the calculation legible, not just correct. Three amount inputs — USD, AED, Received Amount — are stacked together as one fully bidirectional group (editing any one back-solves the other two), sitting above two adjustable rate inputs (Service Rate %, Our Rate %) side by side, captioned once ("Both rates below apply to the AED Amount above") so the base every percentage applies to is never ambiguous. Below the divider, Service Comm and Our Comm are drawn as an explicit addition ledger — a literal "+" between the two rows, a rule, then Comm Main labeled with its own combined percentage ("X% of AED") as the sum. Nothing about the relationship is left for the reader to infer from position alone.

### Named Rules
**The Shown-Sum Rule.** A value that is the sum of two others is never just placed near them — the addition itself is drawn: the parts, a "+", a rule, then the total. A reader should be able to verify the arithmetic from the page alone, without doing it in their head.

## Elevation & Depth

No shadows anywhere, hard or soft. Depth is not simulated — the card is flat black on flat black, defined only by its border. Function over form: nothing is spent on a depth cue that doesn't help the number get read.

### Named Rules
**The No Decoration Rule.** A border defines a boundary; it does not also need a shadow to prove it. If a visual device isn't load-bearing for reading the numbers, it doesn't ship.

## Shapes

Zero border-radius everywhere — the card and every input are clean rectangles. Solid 2–3px white borders stand in for the softness a shadow would otherwise supply; the ledger's own rules (the "+" divider and the pre-sum rule) are the only other lines on the page.

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
- **Rate variant** (Service Rate %, Our Rate %): the same field at a smaller scale (18px value, `%` prefix) — two sit side by side in one row since they're peers, not a sequence. A shared caption above the pair states what base they apply to, once, rather than repeating it on each label.
- **Comma formatting is native to every field here**, not a separate feature: it live-reformats while typing (cursor preserved) and settles to a clean 2-decimal value on blur.

### Copy Button
- **Style:** 2px solid Signal White border, Void Black fill, zero radius — same border language as inputs, no separate visual system invented for it
- **Icon:** a drawn copy glyph (two overlapping squares, one consistent stroke, corners squared to 0px to match the rest of the system) — never an emoji or a font icon
- **Placement:** stretched to match its input's full height, sitting immediately to its right; inside the ledger it shrinks to a compact 26px square next to Our Comm's value
- **Confirmation state:** on copy, the icon swaps to a checkmark and the border/icon turn Sage for about a second, then revert — the only place besides Our Comm's own text that the accent appears, and only for a moment, as a state change rather than a standing mark
- **What it copies:** the value exactly as displayed, prefix included (e.g. "AED 3,636.51"), so there's never a mismatch between what's on screen and what lands on the clipboard
- **Empty state:** the three amount-field buttons (USD, AED, Received) dim to 35% opacity and drop their hover response when their field is blank — nothing to copy, shown rather than a silent no-op tap. Our Comm's button has no empty state: it always shows a real amount, even AED 0.00.

### Export Button
- **Style:** the same border and confirmation language as the Copy Button, but full-width and carrying a visible uppercase label instead of relying on icon recognition alone — it's the one action on the page meant to be found by reading, not by memorizing a glyph
- **What it copies:** every field as one organized block of text, in the card's own reading order (USD/AED principal, rates, the commission ledger, then Received Amount as the bottom line) — see PRODUCT.md for the exact format
- **Confirmation:** on success its label swaps to "Copied" (not just the icon) for about a second, alongside the same Sage border/icon flash as every other copy button
- **Disabled state:** dims whenever there's no AED principal entered — nothing meaningful to export yet

### Commission Ledger
- **Style:** a plain row (Service Comm), a centered gray "+" glyph, a second row in Sage (Our Comm), a 2px white rule, then a larger bold row (Comm Main) — the whole group reads top to bottom as one addition
- **Why this shape:** the Shown-Sum Rule — the reader should never have to trust that Comm Main equals the two rows above it; the page proves it
- **Our Comm's accent:** only its label and value text are drawn in Sage — no fill, no border, no separate block. It stays inside the ledger's natural addition order instead of being pulled out, so the sum stays visibly whole while the color still marks it as the owner's own number.

## Do's and Don'ts

### Do:
- **Do** keep the accent confined to Our Comm's own text — the One Accent Rule.
- **Do** keep every border and rule hard-edged (0px radius) and at least 2px thick.
- **Do** keep text weight at 700+ everywhere; no thin or regular type.
- **Do** draw every sum as a literal addition — parts, "+", rule, total — the Shown-Sum Rule.
- **Do** name what a percentage applies to wherever it's shown or entered — a bare "%" is never enough on its own.

### Don't:
- **Don't** add any shadow, soft or hard — the border alone defines the card; depth cues are decoration this system doesn't spend on.
- **Don't** load an external webfont; the app must render and cache fully offline.
- **Don't** give Comm Main its own independently-editable rate — it must always equal Service Comm + Our Comm exactly, per PRODUCT.md's calculation model.
- **Don't** reach for an urgent or alarm-style color for the accent — the whole point of this palette is that the one signal color reads as calm, not urgent.
- **Don't** pull a value out of its natural sum for emphasis — mark it with color in place instead, so the sum stays whole and visible (see the Commission Ledger).
