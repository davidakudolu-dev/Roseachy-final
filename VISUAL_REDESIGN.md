# Roseachy Desk 2.12.3 — visual finish

The supplied Sales Desk preview guides the appearance: warm ivory surfaces,
restrained gold highlights, serif headings, sans-serif form text, pill-shaped
actions and navigation, rounded fields, and soft card depth. The existing
Roseachy application remains the structural and functional foundation.

## Scope

One named, screen-only CSS block (`roseachy-reference-finish` in `index.html`)
applies the finish to existing Sales, Market, Financial, Settings, Credit and
Sync components. Theme palettes and their foreground pairs remain authoritative;
light surfaces receive an ivory tint and dark surfaces a subtle gold highlight.
All eight themes remain available, including Midnight Gold and Midnight & Ember.
Market's active tab and Settings preview labels now use foreground colors matched
to their actual surfaces. No fonts, frameworks or runtime dependencies were added.

Original styles, responsive breakpoints, element order, navigation placement,
animation definitions, event handlers and every production script are unchanged.
Appearance sizing, shape and elevation controls continue to supply the CSS values.
The app version and service-worker cache identifier advance to 2.12.3 so installed
clients can obtain the new appearance through the existing update workflow.
No business logic, database schema, storage keys, sync protocol, PIN rules or
existing user preferences were changed. No data migration is required.

## Verification

- All 67 existing functional/platform checks passed, including stock, sales,
  supplier payments, credit, signed sync, backups, PIN access and offline reload.
- Eight visual checks cover unchanged script/DOM hashes, a paint-only CSS property
  allowlist, 48 theme/mode/viewport combinations and 192 desk checks with no page
  overflow at 390, 768 and 1280 pixels.
- Sampled field, label, summary, modal, Market-tab and Settings-preview text pairs
  meet 4.5:1 contrast in every theme and mode. These are computed base-color samples,
  not an exhaustive accessibility certification of every transient state.
- Theme and dark-mode changes persist after reload. Mobile tab clicks, synthetic
  horizontal touch swipes and credit-modal actions pass. Computed animation,
  transition, touch-action, position and overflow properties match with the new
  stylesheet enabled and disabled.
- Reference and finished screens were inspected in Chromium. Four screenshots,
  the presentation patch and machine-readable results are in `audit/visual/`.

The initial visual checker treated CSSOM-expanded border properties as unexpected;
its property allowlist was corrected and the visual suite rerun. No application
change was needed for that test-only issue.

## Deliverables and limits

`dist/Roseachy-Desk-2.12.3-Cloudflare-Pages.zip` contains the 13 runtime assets at
the ZIP root. `dist/Roseachy-Desk-2.12.3-Source.zip` includes the complete project,
tests, reports and evidence. Packaging checks ZIP CRCs, file lists and content
hashes; `dist/SHA256-MANIFEST.json` records the resulting checksums.

No unresolved issue was observed in the exercised flows. Physical iOS/Android
hardware, Safari and Firefox were not tested. Fonts use local fallbacks when the
reference fonts are unavailable. Cloudflare deployment has not been performed.
Earlier audit and migration reports describe the preceding 2.12.2 integrity
release and remain included as historical evidence.

To undo the visual finish, remove only the named CSS block and issue a new
service-worker cache version. Application scripts and persisted records need no
rollback.
