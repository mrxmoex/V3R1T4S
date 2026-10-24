# CROSSCHECK BAR - coverage.sqlite (written + hashed BEFORE the check run)
Written 2026-10-24 (Europe/Berlin) by the prepare-only worker for Hephaestus. Applies to k2/eval-pipeline/public/coverage/coverage.sqlite
(sha256 34e3f532d499a425f44926ee7b8a80852b485dd1c0f8938b072429fe913e7f33, 116,527,104 B, 493,018 rows). Read-only. No rescan. Core-budget: <= 0.5 core-h total for the whole task.
Pre-run disclosure: before writing this bar I looked at (a) per-run/status row counts of the DB, (b) the R3 control file counts (108 True / 6 False). Neither was used to tune the thresholds below.

## A. R3 control (control, scan-level, re-derived)
Input: r3/R3_RESULTS.jsonl (rows with set=="control") and r3/R3_CONTROL.json (the recorded verdict; recorded: n_eval 114, recovered 108, GREEN, wilson95 [0.8899, 0.9757]).
Re-derive: n = number of distinct control keys; k = count r3_pass==true; Wilson 95% (z=1.959964) lower/upper bounds computed independently.
PASS iff n==114 AND k==108 AND Wilson lower >= 0.80 AND n >= 100 (derived status GREEN) AND the recorded n_eval/recovered/status/wilson95 (1e-9) equal the re-derived ones.
FAIL otherwise. (This confirms the stored R3 verdict is internally reproducible; it does not rerun the scan.)

## B. Coverage-DB spot recheck (exact match, independent re-derivation from stored scan data)
Independent re-implementation (crosscheck_coverage.py; does NOT import ingest_coverage.py) of the documented derivation from the 4 run dirs
(results/w*.jsonl complete lines, last row per (epic,campaign) wins; status candidate/null/scanned/no_data; state done-without-row -> no_data;
metric_hash from row 'mh' else legacy-<run> / EQUIV rule for k2-lp-01; later run wins on same primary key).
Fixed RNG: random.Random(20261024) over the sorted PK list.
1. Forward: N_db = 3000 random DB rows + ALL 376 status=candidate rows (union). Each must exist in the re-derived set and match EXACTLY on (status, kp, best_bls_snr, run_id); floats compared with ==, NaN == NULL.
2. Reverse: N_run = 3000 random re-derived rows (same RNG continued). Each must exist in the DB with exact match on the same four fields.
3. Structural: PRAGMA integrity_check == ok; DB row count == re-derived row count; superseded == 0 for all rows (informational field).
PASS iff A passes AND B1 mismatches == 0 AND B2 mismatches == 0 AND B3 holds. Any single mismatch => FAIL (no tolerance, no re-draw, no repair in this step).
A full all-rows comparison is also printed as INFORMATIONAL only (non-gating).
## C. Payload integrity (checked at build time)
gzip(coverage.sqlite) gunzips to a file with sha256 equal to the sha above, size < 100 MiB (104,857,600 B).
## Consequence of FAIL
The coverage DB is marked FAIL in DIFF_SUMMARY.md, not pushed by this step; parent/moe decide. Nothing is silently fixed.
