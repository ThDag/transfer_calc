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

**No page scroll/bounce by design.** The content is meant to fit the viewport exactly; `html, body` are locked to `height: 100%; overflow: hidden; overscroll-behavior: none;` specifically to kill iOS's rubber-band bounce on the standalone home-screen app (the classic cause of an apparently-scrollable page even when content fits — `min-height: 100vh` plus native elastic overscroll, not real content overflow). Padding uses `env(safe-area-inset-*)` (`viewport-fit=cover` in the viewport meta) instead of a hardcoded guess, so it adapts correctly across notch, Dynamic Island, and non-notch iPhones. Trade-off: if the content ever grows taller than the smallest supported screen (iPhone SE-class), it will clip at the bottom instead of scrolling — verify on a real small device before adding content, since this can't be checked without a browser here.

**No outer card.** There is no bordered/backgrounded container wrapping the whole app — the black page itself is the canvas, and `.content` is a plain layout wrapper (`width`/`max-width` only, no border, no padding of its own). Structure comes entirely from each element's own border (the Twin Pair Fields, rate inputs, the export button), not from one containing frame. Removing the card also removed a redundant layer of padding — content now sits closer to the screen edges, spaced only by `body`'s own padding.

## Capabilities and Constraints
- **Two twin pairs, not four independent fields.** "Amount" and "Received Amount" are each a single conceptual value shown as USD and AED siblings inside one shared bordered box — **stacked top over bottom, split by one internal horizontal rule** (not side by side), so each currency face gets the box's full width rather than splitting it. Visually and semantically they're one value with two faces, not two coincidentally-related numbers. All four fields (Amount-USD, Amount-AED, Received-USD, Received-AED) plus Service Comm % and Our Comm % are fully bidirectional: editing any one recomputes every other derived figure. Editing an Amount field sets the transfer principal; editing a Received field back-solves the principal needed to net that exact amount (converting from USD to AED first via the exchange rate when the USD side is edited).
- **Exchange rate is adjustable, not hardcoded** — defaults to 3.6725 (the official USD/AED peg) but is a small, deliberately quiet inline field ("1 USD = [3.6725] AED", no full label, thin gray border) placed right after the Amount pair, so it's editable without competing visually with the primary inputs. Still no network call — this is a manual override for the rare case the operator needs a different rate, not a live lookup. Editing it holds whichever amount is actually populated fixed (USD if present, else AED) and re-derives the other side plus the whole breakdown. Formatted to 4 decimal places on blur (2 isn't enough precision for a rate like 3.6725) rather than the 2-decimal formatting every money field uses. **A small reset icon next to it** restores the default 3.6725 in one tap and re-derives everything at that rate, sharing the exact same recompute logic as typing the value in manually.
- **Calculation model** (confirmed with the user):
  - `AED = USD × FxRate` (FxRate defaults to 3.6725, user-adjustable; bidirectional: editing AED back-solves USD, and vice versa)
  - `ServiceComm = AED × ServiceCommPct / 100` (ServiceCommPct defaults to 0.68, meaning 0.68%)
  - `OurComm = AED × OurCommPct / 100` (OurCommPct defaults to 0.30, meaning 0.30%)
  - `CommMain = ServiceComm + OurComm` — **not** an independently-set rate; it is always exactly the sum of the two splits, so the splits can never drift out of sync with the total commission.
  - `RecievedAmountAED = AED − CommMain`, `RecievedAmountUSD = RecievedAmountAED / FxRate` — the amount the recipient actually gets, shown in both currencies as a twin pair. **Either side is directly editable**: editing RecievedAmountAED back-solves `AED = RecievedAmountAED / (1 − CommMainPct/100)`; editing RecievedAmountUSD first converts (`RecievedAmountAED = RecievedAmountUSD × FxRate`) then applies the same back-solve — either way `USD = AED / FxRate`, holding the current Service/Our percentages fixed. i.e. "I want the recipient to get exactly X (in either currency), how much do I need to send?"
  - Editing Service Comm % or Our Comm % holds the current AED principal fixed and only re-splits the commission, which moves Received Amount downstream.
- **Thousands-separator formatting on every editable field** (both twin pairs, Service Comm %, Our Comm %): live-formatted while typing (cursor-position preserved, not rounded mid-entry) and reformatted to a clean 2-decimal value on blur. Plain `<input type="number">` can't display commas, so these are `type="text" inputmode="decimal"` with custom parsing.
- **One copy button per currency face** (not one per pair): each of the four twin-pair fields (Amount-USD, Amount-AED, Received-USD, Received-AED) has its own small borderless copy icon inline inside that segment, copying just its own value with its own currency prefix (e.g. "AED 3,672.50") — reverted from an earlier single-button-per-pair consolidation, which undersold that the two faces are independently useful to copy on their own. Plus copy buttons on Our Comm and the main Export Summary button (not on Service Comm or Comm Main — those weren't asked for). Uses the Clipboard API with an `execCommand` fallback for older WebView contexts; gives a brief visual confirmation (icon swaps to a checkmark, color turns to the accent) for ~1.2s. Each field's own copy button disables itself (dimmed) whenever that field is empty; the export button gates off the AED principal; Our Comm's copy button stays always-enabled since it always shows a real formatted amount, even AED 0.00.
- **A main "Export Summary" button** copies every field as one organized block of text, in the same reading order the page itself uses (USD/AED principal, rate, rates, commission ledger, then Received Amount in both currencies as the bottom line):
  ```
  Amount (USD): $1,000.00
  Amount (AED): AED 3,672.50
  Rate: 1 USD = 3.6725 AED

  Service Rate: 0.68%
  Our Rate: 0.30%
  Service Comm: AED 24.97
  Our Comm: AED 11.02
  Comm Main (0.98% of AED): AED 35.99

  Received Amount (USD): $989.95
  Received Amount (AED): AED 3,636.51
  ```
  Disabled (dimmed) whenever there's no AED principal entered yet. On success its visible label swaps to "Copied" (not just an icon swap, since it's the one button meant to be found by its text at a glance) for ~1.2s.
- **Page order follows the calculation's own dependency chain**: Amount (the principal you set) → exchange rate → Service/Our rates → the commission ledger (Service Comm + Our Comm = Comm Main) → Received Amount (the actual bottom-line result, computed last: `AED − CommMain`) → Export Summary. Received Amount sits at the bottom because it's mathematically the last thing derived, not because it's least important — it's still a valid alternate entry point (back-solving the principal from either its USD or AED face) despite being downstream in the reading order.
- Service Comm and Our Comm are shown as an explicit addition ledger (`Service Comm + Our Comm = Comm Main`, with a literal "+" between the two rows and a rule before the sum) rather than implying the sum through nesting — the split relationship is shown, not just structurally implied. **Comm Main is styled at the same size and weight as Service Comm and Our Comm** — it's the last row in the addition, not a bigger number; hierarchy between rows comes only from the One Accent Rule (Our Comm's color) and position (Comm Main follows the rule as the sum), never from an enlarged font.
- **Both rate fields are labeled and captioned to make clear they apply to the AED Amount** (not USD, not Comm Main): "Service Rate %" / "Our Rate %", with a shared caption above them ("Both rates below apply to the AED Amount above"), and the Comm Main row spells out "(X% of AED)" rather than a bare percentage.
- **OurComm is the headline number** — it is the owner's own earnings from the transfer, matching this tool's original purpose (checking what you personally earn), so it keeps the one visual accent in the design system (its row's text is drawn in the accent color within the ledger, no longer pulled into a separate block). RecievedAmount and ServiceComm are shown but not specially emphasized.
- No history, no multiple line items — none requested yet.
- Must work fully offline once installed (service worker pre-caches all three files). The cache name in `sw.js` gets bumped on every meaningful change to `index.html` so an already-installed phone picks up the update on next launch — currently `havcal-v14`.

## Evidence on Hand
The USD/AED peg rate (3.6725) is a real, publicly documented, decades-stable central-bank peg — safe to treat as the durable default, though the field is user-adjustable for edge cases. The default split percentages (0.68% service, 0.30% ours) are the user's own real business defaults, confirmed directly, not placeholders. No other business data (customer names, volumes, historical transactions) has been provided — do not fabricate any.

## Product Principles
- Offline-first: the app must never depend on a live network connection once installed.
- Reactive by design: any input the user edits should immediately and correctly recompute every other derived figure — never leave stale numbers on screen.
- Internal consistency over flexibility: CommMain is derived, never a separate editable value, specifically so the commission split can never silently disagree with the total taken.
- Twin values share one container: USD and AED are the same underlying amount in two currencies, not two separate facts — Amount and Received Amount are each drawn as one bordered unit (stacked, split by an internal rule), never as two independent-looking fields that happen to relate.
- No container without a reason: a bordered frame exists only where it does real work (grouping a twin pair, bounding an input, marking a button) — never as a default wrapper around content that doesn't need one. The page itself, not a card, is the outermost frame.

## Accessibility & Inclusion
None established yet.
