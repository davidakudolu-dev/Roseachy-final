# Credit receipts and navigation report — 2.15.0

The base is the user's supplied 2.14.3 backup, archived under audit/baseline. Newer supplier-payment functionality from that backup was retained. Theme cleanup and credit-edit protections from the preceding work remain active.

## Accounting and storage

Purchases and payments already use separate authoritative transaction types. `calc` already recognizes full purchase turnover/profit, only purchase split receipts as money received, and later collections as cash/bank inflows without new revenue or inventory. That accounting engine is unchanged.

`dailyRecordRows` derives the operational list: unpaid `sale_account` rows are omitted; actual upfront allocations are grouped into an initial receipt; standalone collections retain their payment ID. The projection is never persisted or used as an accounting input. CSV exports the same collection representation. Full backups and reports retain original purchases.

New rows record the selected business date. Older records use their existing saved-day context without inventing a payment date or purchase allocation. Account-level payments explicitly say that no purchase allocation was recorded. Inconsistent legacy purchase groups are flagged and shown as separate factual receipts; correction is blocked until references are reconciled.

Purchase edits reuse the existing editor and reconciliation code. Payment edits and upfront-allocation corrections use `inventoryCommit` plus `commitSalesLedger`, updating matching saved-day copies within the same atomic write. IDs and collection timestamps remain stable. Before/after snapshots preserve amount, method, customer and product fields. Busy/stale submission guards prevent overwrites or replays. Shared audit snapshots exclude private notes.

Standalone receipt reversals use the existing deletion/recycle-bin model, retaining original records and correction history. Sync carries the original before-state and removal in its signed event history. Upfront reversal zeros receipt allocations and restores the obligation, preserving the purchase and its audit. Changed bank claims use the unchanged owner confirmation queue; corrections never write directly to the protected bank balance.

## Navigation

The attachment and available earlier source contained no original dedicated top-right Sales button. The user explicitly selected reuse of the existing header styling. The new Sales control uses that style and the existing `switchDesk` behavior; the supplied Credit Accounts button markup/icon/position remains intact. Only Sales was removed from the left menu. Market, Financial Desk and Settings remain.

The existing back model treats Sales Home as the terminal internal destination. Tests verify Back from Market returns Home and Back closes a Credit modal first. No navigation architecture was replaced.

## Explicit limits and unresolved cases

- Historical saved days remain read only, consistent with the existing application's editing rules. Corrections operate on current-day transactions; mismatched saved copies require saving the complete current day first.
- Legacy standalone payments are account-level, without reliable purchase allocation. They are labeled accordingly. Reassignment with payments, unsafe reductions, and paid-purchase cancellation are conservatively blocked rather than guessing allocation or creating an unsupported refund/unapplied-credit balance.
- No independent cash refund or customer-credit-allocation module was introduced. Such resolutions require the business's supported reconciliation workflow before a blocked cancellation can proceed.
- Ambiguous legacy references are preserved and flagged. No historical payment or starting debt is inferred from a purchase total.
- Browser/PWA tests run in Chromium with mobile/tablet/desktop viewports and independent contexts. Physical devices, native Android file pickers and a live Cloudflare deployment are not tested here.

See VERIFICATION.md and audit/final for exact executed results. The corrected source, thirteen-file Pages upload ZIP, HTML/ZIP preview, changelog and SHA-256 manifest are packaged separately.
