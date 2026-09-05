---
name: Commission Calculator
description: A brutalist, high-contrast black-and-white offline commission calculator with one alarm-red accent.
colors:
  void-black: "#000000"
  signal-white: "#ffffff"
  concrete-gray: "#999999"
  alarm-red: "#ff2400"
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
    backgroundColor: "{colors.alarm-red}"
    textColor: "{colors.void-black}"
    typography: "{typography.numeric}"
    rounded: "{rounded.none}"
    padding: "14px 16px"
---

# Design System: Commission Calculator

## Overview

**Creative North Star: "The Trading Terminal"**

Everything here reads like a stripped instrument panel: black field, hard white rules, stenciled uppercase labels, and exactly one alarm color reserved for the number that matters. There is no ornament, no soft glow, no rounded comfort — the interface is a printed readout, not a friendly app. Weight does the talking: nothing on screen is drawn lighter than 700, so even the smallest label reads as a command, not a caption.

The current palette and rates are an explicit first pass (see PRODUCT.md) — the structure and material language below are the durable part; the exact red hue and rate values may still move.

**Key Characteristics:**
- Void-black field with hard white 2-3px rules standing in for all depth and separation
- Exactly one saturated color (Alarm Red), spent only on the final total
- All type at 700-900 weight; uppercase, tracked labels; tabular numerals throughout
- Zero border-radius everywhere — every shape is a hard rectangle
- A single flat, zero-blur offset shadow as the only depth cue on the page

## Colors

Two neutrals, one accent, spent once.

### Primary
- **Alarm Red** (#ff2400): reserved entirely for the Net Commission total block — the one filled, inverted surface on the page. Never used for chrome, borders, or any other row.

### Neutral
- **Void Black** (#000000): page and card background.
- **Signal White** (#ffffff): primary text, all borders and rules, input value text.
- **Concrete Gray** (#999999): secondary text — field label, row captions, placeholder text.

### Named Rules
**The One Alarm Rule.** Red appears in exactly one place: the final commission total. Everywhere else is black, white, or gray, so when red shows up it means "this is the number that matters."

## Typography

**Body/Display Font:** system font stack (`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`) — deliberately not a custom webfont, so the service worker never needs a network font fetch to render offline. Revisit with a self-hosted display face via a future typeset pass once the visual world is confirmed beyond this first placeholder build.

**Character:** heavy, stenciled, no soft edges — every size step is bold-to-black weight, uppercase tracking on labels, tabular numerals on every value.

### Hierarchy
- **Label** (800, 13px, 1.5px tracking, uppercase): field name and row captions.
- **Numeric** (800–900, 24–28px, tabular-nums): the price input and every computed value, including the total.

### Named Rules
**The No Regular Weight Rule.** Nothing on this screen is drawn below font-weight 700 — thin or regular type has no place in a brutalist instrument panel.

## Layout

Single centered console, no page grid: a 380px-max-width bordered card sits near the top of an otherwise empty black viewport. Vertical rhythm is deliberately coarse — 24px card padding, 14px gap between breakdown rows, a 2px white rule before the total block. Density stays low so the one red block reads immediately.

## Elevation & Depth

No soft shadows anywhere. The only shadow on the page is a flat, zero-blur 8px offset block in Alarm Red behind the card — a stamped/printed depth cue, not an ambient one.

### Shadow Vocabulary
- **Hard offset** (`box-shadow: 8px 8px 0 #ff2400`): the card's only depth cue.

### Named Rules
**The Flat Stamp Rule.** Depth is drawn as a hard offset block, never a blurred glow. Anything ambient is a foreign material in this system.

## Shapes

Zero border-radius everywhere — the card, the input, and the total block are all hard rectangles. Solid 2–3px borders (white on black for the card and input; black on red for the total block) stand in for the softness a shadow would otherwise supply.

## Components

### Cards / Containers
- **Corner Style:** square (0px)
- **Background:** Void Black
- **Border:** 3px solid Signal White
- **Shadow Strategy:** hard offset red block (see Elevation)
- **Internal Padding:** 24px

### Inputs / Fields
- **Style:** 2px solid Signal White border, Void Black fill, 800-weight white numerals
- **Focus:** border and outline switch to Alarm Red
- **Placeholder:** Concrete Gray

### Total Block (Net Commission)
- **Style:** solid Alarm Red fill, 2px Void Black border — the one inverted, filled surface on the page
- **Weight:** 900 / 28px value, 800 / 14px uppercase label, both in Void Black for contrast on red

## Do's and Don'ts

### Do:
- **Do** keep red confined to the Net Commission block — the One Alarm Rule.
- **Do** keep every border and rule hard-edged (0px radius) and at least 2px thick.
- **Do** keep text weight at 700+ everywhere; no thin or regular type.

### Don't:
- **Don't** add soft, blurred, or ambient shadows — only the flat offset block earns a shadow here.
- **Don't** load an external webfont; the app must render and cache fully offline.
- **Don't** treat the current commission rates (10% / 15% / 3%) as final — they're explicit placeholders per PRODUCT.md.
