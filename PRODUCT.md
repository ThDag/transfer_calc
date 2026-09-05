# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack
static HTML/CSS/JS (no build step, no framework) — installable as an iOS home-screen PWA via manifest.json + a service worker for offline caching.

## Users
Just the owner, for personal use — a quick private tool to check a commission split after/before a deal, opened from the iPhone home screen.

## Product Purpose
Computes a money-transfer (havale) commission breakdown: a USD amount is converted to AED at a fixed peg, a commission is taken from the AED amount, and that commission is split between a service party and the owner. Success means opening the home-screen icon, entering an amount from any of several linked fields, and getting a correct, instant, offline breakdown — the recipient's net amount and the owner's own earned cut (OurComm) above all.

## Operating Context
Used on an iPhone, added to the home screen, expected to work with no internet connection (airplane mode / cellular dead zones). All three files (index.html, sw.js, manifest.json) must be served together over HTTPS for the service worker and install prompt to function; GitHub Pages is the current deploy target.

**App name: HavCal.** Set in three places that all must agree: `manifest.json` (`name` and `short_name`), the `apple-mobile-web-app-title` meta tag, and `<title>` — iOS reads the home-screen icon label from the meta tag (not just the manifest), so all three need to change together on any future rename.

**No page scroll/bounce by design.** The card is meant to fit the viewport exactly; `html, body` are locked to `height: 100%; overflow: hidden; overscroll-behavior: none;` specifically to kill iOS's rubber-band bounce on the standalone home-screen app (the classic cause of an apparently-scrollable page even when content fits — `min-height: 100vh` plus native elastic overscroll, not real content overflow). Padding uses `env(safe-area-inset-*)` (`viewport-fit=cover` in the viewport meta) instead of a hardcoded guess, so it adapts correctly across notch, Dynamic Island, and non-notch iPhones. Trade-off: if the card ever grows taller than the smallest supported screen (iPhone SE-class), content will clip at the bottom instead of scrolling — verify on a real small device before adding content, since this can't be checked without a browser here.

## Capabilities and Constraints
- Three fully bidirectional amount inputs, not a single one-way input: **USD**, **AED**, and **Received Amount** can each be edited directly, and editing any one back-solves the other two (plus Service Comm % and Our Comm % are independently editable rate inputs). Editing any of the five recomputes every derived figure.
- **Fixed exchange rate: 1 USD = 3.6725 AED** — the official, decades-stable USD/AED peg. Hardcoded, no network call, confirmed with the user.
- **Calculation model** (confirmed with the user):
  - `AED = USD × 3.6725` (bidirectional: editing AED back-solves USD, and vice versa)
  - `ServiceComm = AED × ServiceCommPct / 100` (ServiceCommPct defaults to 0.68, meaning 0.68%)
  - `OurComm = AED × OurCommPct / 100` (OurCommPct defaults to 0.30, meaning 0.30%)
  - `CommMain = ServiceComm + OurComm` — **not** an independently-set rate; it is always exactly the sum of the two splits, so the splits can never drift out of sync with the total commission.
  - `RecievedAmount = AED − CommMain` — the amount the recipient actually gets. **Also directly editable**: editing it back-solves `AED = RecievedAmount / (1 − CommMainPct/100)` and then `USD = AED / 3.6725`, holding the current Service/Our percentages fixed — i.e. "I want the recipient to get exactly X, how much do I need to send?"
  - Editing Service Comm % or Our Comm % holds the current AED principal fixed and only re-splits the commission, which moves Received Amount downstream.
- **Thousands-separator formatting on every editable field** (USD, AED, Received Amount, Service Comm %, Our Comm %): live-formatted while typing (cursor-position preserved, not rounded mid-entry) and reformatted to a clean 2-decimal value on blur. Plain `<input type="number">` can't display commas, so these are `type="text" inputmode="decimal"` with custom parsing.
- **Copy buttons on USD, AED, Received Amount, and Our Comm** (not on Service Comm or Comm Main — those weren't asked for): each copies the value exactly as displayed, including its currency prefix (e.g. "AED 3,636.51"), so what lands on the clipboard matches what's on screen. Uses the Clipboard API with an `execCommand` fallback for older WebView contexts; gives a brief visual confirmation (icon swaps to a checkmark, border turns to the accent color) for ~1.2s. The three amount-field copy buttons disable themselves (dimmed, no hover) whenever their field is empty — nothing to copy yet, shown rather than a silent no-op click; Our Comm's copy button stays always-enabled since it always shows a real formatted amount, even AED 0.00.
- Information hierarchy: AED amount is the transfer principal; Received Amount and Comm Main are siblings derived from it (Received Amount is also a valid alternate entry point that back-solves the principal). Service Comm and Our Comm are shown as an explicit addition ledger (`Service Comm + Our Comm = Comm Main`, with a literal "+" between the two rows and a rule before the sum) rather than implying the sum through nesting — the split relationship is now shown, not just structurally implied.
- **Both rate fields are labeled and captioned to make clear they apply to the AED Amount** (not USD, not Comm Main): "Service Rate %" / "Our Rate %", with a shared caption above them ("Both rates below apply to the AED Amount above"), and the Comm Main row spells out "(X% of AED)" rather than a bare percentage.
- **OurComm is the headline number** — it is the owner's own earnings from the transfer, matching this tool's original purpose (checking what you personally earn), so it keeps the one visual accent in the design system (its row's text is drawn in the accent color within the ledger, no longer pulled into a separate block). RecievedAmount and ServiceComm are shown but not specially emphasized.
- No history, no multiple line items — none requested yet.
- Must work fully offline once installed (service worker pre-caches all three files; cache bumped to v3 when Received Amount became editable).

## Evidence on Hand
The USD/AED peg rate (3.6725) is a real, publicly documented, decades-stable central-bank peg — safe to treat as durable. The default split percentages (0.68% service, 0.30% ours) are the user's own real business defaults, confirmed directly, not placeholders. No other business data (customer names, volumes, historical transactions) has been provided — do not fabricate any.

## Product Principles
- Offline-first: the app must never depend on a live network connection once installed.
- Reactive by design: any input the user edits should immediately and correctly recompute every other derived figure — never leave stale numbers on screen.
- Internal consistency over flexibility: CommMain is derived, never a separate editable value, specifically so the commission split can never silently disagree with the total taken.

## Accessibility & Inclusion
None established yet.
