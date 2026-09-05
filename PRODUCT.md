# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack
static HTML/CSS/JS (no build step, no framework) — installable as an iOS home-screen PWA via manifest.json + a service worker for offline caching.

## Users
Just the owner, for personal use — a quick private tool to check a commission split after/before a deal, opened from the iPhone home screen.

## Product Purpose
Instantly computes a commission breakdown from a single product price input: platform fee deducted, then a two-tier commission (direct + override) split off the adjusted base. Success means opening the home-screen icon and getting a correct, instant, offline breakdown with no network dependency.

## Operating Context
Used on an iPhone, added to the home screen, expected to work with no internet connection (airplane mode / cellular dead zones). All three files (index.html, sw.js, manifest.json) must be served together over HTTPS for the service worker and install prompt to function; GitHub Pages is the current deploy target.

## Capabilities and Constraints
- Single input: product price (USD only, no other currency needed yet).
- Fixed two-tier commission structure: Platform Fee, Adjusted Base, Tier 1 Direct, Tier 2 Override, Net Commission.
- **Current rates (10% platform fee, 15% Tier 1, 3% Tier 2) are explicitly placeholders**, not confirmed business rules — expect them to change before this is treated as final. Don't treat these numbers as durable product facts to preserve.
- No history, no multiple line items, no additional tiers — none requested yet.
- Must work fully offline once installed (service worker pre-caches all three files).

## Evidence on Hand
None — no real rate sheet, customer, or business data provided yet. Do not fabricate commission-structure specifics beyond the placeholder rates already in code.

## Product Principles
- Offline-first: the app must never depend on a live network connection once installed.
- Single-purpose utility: one input, one clear breakdown — resist adding scope beyond what's asked.
- Placeholder honesty: keep the current rates clearly swappable; don't harden them into the design as if final.

## Accessibility & Inclusion
None established yet.
