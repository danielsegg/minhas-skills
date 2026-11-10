# Equipexe database context

Use this reference only for work in `/home/sistemas/projetos/equipexe`; re-check the live schema and runtime state before relying on any detail.

## Engines and locations

- `extracted_data/equipexe_app.db` is the application SQLite database for metadata, migrations, target-connection configuration, and financial synchronization audit entries.
- `extracted_data/stores/<store_slug>/equipexe.db` contains an isolated SQLite replica for an imported legacy store.
- The destination is MySQL/MariaDB accessed through PyMySQL. Connection details come from `crm_target_db_config`; never print its password.
- Original legacy files under `data/` are immutable. Generated artifacts belong under `extracted_data/`.

## Relevant code and documentation

- Database connection and target configuration: `dashboard/src/app/dependencies.py`, `dashboard/src/settings/repository.py`.
- Financial synchronization SQL and transaction orchestration: `dashboard/src/financeiro/service.py` and `dashboard/src/financeiro/repository.py`.
- Destination mapping: `docs/mapeamento_banco_destino.md` and `docs/mapeamento_financeiro_roll.md`.
- Runtime guidance and validation commands: `AGENTS.md`.

## Safe project checks

- Prefer repository commands and Docker for the authoritative runtime. Run pytest through Docker as required by `AGENTS.md`.
- For SQLite, use `EXPLAIN QUERY PLAN` and a disposable copy when a write-side statistic or experiment is needed.
- For MySQL/MariaDB, use read-only metadata and plan checks first: `EXPLAIN`, `EXPLAIN FORMAT=JSON`, `SHOW INDEX`, `information_schema.PROCESSLIST`, `information_schema.innodb_trx`, and lock metadata when available.
- Do not copy credentials from documentation examples into commands. Redact connection data from logs and reports.

## Known diagnostic lead

The financial sync checks for an existing `roll_logs` row with predicates on `roll_id`, `user_id`, `action`, and `created`, then inserts when absent. If this flow is slow, re-run `EXPLAIN` against the live schema and inspect existing indexes before proposing a composite index. Validate duplicate-log semantics, concurrent sync behavior, and migration lock impact; do not create the index automatically.
