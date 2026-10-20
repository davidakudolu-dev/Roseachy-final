# Final verification — 2.13.1

Execution: 2026-10-20T00:17:56.461Z. Node v24.19.0, Linux, Chromium.

**120 automated checks passed, 0 failed.** All 14 suite processes exited successfully, including the static audit. These counts describe the executable plan, not all conceivable product cases. All final checks were rerun on the repaired build. Detailed subcases (160 contrast states, 500 UI sales, four contexts and large datasets) are contained within those checks.

**Release remains not fully verified** for the acceptance gaps listed in AUDIT_REPORT.md. No cases are counted as passing merely because they were skipped or unrun. Entire unimplemented acceptance scenarios remain outside this count.

| Result file | Passed | Failed |
|---|---:|---:|
| contrast-results | 1 | 0 |
| failure-results | 6 | 0 |
| financial-results | 4 | 0 |
| integrity-results | 10 | 0 |
| multidevice-results | 7 | 0 |
| performance-results | 5 | 0 |
| platform-results | 5 | 0 |
| regression-results | 19 | 0 |
| repair-results | 5 | 0 |
| sync-results | 11 | 0 |
| system-results | 13 | 0 |
| theme-results | 12 | 0 |
| workflow-results | 22 | 0 |

## Final local performance samples

| Dataset | Measurements |
|---|---|
| PERF-100 | `{"products":100,"bulkImportMs":110,"startupMs":159,"searchDispatchMs":3,"navigation40TransitionsMs":2994,"heapBefore":3740628,"heapAfter":4931388,"heapDelta":1190760,"visibleRows":1}` |
| PERF-1000 | `{"products":1000,"bulkImportMs":626,"startupMs":430,"searchDispatchMs":0,"navigation40TransitionsMs":3581,"heapBefore":13795188,"heapAfter":14120408,"heapDelta":325220,"visibleRows":1}` |
| PERF-5000 | `{"products":5000,"bulkImportMs":5767,"startupMs":1855,"searchDispatchMs":0,"navigation40TransitionsMs":3904,"heapBefore":58246788,"heapAfter":80692364,"heapDelta":22445576,"visibleRows":1}` |
| PERF-TX-10000 | `{"saveMs":490,"renderMs":445,"count":10000,"stock":90000,"sales":10000000,"startupMs":495}` |
| PERF-TX-50000 | `{"saveMs":2108,"renderMs":1779,"count":50000,"stock":50000,"sales":50000000,"startupMs":3203}` |

Timing is in milliseconds and heap sizes in bytes. Search dispatch is not paint latency. Heap growth includes cache warm-up; leak freedom is not certified. See the report for baseline comparison and boundaries.

The runtime ZIP contains 13 verified files; all 12 non-HTML files match the 2.13.0 baseline byte-for-byte. Packaging checks CRC, exact filenames and source-byte hashes. No Cloudflare deployment was performed.
