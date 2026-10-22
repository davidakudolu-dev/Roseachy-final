# Verification — Roseachy Desk 2.15.0

Final verification: **190 checks passed, 0 failed**, across 19 suites (including static analysis). Executed 2026-10-22T16:59:21.206Z with v24.19.0 on Linux Chromium. Full logs/results: audit/final. The initial run passed 189 checks; one visual comparison included a header logo whose explicit visibility overrode the mask. Correcting that test-only mask and rerunning all 12 theme checks resolved it. The initial failure and recheck are retained; this is a consolidated result, not a claim of a single all-green run.

| Suite | Passed / checks |
|---|---|
| contrast-results.json | 1 / 1 |
| credit-daily-results.json | 18 / 18 |
| credit-daily-sync-results.json | 7 / 7 |
| credit-edit-results.json | 20 / 20 |
| failure-results.json | 6 / 6 |
| financial-results.json | 4 / 4 |
| integrity-results.json | 10 / 10 |
| multidevice-results.json | 7 / 7 |
| navigation-results.json | 8 / 8 |
| performance-results.json | 5 / 5 |
| platform-results.json | 5 / 5 |
| regression-results.json | 19 / 19 |
| repair-results.json | 5 / 5 |
| sync-results.json | 11 / 11 |
| system-results.json | 13 / 13 |
| theme-cleanup-results.json | 17 / 17 |
| theme-results.json | 12 / 12 |
| workflow-results.json | 22 / 22 |

The final receipt-reference wording now includes the customer in compact rows. After that display-only change, all 18 credit-daily checks passed again; evidence is under audit/rechecks/receipt-label. The full run's later source-boundary and theme/contrast checks also used this final source.

## Independent four-day reconciliation

| Business date | Activity | Cash | Confirmed bank | Customer debt | New turnover | Collection | Stock |
|---|---|---:|---:|---:|---:|---:|---:|
| 2026-10-22 | Unpaid purchase 50000 | 100000 | 200000 | 50000 | 50000 | 0 | 90 |
| 2026-10-23 | Cash collection 20000 | 120000 | 200000 | 30000 | 0 | 20000 | 90 |
| 2026-10-24 | Transfer collection 15000, owner confirmed | 120000 | 215000 | 15000 | 0 | 15000 | 90 |
| 2026-10-25 | Cash settlement 15000 | 135000 | 215000 | 0 | 0 | 15000 | 90 |

Opening product stock was 100; ten units were sold only on day 1. Turnover and total collections each equal 50000. Bank receipts did not change the protected balance before owner confirmation. The simulation uses real UI save/start-day transitions and a controlled browser clock.

Subsequent payment correction 15000→12000 restored debt by 3000; cash→transfer moved source without a new payment; confirmed bank rose to 227000; reversal restored bank to 215000 after owner approval and removed the active receipt while retaining audit history. Tests separately cover initial deposits, purchase editing, blocked overpayments/cancellations, backup/reload and actual IndexedDB write failures.

## Sync and navigation

Independent signed browser contexts verify offline unpaid purchases produce no receipts, actual collections arrive once, corrections preserve ID/date/audit, stale editors reject synchronized changes, and reversal restores debt without changing inventory. Repeated imports are exercised, along with the earlier four-device/concurrent-edit suites.

Navigation checks cover 320, 390, 768 and 1280px × six themes × both modes (48 combinations), plus orientation, rapid taps, modal Back/Escape, browser Back, tab swipes and restart. The supplied right-side Credit button stays in place; the new Sales button uses user-approved existing header styling. Both left-menu duplicates are absent. Evidence screenshots are under audit/credit-daily-screenshots.

Source-boundary tests protect unrelated JavaScript against the supplied 2.14.3 backup. Eleven other runtime assets are identical; the service worker differs only in its release identifier. The six retained palette definitions equal the earlier verified palettes. Four obsolete-theme backup migrations preserve business records. The contrast scan covers 96 sampled theme/mode/screen states.

## Boundaries

Historical days remain read only. Purchase-specific allocation for legacy account-level collections and unsupported refund/customer-credit allocation are not invented; unsafe corrections are blocked and explained. Chromium viewport/PWA tests do not certify physical devices or native Android file pickers. The Pages artifact is packaged and byte-verified, not deployed. See AUDIT_REPORT.md for details.
