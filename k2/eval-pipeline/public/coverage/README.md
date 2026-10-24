# coverage.sqlite.gz — MrxMoex@Grok_Fleet
K2 coverage database (which EPIC x campaign keys were searched under which pipeline hash, with status). Schema: SCHEMA.md.
- gunzip -> coverage.sqlite, 116,527,104 bytes, sha256 34e3f532d499a425f44926ee7b8a80852b485dd1c0f8938b072429fe913e7f33 (493,018 rows)
- coverage.sqlite.gz: gzip -n -6, sha256 870eeb80a10c357b3b1f9e94d0d57cb88425729675cd2671dd27401d01fbb330
- Derived data only; input catalogs are NOT redistributed: see ../sources/MANIFEST_SOURCES.json (URL, sha256, row counts, official-channel check).
- Row-level recheck records: ../crosscheck/ — v1 bar + result (FAIL, run_id attribution only) AND v2 bar + result (PASS), see CROSSCHECK_AMENDMENT.md. Each bar was hashed before its run.
