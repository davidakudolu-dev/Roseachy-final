# Credit Account editability and integrity — 2.14.0

## Implemented behavior

The existing current-day Sales Records editor now accepts Customer Credit Account sale lines. Each line retains its ID, purchase-group ID and original timestamp. Quantity and product corrections go through the existing stock replay/delta machinery: changing five units to three restores two; changing five to eight deducts three more. Substituting a product reverses the prior product movement. A guarded Remove action removes a product line and retains it with correction history in the existing recycle bin.

Totals use the existing product/snapshot pricing rules; this update does not introduce arbitrary price overrides. The editor retains the existing applicable note/person field. All upfront cash/bank allocations are locked while editing. Collections remain separate, byte-preserved rows and are never posted again. Credit balances derive from the corrected obligations minus existing collections.

Changing customer moves all current-day lines in the same purchase group. It is blocked if the source account has any collections or the purchase has upfront payments, because the existing data model cannot safely transfer their provenance. The destination customer's credit limit is enforced. A reduction below a line's upfront payment, below potentially applicable collections, or below a nonnegative customer balance is blocked. No implicit refund, payment deletion, negative receivable or reassignment of money occurs.

The top-left menu entry and its exclusive creation/click code are removed. The entry-form Manage Credit Accounts shortcut and Sales Records Account shortcuts are also removed to satisfy the single-access requirement. The existing right-side button, icon, positioning, menu and opening handler remain unchanged. Credit collections are visible in records but are not editable money transfers; this update edits credit sales, preserving collection records.

## Atomicity, history and security

Corrections use inventoryCommit / RS.atomic. Live transactions, inventory, any matching saved current-day copy, correction history and sync events commit together. Persistent storage aborts restore the prior state. Existing busy guards prevent duplicate submissions. A captured transaction fingerprint rejects stale or restored edit sessions; reopen Edit after such a rejection.

Correction history stores a change ID, time and before/after line snapshots, including customer identity, product, quantity, total and allocations. Local notes remain in local history/full backups; signed packages omit private note/person text under the existing privacy policy. Record details display correction quantities and amounts. Removed rows retain their history in the recycle bin. Business payment records are preserved.

The Financial Desk PIN gate and owner-confirmation code are unchanged. A credit edit cannot modify the upfront bank amount or payment method, so it does not manufacture another bank posting or bypass settlement approval. Historical saved days remain read only under the existing authorization rules.

## Sync repair discovered during simulation

A saved day's opening-stock batch layout is reconstructed when accepting a checkpoint. Comparing that derived batch layout against an older saved snapshot caused a false day-header conflict on a valid subsequent correction. dayComparable now excludes those derived opening batches, just as it already excluded derived closing stock. It continues to compare opening quantities, prices, costs and other business metadata. No transaction identity, amount, signature or conflict-resolution check was removed.

Two independent browser contexts verified signed checkpoint setup, edited transaction propagation, three repeated imports without duplicated stock/payments, preserved correction history, stale-editor rejection and explicit conflict detection for simultaneous edits to the same sale. Real devices were not used.

## Required simulation matrix

| Scenario | Executable coverage |
|---|---|
| Right-side account access / duplicate Settings entry absent | Actual right-side clicks and DOM assertions; other management shortcuts absent |
| Sales Records Edit | Existing data-edit control and Save button |
| Reduce/increase quantity | Stock and obligation arithmetic; 25,000 sale/10,000 collection reduced to 20,000 leaves 10,000 |
| Substitute/remove product | Original movement reversed, new movement applied; guarded removal preserves retained lines |
| Partial/fully paid sale | Collections unchanged; increased credit accepted and excess-payment reduction blocked |
| Customer change | Entire unpaid purchase moved; paid reassignment blocked |
| Repeated Save | No second sale, stock movement or collection |
| Insufficient stock / invalid input / credit limits | Full state unchanged after rejection |
| Saved copy and restart | Matching current-day snapshot updates; reload equality |
| Backup restore | State and history preserved; right-side access opens |
| Storage failure | Persistent IndexedDB abort rolls back stock, balances, transactions and saved copy; reload verified |
| Pack-enabled stock | Existing loose-unit editor changes 60 to 96 out of 120 units, leaving 24 |
| Signed sync / repeated import / concurrency | Independent contexts and explicit conflict assertions |
| Financial security | Upfront bank payment unchanged and private Financial Desk remains locked |

Final counts and per-suite outcomes are in VERIFICATION.md and audit/final. The first full run had one fixture failure: it attempted hidden pack-entry controls that the current app deliberately disables. The corrected test uses the actual loose-unit editor. No hidden control was enabled and no pack-entry interface was added.

## Scope and limitations

- Customer collections have no purchase-specific allocation. The reduction guard conservatively reserves potentially applicable collections, so some reductions may be blocked even when other debts exist. An explicit allocation/refund feature would be a separate data-model change; none was invented here.
- Historical days and collection records retain their existing read-only/edit protections. Credit-sale line editing is for the current working day, including its matching saved copy.
- Products are edited per existing Sales Records line. This update does not introduce a new basket editor or permit adding a new line into an existing purchase group.
- All peers should run this version before exchanging correction-history packages. Older versions may reject the newly classified history field rather than silently discard audit information.
- Physical devices, native file handling and live Cloudflare deployment remain unverified. Automated browser simulation is not exhaustive platform certification.

## Change boundaries and delivery

Runtime changes are confined to index.html: CreditAccounts, sanitized transaction projection, existing sales edit/remove handlers, ledger actions, correction details and day-header comparison. The source-boundary test checks against the preserved 2.13.1 runtime. The stylesheet and twelve non-HTML runtime assets remain unchanged. No new production dependency, backend, database or interface redesign was added.

Deliverables: complete source with tests/evidence, thirteen-file Cloudflare Pages ZIP, standalone HTML/preview ZIP and SHA-256 manifest. The archives are validated against their source bytes before publication. No Cloudflare deployment is performed by packaging.
