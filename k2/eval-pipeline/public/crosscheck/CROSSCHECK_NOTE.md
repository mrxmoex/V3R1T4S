# Crosscheck of coverage.sqlite — MrxMoex@Grok_Fleet (2026-10-24, Europe/Berlin)
Bar: CROSSCHECK_BAR.md (sha256 d056a01055663c12964ac07da5fb03ecbb851914f7e95035d0412dde63a9e9a9), hashed 12:20:38 before any check ran. Result: CROSSCHECK_RESULT.json.
Outcome AS PRE-REGISTERED: **FAIL**.
- R3 control re-derived from stored per-star results: 108/114 recovered, Wilson95 lower 0.8899 -> GREEN, identical to the recorded verdict (PASS).
- Spot recheck: 3,374 DB rows (3,000 seeded random + all 376 candidates) and 3,000 re-derived rows: 0 value mismatches (status, kp, best_bls_snr);
  14 forward mismatches, all in the 4th compared field run_id only (DB says k2-lp-01, re-derivation said k2-lp-02). Cause: the ingester keeps the first run_id when a later run writes identical values (ingest log: k2-lp-02 inserted 38, unchanged 14); my bar assumed last-writer. Reverse direction: 0 mismatches. Integrity ok; 493,018 = 493,018 rows.
- A post-hoc diagnostic with run_id excluded gives 0/0 mismatches. It does NOT replace the pre-registered result.
