# Roseachy Desk — Reference Theme Reconstruction

Release: **2.13.0-reference-themes**.

## Source and preservation

This release starts from the exact supplied `Roseachy-Desk-Cloudflare (1).zip`, not the previous visual redesign. Its SHA-256 is `cf027a236bc1f1debba6f9628ccb94954afde73d3dadc0003cb5f83d0c2624dd`; the original is retained in `audit/original-input.zip`.

**Cobalt Studio, Midnight & Ember and Pearl & Royal Blue** retain their original palette definitions and styling. The reference styling only matches the seven new theme IDs. Switching back to an original theme removes all reference tokens.

Only runtime `index.html` changes. All 12 other runtime files, including the service worker, manifest, security headers, redirects, icons and external scripts, are byte-identical to the input. Business, storage, PIN, backup, sync and navigation code is unchanged outside the explicitly tested appearance registry and application changes. The DOM is unchanged outside the theme picker and its description. Original CSS is retained verbatim; the new screen-only block changes colors, fonts and surface treatment without changing layout, spacing, border radii, animation or gesture handling.

## Reference map

| Image | Theme | Visual treatment | Previous saved theme |
| --- | --- | --- | --- |
| 1 | Cyan Flux | Graphite surfaces, cyan/blue/violet gradients, sculpted shadows and cool glow | Deep Blue |
| 2 | Lunar Glass | Indigo ambient shell, pale lavender/cyan glass cards and luminous controls | Midnight Gold |
| 3 | Coral Carbon | Matte charcoal, coral/salmon controls and balance-card highlight | New slot |
| 4 | Mono Mint | Pale neutral surfaces, black balance card and controls, mint accents | Green |
| 5 | Acid Ledger | Slate shell, lime controls, white transaction tables with dark text | New slot |
| 6 | Teal & Apricot | Paper/cream surfaces, teal highlights and apricot controls | Ocean |
| 7 | Violet Wallet | White/lilac light mode, ink dark mode and luminous violet highlights | Aubergine |

Image 8 repeats image 6 and is represented once. Each theme has coordinated light/dark variants. The reference palettes and materials are applied to Roseachy's existing interface; the unrelated banking, gaming and scheduling layouts are not copied. Font families use local sans-serif fallbacks because the reference images do not identify licensed source fonts.

Compatibility aliases migrate older theme IDs directly to the appropriate new theme. Protected aliases retain their original targets. The original appearance-default object remains unchanged, with its retired Green ID resolved to Mono Mint by the existing validation path.

## Verification

**80 automated checks pass**, plus the static audit. The 36 protected screenshots match exactly in the final comparison. The final recorded results are in `audit/verification-summary.json` and individual `audit/*-results.json` files. Functional suites cover sales, inventory, suppliers, payments, credit limits, refunds, PIN access, backup/restore, failed-save rollback, signed synchronization and offline loading. Theme checks cover protected appearance comparisons, palette migration, persistence, text contrast, responsive bounds and mobile navigation/dialog interactions.

Screenshot comparisons use the same Chromium build with transitions frozen in both versions. The test also requires matching computed chevron styles and geometry. A tightly bounded allowance exists for rasterization differences inside those identical chevrons; the exact measurements and tolerance are recorded with the result. This is separate from the check that production animation and gesture settings are unchanged.

Contrast checks sample actual inherited backgrounds and gradient endpoints across 14 theme/mode combinations, including the locked and unlocked Financial screens. They do not constitute exhaustive accessibility certification. Screenshots use isolated synthetic browser profiles and are not included in the runtime ZIP.

Chromium automation does not certify physical devices, Safari/Firefox rendering, native Android file access, or Cloudflare production headers. No Cloudflare deployment was performed. Existing PWA configuration is deliberately retained.

## Deliverables

- `Roseachy-Desk-2.13.0-Cloudflare-Pages.zip`: 13 runtime files at the ZIP root.
- `Roseachy-Desk-2.13.0-Source.zip`: runtime, tests, documentation, provenance and visual evidence.
- `Roseachy-Desk-2.13.0-Preview.html`: downloadable HTML preview.
- `Roseachy-Desk-2.13.0-Preview.zip`: the same HTML packaged for a direct ZIP download.
- `SHA256-MANIFEST.json`: package and deployment-file hashes.

The packager verifies ZIP CRCs, member lists and content hashes against local source files.
