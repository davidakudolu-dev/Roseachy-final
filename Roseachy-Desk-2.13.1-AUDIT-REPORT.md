# Roseachy Desk 2.13.1 — audit and repair report

## Release status

**Not fully verified.** This is a repaired, tested candidate built from 2.13.0-reference-themes. Physical device input, native Android file handling, all reachable control states and production Cloudflare behavior remain unverified. No known reproducible Critical or High defect identified in the executed cases is intentionally left unresolved. That statement is limited to the tested cases; it is not proof that none exist elsewhere.

The final execution totals and environment are recorded in `audit/final/summary.json`; individual result files provide exact cases and outcomes. This report is supplemented by `VERIFICATION.md` with final counts and timing measurements.

## Baseline and method

The supplied request is preserved in `audit/baseline/request.txt`. The exact previous runtime is `audit/baseline/2.13.0-runtime.zip`, SHA-256 `7172a53b443a55389544e7cc8115f13745e6527da4a9701d0c975318e011ae23`. Older design evidence is explicitly separated under `audit/baseline/prior-release-evidence`.

The initial unchanged build passed 80 existing automated checks. Additional reproductions exposed defects that those tests did not cover. Some earlier layout tests selected nonexistent lowercase `stock`/`tx` tabs; corrected tests select actual `Stock`/`Tx` tabs and assert visible panels. Previous passing layout evidence must therefore not be treated as exhaustive.

Testing uses isolated synthetic profiles, production APIs and actual form controls as documented per suite. Browser contexts emulate independent devices; they are not four physical devices. Storage failures and interrupted saves use test-only injection. Production data was neither read nor modified. The release contains no test hooks or synthetic business records.

## Defect and code change register

| ID / severity | Reproduction and root cause | Repair and verification |
|---|---|---|
| AUD-001 / High | Invoke an inventory action while a prior save is busy: silent undefined return gives no clear outcome. | `inventoryCommit` returns false and shows retry feedback while retaining input. Repair case AUD-001. |
| AUD-002 / High | Bulk quantity containing malformed, negative, fractional or oversized values is stripped, floored or clamped. | Strict `bulkNumber` validation and explicit preview errors; six invalid quantity cases. |
| AUD-003 / High | Invalid bulk prices are coerced to zero. | Reject malformed/out-of-range monetary fields; preserve supported blank defaults. Eight price cases. |
| AUD-004 / High | Legacy `2p+7u` with 12 units per pack becomes 27 instead of 31; expiry column is misread. | Parse legacy pack columns explicitly and preserve metadata. Exact 31/20-unit examples. |
| AUD-005 / High | Supplier save fails storage after mutating memory and closing the editor. | Async `RS.atomic` handler, busy guard and disabled save control; close only after success. Failure retains fields and rolls back supplier state. |
| AUD-006 / High | Join returns an unsanitized enrollment checkpoint that a recipient rejects as having unclassified fields. | Return the persisted sanitized checkpoint. Four-context enrollment, trust and updates pass. |
| AUD-007 / High (privacy and availability) | Saved-day fingerprint embeds full transaction JSON; large days exceed shared-field limits and private notes can enter signed shared data. | Omit local-only fingerprint from shared day projection; keep it locally and in full backups. Large save/restore and private sentinel tests pass. |
| AUD-008 / High | Restored devices export unsigned original-owner events already represented by a verified checkpoint. | Exclude accounted event IDs from updates; retain origin checks on active events. Four-device merges and repeat imports converge. |
| AUD-009 / Medium | Repeated name parsing and unchanged batch comparison make a 5,000-product preview/import take about 109 seconds. | Bounded 8,192-name cache plus one-batch duplicate cache. 289 matcher pairs and eviction cases match baseline; duplicate blocking survives edits. Measured after-fix import 4.1 seconds in the targeted run and 5.8 seconds in the final run. |
| AUD-010 / Medium | Low-contrast status, helper, header and table-empty text in sampled theme states. | Separate screen-only `roseachy-audit-contrast` layer and theme-derived foreground/background tokens. Final scan samples ten themes, two modes and eight screens. |

All changes are confined to `index.html` at runtime. `bulkNumber`, `cachedParse`, `rsApplyAuditContrast`, cache/busy variables, supplier handler, shared day projection and the named changed functions are explicitly enumerated by the source-boundary regression. No ledger migration, deletion, permission weakening or new runtime dependency was introduced. See `audit/index.patch` for the exact baseline diff.

The financial test initially passed `bank` where the supported credit API requires `transfer`; correcting this fixture resolved the failure without a financial code change. A single aborted storage attempt can correctly succeed on retry; the persistent-abort test now aborts every retry. These are test corrections, not additional application defects.

## Business simulation and reconciliation

- Seed 150 products, 25 suppliers and 40 customers using supported workflows; 15 opening customer obligations total ₦75,000, separate from ₦50,000 Opening Debts. Initial product stock totals 300,000 units.
- Execute 500 sales via the Add control, including 100 repeated-click attempts, and 100 purchase receipts. Verify unique events, independent product arithmetic, totals, saved history and reload.
- A ₦25,000 credit sale is settled through ₦10,000 and ₦15,000 collections. Receivables settle without recognizing revenue or deducting stock a second time. Limit boundaries and concurrent collection rejection are exercised.
- Full backup restores twice into a fresh context with matching business state. Pack/loose arithmetic is independently checked.
- Controlled accounting: opening cash ₦100,000 and Business Bank ₦400,000; cash sale ₦20,000, bank sale ₦15,000, bank purchase ₦30,000 and prior-credit bank collection ₦10,000. Expected final cash ₦120,000, bank ₦395,000, combined ₦515,000, revenue ₦35,000 and stock 95 units. Bank postings require owner confirmation and repeated confirmations do not duplicate them.
- Four independent contexts merge 17 sales, one 50-unit receipt and one credit collection in differing orders. They converge to 133 stock units, 19 unique transactions, ₦17,000 sales and ₦9,000 receivables. Three repeat imports preserve state. Older backup restoration requires enrollment; tampered signatures are rejected.
- Persistent IndexedDB aborts and reload interruptions at 0/1/10/50 ms produce no partial stock/cash event. Invalid quantities leave state unchanged.

## UI, themes, platform and performance

The existing reference-theme design and architecture are retained. Original-theme screenshot comparisons isolate the new contrast layer; the original CSS remains unchanged outside named additive style blocks. All twelve non-HTML runtime assets match the previous release. Before/after screenshots are under `audit/system-screenshots`.

The test plan includes six required viewport sizes, visible navigation, persisted appearance choices, sampled mobile interactions and 160 contrast states. `audit/control-inventory.csv` lists 440 static form controls. It deliberately marks controls as not individually certified: scenario coverage cannot establish that every reachable control and input method was exercised.

Performance tests cover 100/1,000/5,000 products and 10,000/50,000 sale records. Large sale datasets use the production batch ledger API, not 50,000 physical clicks. Search dispatch timing is not end-to-end paint latency. Heap samples include warm-cache growth and do not certify absence of memory leaks. Timings are local measurements, not a latency SLA.

The PWA suite verifies a real service-worker offline reload in Chromium. Native Android bridge behavior is mocked. Deployment packaging validates exact root filenames, ZIP integrity and SHA-256 bytes; it does not demonstrate a live Cloudflare deployment.

## Compatibility and remaining acceptance gaps

1. **Legacy signed sync packages:** older offers/checkpoints containing local day fingerprints may fail canonical validation. Generate fresh packages using this version. Signature verification was not relaxed, and local history is retained.
2. **Physical devices and input:** Safari/Firefox, real Android/iOS installation, native file pickers, voice/IME/paste variants, every keyboard and swipe interaction are not exhaustively verified. They require those platforms and manual/device testing.
3. **Exhaustive controls and timing:** all 440 controls and every combination of dialogs, focus, race timing and long-session state have not been individually exercised. Long-duration memory behavior is not certified.
4. **Bank model:** the current product has one private Business Bank balance. The requested separate Bank A/Bank B scenario is not representable without a feature/model change; the supported single-account equivalent was tested.
5. **Deployment:** no production Cloudflare deploy was performed. Actual headers, HTTPS installation and upgrade behavior on the target origin require a deployed environment.
6. **Contrast scope:** sampled text passes are not an exhaustive accessibility certification of all populated dialogs, charts and dynamic states.

Because these acceptance gaps remain, the release is **not fully verified**. No claim is made that every financial operation or supported input method has an executable test. The included evidence identifies the actual tested operations.

## Deliverables and reproduction

The Cloudflare ZIP contains the thirteen runtime files at its root. The source ZIP adds tests, lockfile, packaging tool, request, baseline, report, screenshots and results. The preview ZIP contains the HTML preview only; use the full runtime for PWA behavior. A SHA-256 manifest accompanies the files.

Run `npm ci --prefix tests`, `npm test --prefix tests`, then `python3 tools/package.py` with Node 24+, Chromium and Python 3. All final regression checks rerun against the repaired build; pre-repair evidence is retained separately. See `VERIFICATION.md` for the final observed outcome.
