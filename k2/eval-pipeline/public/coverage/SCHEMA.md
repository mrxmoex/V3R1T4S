# Coverage DB spec (mirrors TESS ledger, key adapted to K2)
Key: (epic, campaign, product, metric_hash). SQLite, single writer (ingest), WAL.
```sql
CREATE TABLE coverage(epic INT, campaign INT, product TEXT, metric_hash TEXT,
  status TEXT CHECK(status IN ('scanned','null','candidate','no_data')),
  kp REAL, best_bls_snr REAL, run_id TEXT, ingested_utc TEXT, superseded INT DEFAULT 0,
  PRIMARY KEY(epic,campaign,product,metric_hash));
CREATE TABLE pipelines(metric_hash TEXT PRIMARY KEY, manifest_json TEXT, label TEXT, created_utc TEXT);
CREATE TABLE runs(run_id TEXT PRIMARY KEY, metric_hash TEXT, started_utc TEXT, ended_utc TEXT, note TEXT);
CREATE TABLE ingest_log(ts TEXT, run_id TEXT, inserted INT, updated INT, unchanged INT, skipped_partial INT);
```
Status labels (as written by the ingester; all four = covered):
| status | meaning | old spec label |
|---|---|---|
| candidate | result row with pass_all true | searched |
| null | vetted row (pass_all/pass_basic present), no pass | searched |
| scanned | gate-level row, no vet fields (below trigger); NOTE: worker `nodata:true` rows (MAST returned no LC) currently also land here | searched |
| no_data | key in state `done` with no result row (LC <200 pts or BLS returned nothing) | no_data |
`failed` is never stored: failures stay out of `done` and live in the run's requeue files until retried.
`run_id` in public exports = opaque label (e.g. `k2-lp-01`), never a host folder name.

## Known scope so far (LP pass, values from kit; to be derived by code, not hand-typed)
| field | value |
|---|---|
| search_pass | long-period 2-transit |
| campaigns | 0-8,10-19 |
| kp cut | Kp <= 15 (catalog col 3), dedup by EPIC per campaign |
| period grid | log-uniform, 8000 pts, span/3 .. 0.95*span |
| durations | base DURS + 0.3, 0.4 d |
| BLS objective | snr; trigger gate bls_snr >= 7 |
| vet | depth SNR>=7, odd-even <3 sigma, null: bls_snr > null_max_snr+1, TLS agree (P,P/2,2P @1%), inj-rec >= 0.8, pixel centroid <1 px |
| catalog tie rule | known in k2pandc/KOI/PS = TIE, not NEW |
