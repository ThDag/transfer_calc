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
- No shadows at all — borders alone define structure; function over form
- Every percentage names what it applies to, and every sum is drawn as a literal addition (a "+" between its parts, a rule, then the total) rather than left implicit
- Twin currency values (Amount, Received Amount) share one bordered box, stacked top over bottom and split by an internal rule, never two separate fields that merely sit near each other
- No outer card — the black page itself is the canvas; borders exist only where an individual element earns one

## Colors

Two neutrals, one accent, spent once.

### Primary
- **Sage** (#87a96b): a calm, muted green reserved entirely for the Our Comm row's label and value text — the owner's own earnings. Never used for chrome, borders, Received Amount, Comm Main, or Service Comm.

### Neutral
- **Void Black** (#000000): page background — the only background on the page; nothing sits on a lighter surface.
- **Signal White** (#ffffff): primary text, all borders and rules, input value text.
- **Concrete Gray** (#999999): secondary text — field label, row captions, placeholder text.

### Named Rules
**The One Accent Rule.** As a *standing* mark of ownership, the accent color lives in exactly one place: Our Comm's own label and value text, the owner's own earnings. Everywhere else — including Received Amount and Comm Main, both larger dollar figures — stays black, white, or gray. The one exception is *momentary*: any copy confirmation also uses Sage (as a border/icon color on a standalone button, or as a background invert on a tappable value), because that's feedback about an action just taken, not a standing claim about a number's importance — it fades in about a second, on whichever element was tapped, not just Our Comm's.

## Typography

**Body/Display Font:** system font stack (`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`) — deliberately not a custom webfont, so the service worker never needs a network font fetch to render offline. Revisit with a self-hosted display face via a future typeset pass once the visual world is confirmed beyond this first placeholder build.

**Character:** clear and even-weighted, no soft edges — every size step is bold-to-black weight, uppercase tracking on labels, tabular numerals on every value.

### Hierarchy
- **Label** (800, 13px, 1.5px tracking, uppercase): field name and row captions.
- **Numeric** (800–900, 24–28px, tabular-nums): the price input and every computed value, including the total.

### Named Rules
**The No Regular Weight Rule.** Nothing on this screen is drawn below font-weight 700 — thin or regular type stays out of this system, keeping labels legible without any of them needing to be loud about it.

## Layout

Single centered column, no page grid, no outer frame: content is capped at 380px max-width and sits near the top of an otherwise empty black viewport, spaced from the screen edges by the page's own padding alone. There is no containing card — structure comes from each element's own border (a Twin Pair Field, a rate input, the export button), never from one frame around everything. Vertical rhythm is deliberately coarse — a consistent ~20px between major groups, 14px gap between breakdown rows, a 2px white rule before the breakdown. Density stays low so the ledger reads clearly.

Information is arranged to make the calculation legible, not just correct, and the page's vertical order **is** the calculation's own dependency order: Amount (the principal the operator sets) → exchange rate → Service/Our rates → the commission ledger → Received Amount, the bottom-line result the ledger produces. Received Amount sits last because it's mathematically last — computed from Comm Main, which is computed from the rates, which apply to Amount — not because it matters least; reading order and computation order are the same thing here.

Amount and Received Amount are each one bordered box holding USD and AED as two sibling faces of the same value, **stacked top over bottom and split by one internal horizontal rule** — not side by side — rather than drawn as separate fields. Editing either face recomputes the other, and the shared border is what tells the reader they're one thing, not two; stacking (rather than splitting the box's width in half) gives each face the full width to stay legible at a size close to a real primary input. The exchange rate connecting them sits quietly right after the Amount box. Below that, two adjustable rate inputs (Service Rate %, Our Rate %) sit side by side, captioned once ("Both rates below apply to the AED Amount above") so the base every percentage applies to is never ambiguous. Below the divider, Service Comm and Our Comm are drawn as an explicit addition ledger — a literal "+" between the two rows, a rule, then Comm Main labeled with its own combined percentage ("X% of AED") as the sum, styled at the same size as its two parts. Nothing about either relationship — twin currencies, or the commission sum — is left for the reader to infer from position alone.

### Named Rules
**The Twin Pair Rule.** Two values that are the same underlying amount expressed differently (USD vs. AED) are drawn inside one shared border, stacked and split by an internal rule — never as two independent-looking fields that merely sit near each other, and never split side by side at the cost of either face's legibility. A shared container is the claim "these are one thing"; proximity alone never makes that claim.

**The Earned Border Rule.** A border is drawn only where it groups or bounds something real — a twin pair, an input, a button. It is never a default wrapper applied to content that doesn't need one; the page itself is the outermost frame, not a card.

**The Shown-Sum Rule.** A value that is the sum of two others is never just placed near them — the addition itself is drawn: the parts, a "+", a rule, then the total. A reader should be able to verify the arithmetic from the page alone, without doing it in their head.

## Elevation & Depth

No shadows anywhere, hard or soft. Depth is not simulated — the page is flat black throughout, and each bordered element is defined only by its own border, never a background shift or a shadow. Function over form: nothing is spent on a depth cue that doesn't help the number get read.

### Named Rules
**The No Decoration Rule.** A border defines a boundary; it does not also need a shadow to prove it. If a visual device isn't load-bearing for reading the numbers, it doesn't ship.

## Shapes

Zero border-radius everywhere — every bordered element (Twin Pair Fields, inputs, the export button) is a clean rectangle. Solid 2px white borders stand in for the softness a shadow would otherwise supply; the ledger's own rules (the "+" divider and the pre-sum rule) are the only other lines on the page.

## Components

### Inputs / Fields
- **Style:** 2px solid Signal White border, Void Black fill, 800-weight white numerals, thousands-comma-grouped as you type
- **Focus:** border and outline switch to Sage
- **Placeholder:** Concrete Gray
- **Rate variant** (Service Rate %, Our Rate %): the same field at a smaller scale (18px value, `%` prefix) — two sit side by side in one row since they're peers, not a sequence. A shared caption above the pair states what base they apply to, once, rather than repeating it on each label.
- **Comma formatting is native to every field here**, not a separate feature: it live-reformats while typing (cursor preserved) and settles to a clean 2-decimal value on blur.

### Twin Pair Field (Amount, Received Amount)
- **Style:** one 2px Signal White border wraps the whole group; a USD segment sits stacked **above** an AED segment, split by a 2px internal horizontal rule, each spanning the box's full width with its own small currency-prefix ("$" / "AED"), 22px numerals — close to a real primary input's weight, since each face now owns the box's full width rather than sharing it side by side — and its own borderless mini copy icon (see Copy Button) tucked inside that segment's own trailing edge
- **One label, not two:** a single uppercase label ("Amount" / "Received Amount") sits above the whole bordered group; neither segment gets its own label, because they're not two fields, they're two faces of one field
- **Focus:** since there's no individual border to recolor, the focused segment gets an inset Sage outline (`outline-offset: -2px`) instead — the system's usual "border turns Sage" rule adapted for a shape with no per-segment border to turn

### Copy Button
- **Style:** 2px solid Signal White border, Void Black fill, zero radius — same border language as inputs, no separate visual system invented for it
- **Icon:** a drawn copy glyph (two overlapping squares, one consistent stroke, corners squared to 0px to match the rest of the system) — never an emoji or a font icon
- **Standalone placement** (Export): stretched to match its row's height, sitting immediately after the value it copies
- **Twin Pair Field variant:** borderless, transparent, gray-until-hover — sits inside each currency segment itself (USD and AED each get their own), not a shared button for the whole pair; copies that one segment's own value with its own prefix (e.g. Amount's AED face copies "AED 3,672.50" alone, independent of its USD sibling)
- **Confirmation state:** on copy, the icon swaps to a checkmark and the border/icon (or just the icon color, for the borderless pair variant) turn Sage for about a second, then revert — the only place besides Our Comm's own text that the accent appears, and only for a moment, as a state change rather than a standing mark
- **What it copies:** the value exactly as displayed, prefix included, always scoped to the one field the button sits beside — never a combined multi-value string
- **Empty state:** every copy button dims to ~30-35% opacity and drops its hover response when its own field is empty

### Ledger Value (tap-to-copy, no button)
- **Style:** Service Comm, Our Comm, and Comm Main have no separate copy button at all — the value itself IS the control, rendered as an unstyled-at-rest `<button>` (no border, no background, `font: inherit`) so it looks exactly like the plain text it replaces until interacted with
- **Confirmation:** on tap, the value's own background and text invert (Sage background, Void Black text) for about a second, then revert — deliberately **not** just a color change, because Our Comm's text is already Sage at rest; an icon swap or color-only flash would be invisible or redundant there. The invert reads clearly regardless of the row's resting color, so one confirmation mechanism covers all three rows.
- **Why not a button next to it:** a value that's directly tappable removes a whole extra element per row; the Twin Pair Field's inline mini-copy buttons exist because those rows already have room and a clear boundary (the segment) to anchor a button to — the ledger's plain rows don't, so the value absorbs the action instead.

### Exchange Rate Field
- **Style:** the one deliberately quiet control on the page — no full uppercase label above it, a thin-feeling inline sentence ("1 USD = [ ] AED") in 12px Concrete Gray instead of a bordered block with its own row. The input itself still keeps the system's 2px-border floor (just in gray, not white) and the same Sage focus treatment as every other field — quiet is achieved through absence of ceremony (no label, tiny width), never by breaking the border-weight invariant.
- **Why it's quiet:** it's an edge-case override (the AED peg almost never actually changes), not a primary input — it shouldn't compete with Amount or Received Amount for attention, but it must still look editable, not like static text.
- **Behavior:** editing it holds whichever of USD/AED is currently populated fixed and re-derives the other side; formatted to 4 decimal places on blur (its own precision, not the 2-decimal rule every money field uses).
- **Reset icon:** a tiny borderless glyph (a single arc with one arrowhead, not the two-arrow copy-glyph language) sits right after it — quieter even than the rate field itself, since resetting is rarer still than adjusting. One tap restores the default 3.6725 and re-derives everything, sharing the exact same recompute path as typing the value in by hand — never a separate, parallel code path that could drift out of sync.

### Export Button
- **Style:** the same border and confirmation language as the Copy Button, but full-width and carrying a visible uppercase label instead of relying on icon recognition alone — it's the one action on the page meant to be found by reading, not by memorizing a glyph
- **What it copies:** every field as one organized block of text, in the page's own reading order (USD/AED principal, rates, the commission ledger, then Received Amount as the bottom line) — see PRODUCT.md for the exact format
- **Confirmation:** on success its label swaps to "Copied" (not just the icon) for about a second, alongside the same Sage border/icon flash as every other copy button
- **Disabled state:** dims whenever there's no AED principal entered — nothing meaningful to export yet

### Commission Ledger
- **Style:** a plain row (Service Comm), a centered gray "+" glyph, a second row in Sage (Our Comm), a 2px white rule, then Comm Main at the **same size and weight as the two rows above it** — the whole group reads top to bottom as one addition, and the sum doesn't get to be visually louder than its own parts just for being the total
- **Why this shape:** the Shown-Sum Rule — the reader should never have to trust that Comm Main equals the two rows above it; the page proves it. Size is not part of that proof: Comm Main is distinguished only by position (last, after the rule), never by being drawn bigger.
- **Our Comm's accent:** only its label and value text are drawn in Sage — no fill, no border, no separate block. It stays inside the ledger's natural addition order instead of being pulled out, so the sum stays visibly whole while the color still marks it as the owner's own number.

## Do's and Don'ts

### Do:
- **Do** keep the accent confined to Our Comm's own text — the One Accent Rule.
- **Do** keep every border and rule hard-edged (0px radius) and at least 2px thick.
- **Do** keep text weight at 700+ everywhere; no thin or regular type.
- **Do** draw every sum as a literal addition — parts, "+", rule, total — the Shown-Sum Rule.
- **Do** name what a percentage applies to wherever it's shown or entered — a bare "%" is never enough on its own.
- **Do** wrap twin values (the same amount in two currencies) in one shared border, stacked top over bottom — the Twin Pair Rule.
- **Do** keep every ledger row (Service Comm, Our Comm, Comm Main) at the same size and weight — a sum earns its position, never a bigger font.
- **Do** order the page to match the calculation's own dependency chain — Received Amount sits last because it's computed last, not because it matters least.
- **Do** draw a border only where it groups or bounds something real — the Earned Border Rule.

### Don't:
- **Don't** add any shadow, soft or hard — borders alone define structure; depth cues are decoration this system doesn't spend on.
- **Don't** load an external webfont; the app must render and cache fully offline.
- **Don't** give Comm Main its own independently-editable rate — it must always equal Service Comm + Our Comm exactly, per PRODUCT.md's calculation model.
- **Don't** reach for an urgent or alarm-style color for the accent — the whole point of this palette is that the one signal color reads as calm, not urgent.
- **Don't** pull a value out of its natural sum for emphasis — mark it with color in place instead, so the sum stays whole and visible (see the Commission Ledger).
- **Don't** draw twin currency values as two separate bordered boxes, or split them side by side — proximity alone doesn't read as "these are one value," and splitting the width in half costs both faces legibility. Stack them, and share one border.
- **Don't** enlarge a sum's font size or weight relative to the values it's summing — Comm Main is proven by the addition it sits in, not by looking heavier than Service Comm or Our Comm.
- **Don't** wrap the whole page in an outer card — the black page is already the frame; a container around everything is a border with nothing left to bound.
