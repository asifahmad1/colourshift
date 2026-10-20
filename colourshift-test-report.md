# ColourShift v1.0 — Test Report
**Date:** 20 October 2026 · **File:** `colourshift.html` (23,341 bytes / 22.8 KB — limit 150 KB)

## Automated tests (run against the shipped file's conversion code)
40/40 assertions passed (Node.js, functions extracted verbatim from the delivered HTML):

**Spec cases — all pass:**
- `#FF0000` → RGB 255, 0, 0 → HSL 0°, 100%, 50%
- `#00FF00` → RGB 0, 255, 0 → HSL 120°, 100%, 50%
- `#0000FF` → RGB 0, 0, 255 → HSL 240°, 100%, 50%
- `#FFFFFF` → RGB 255, 255, 255 → HSL 0°, 0%, 100% (achromatic: hue 0, sat 0)
- `#000000` → RGB 0, 0, 0 → HSL 0°, 0%, 0% (achromatic: hue 0, sat 0)
- `#808080` → RGB 128, 128, 128 → HSL ≈ 0°, 0%, 50.2%
- `#ABC` expands to `#AABBCC`
- Hue 360 produces the identical colour as hue 0
- Invalid inputs (`#GGG`, `#12345`, empty, `banana`) return null — no crashes

**Paste parser:** `#3498DB`, `rgb(52, 152, 219)`, `hsl(204, 70%, 53%)` all recognised.

**Round trips:** `#FF0000 #00FF00 #0000FF #FFFFFF #000000 #808080 #3498DB #E67E22 #1A2B3C`
all return to the exact original HEX (within integer rounding).

**Formatting:** 2-decimal max with trailing-zero trimming (`50.2%`, not `50.20%`).

## File-constraint checks
- HTML tag balance: no unclosed tags, no mismatches (python html.parser).
- No external URLs, CDN assets, frameworks, fonts, `fetch`/`XHR`, `localStorage`/`sessionStorage`, cookies, or service workers (grep verified — zero matches).
- All conversions run locally; privacy note included in the page.

## Manual / interactive checks (code-reviewed, not browser-executed)
- Sliders ↔ numeric fields stay synced via a single state object; programmatic updates never fire `input` events (no circular loops).
- Invalid input shows an inline error and preserves the last valid colour; nothing is silently replaced with black.
- Copy uses `navigator.clipboard` with a visible manual-copy fallback (selectable textarea + instructions).
- Layout: side-by-side on desktop, stacked on mobile (media query at 860px); no horizontal scrolling.
- Accessibility: labelled controls, `aria-invalid` + `role="alert"` on errors, visible focus indicators, `prefers-reduced-motion` support, keyboard-operable buttons/sliders/inputs.
- Recent colours kept in memory only (cleared on page close).

## Product listing details
- **Name:** ColourShift — HEX, RGB & HSL Converter
- **Category:** Design
- **Version:** 1.0
- **Description:** Instantly convert colours between HEX, RGB and HSL with a live preview, colour picker, sliders and copy-ready CSS values.

## Uploading to the Asif SaaS Studio website (Wix)
1. In the Wix dashboard for the SaaS Studio site, open the Creator Workspace / content manager for the product listing.
2. Create a new product (or tool entry): name **ColourShift — HEX, RGB & HSL Converter**, category **Design**, version **1.0**, and paste the description above.
3. Upload `colourshift.html` as the product's file/download asset (single file, 22.8 KB).
4. If the site embeds the tool in a page: add an **Embed / Custom HTML** element (or HTML iframe component), upload or link the file per Wix's flow, and ensure the iframe allows scripts. The file needs no same-origin access, cookies, or storage, so it works in a sandboxed iframe with scripts allowed.
5. Publish the site through Wix CMS / Site Publish and open the live page to confirm the converter loads and converts.
6. Do not claim deployment is complete until the live page has been opened and a test conversion verified.
