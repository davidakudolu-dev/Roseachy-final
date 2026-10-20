# Verification — 2.14.0

**Final results: 140 automated checks passed, 0 failed; 0 static findings.** This includes 20 new credit-edit checks and the existing 120 checks.

The full run returned 139 passes and 1 failure(s). Its protected-theme comparison differed at 23 pixels around the unchanged logo (maximum channel difference 8). The original failed images and result are retained under audit/regression-attempts. The same unmodified screenshot comparison was rerun; no pixel tolerance was relaxed.

After the last credit-editor refinement (locked captured price and correct substituted-product unit price), the 20 credit-edit checks, static audit and theme/source-boundary suite were rerun successfully. Final result files and all run logs are under audit/final. The final counts aggregate the full regression and these targeted rechecks; they do not claim a single uninterrupted passing run.

| Suite | Passed | Failed |
|---|---:|---:|
| regression | 19 | 0 |
| workflow | 22 | 0 |
| integrity | 10 | 0 |
| sync | 11 | 0 |
| platform | 5 | 0 |
| repair | 5 | 0 |
| credit-edit | 20 | 0 |
| failure | 6 | 0 |
| financial | 4 | 0 |
| system | 13 | 0 |
| multidevice | 7 | 0 |
| performance | 5 | 0 |
| theme | 12 | 0 |
| contrast | 1 | 0 |

Final index.html SHA-256: `62d795edfed2645421acec0ad8141805985a92d548af7c0aab3e3a937af45d3b`.

The main style blocks and all twelve non-HTML runtime files exactly match 2.13.1. Packaging verifies thirteen root deployment files, ZIP integrity, member names and byte hashes. No production Cloudflare deploy was performed.

Tests ran on Linux with Node 24 and Chromium in synthetic independent contexts. Physical devices, native Android file handling and live Cloudflare behavior remain unverified. See AUDIT_REPORT.md for the conservative payment guard and existing historical-day/pricing restrictions.
