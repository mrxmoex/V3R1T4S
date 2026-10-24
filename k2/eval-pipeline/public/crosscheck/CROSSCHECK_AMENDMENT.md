# Crosscheck amendment (bar v2) — MrxMoex@Grok_Fleet (2026-10-24, Europe/Berlin)
Both records are shipped. Neither replaces the other.
| | v1 | v2 |
|---|---|---|
| bar | CROSSCHECK_BAR.md, sha256 d056a01055663c12964ac07da5fb03ecbb851914f7e95035d0412dde63a9e9a9, hashed 12:20:38 CEST | CROSSCHECK_BAR_v2.md, sha256 25a3502e0d2695727cc314f5a9b2d32f9183c6bd7f2430f443ad7bea669881ff, hashed 12:42:58 CEST (before any v2 check ran) |
| seed | 20261024 | 190104845 (fresh; v1 seed not reused) |
| compared | status, kp, best_bls_snr, run_id | status, kp, best_bls_snr (+ key existence); run_id NON-compared |
| result | CROSSCHECK_RESULT.json: **FAIL** (14 forward mismatches, all run_id attribution only; reverse 0; R3 control GREEN 108/114) | CROSSCHECK_RESULT_v2.json: **PASS** (forward 3,375 rows 0 mismatches; reverse 3,000 rows 0 mismatches; R3 control re-derived GREEN 108/114, Wilson95 [0.8899, 0.9757]) |
- The v1 FAIL stands and stays in the record (CROSSCHECK_BAR.md, CROSSCHECK_RESULT.json, CROSSCHECK_NOTE.md are unchanged).
- Cause of the v1 FAIL: bar error, not data error. The ingester keeps the first run_id when a later run writes identical values (ingest log: k2-lp-02 inserted 38, unchanged 14); the v1 bar assumed the later run_id wins. The 14 mismatching rows show DB k2-lp-01 vs re-derived k2-lp-02, with equal status, kp and best_bls_snr.
- Bar v2 is a dated amendment (2026-10-24, after the v1 FAIL) that removes run_id from the compared fields and adds nothing else except gating superseded == 0, which only tightens the bar.
- v2 result is the pre-registered result of bar v2. In the v2 run, the informational all-rows pass counted 14 run_id attribution differences (non-compared) and 0 differences in compared fields; 493,018 DB rows count-match 493,018 re-derived rows.
- Wording: where only counts or fingerprints were compared the records say "count match" / "count+Period-sum match" / "size match"; "content equal" is used only where a hash or row comparison backs it (the row-level comparison above; the gunzip sha256).
