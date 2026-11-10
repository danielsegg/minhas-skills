---
name: database-optimization
description: Evidence-based database performance diagnosis and query tuning for SQLite, MySQL, and MariaDB, including execution plans, indexes, locks, transactions, and connection behavior. Use when Codex needs to investigate slow queries, synchronization bottlenecks, database timeouts, missing indexes, schema performance, or safe optimization plans.
---

# Database Optimization

Diagnose database bottlenecks with measurable evidence and produce safe, reversible optimization recommendations. Treat production and user data as protected: analysis is read-only by default, and schema/data changes require explicit approval.

## Safety rules

- Do not run the supplied Claude Code `npx` installer. A package installer executes third-party code and is unnecessary once this skill is installed.
- Do not print passwords, connection strings, tokens, full environment variables, or query parameters containing sensitive data. Use the application's existing connection factory where possible.
- Start with read-only inspection. Do not create/drop indexes, alter schemas, update statistics, kill sessions, cancel jobs, change configuration, or modify data without explicit user approval for that exact action.
- Do not run `EXPLAIN ANALYZE` or equivalent runtime profiling against a live write or an expensive production query without approval; it can execute the statement. Prefer `EXPLAIN`, `EXPLAIN FORMAT=JSON`, MariaDB `ANALYZE FORMAT=JSON` only when approved, and SQLite `EXPLAIN QUERY PLAN`.
- Never modify original files under `data/`; prefer `extracted_data/`, fixtures, staging, or a disposable database for experiments.
- Before an approved write, identify the exact target, expected lock/availability impact, backup or rollback path, and validation query. Never use broad globs or destructive commands to select database targets.

## Workflow

### 1. Establish scope and engine

Identify the database engine/version, environment, workload, exact endpoint or job, query text, parameters, time window, and success criterion. Read the repository's database mapping and architecture documentation before making assumptions. For this project, read [project-database.md](references/project-database.md) when the task concerns the Equipexe dashboard.

### 2. Measure before changing

Capture a baseline using the narrowest safe checks:

- application logs and request duration;
- `EXPLAIN` or engine-specific plan output;
- row estimates versus actual counts when available;
- active sessions, transaction age, lock waits, and connection counts;
- representative data volumes and query frequency.

Record the exact query shape and relevant plan fields. Do not infer that an index helps solely because a column appears in a `WHERE` clause.

### 3. Diagnose the bottleneck

Classify the evidence as one or more of:

- access path or missing/low-selectivity index;
- joins, sorting, grouping, or excessive rows examined;
- lock contention, long transactions, or unsafe concurrency;
- connection creation, pooling, timeout, or network behavior;
- application-side N+1 queries, repeated scans, or oversized batches;
- stale statistics, data skew, or schema/type mismatch.

Check the full query pattern, including equality/range/order columns, joins, uniqueness, nullability, cardinality, and write cost. For synchronization code, inspect commit boundaries and idempotency before proposing concurrency changes.

### 4. Propose a controlled change

For each recommendation provide:

1. evidence and baseline;
2. exact query/schema change or application change;
3. expected plan and performance impact;
4. read/write, lock, storage, and maintenance tradeoffs;
5. migration and rollback steps;
6. verification and acceptance criteria.

Prefer the smallest change that addresses the measured bottleneck. A composite index must reflect the real predicate and ordering; include its write amplification and duplicate/uniqueness implications.

### 5. Implement only after approval

Use the repository's migration convention for DDL. Do not apply ad hoc SQL directly to a live database when a migration or disposable reproduction is appropriate. Keep business logic independent from database-specific details, and preserve route/API contracts. For tests, use fakes or a disposable database; do not start a real synchronization merely to validate a change.

### 6. Verify and report

Re-run the same representative query and plan, compare rows examined, duration, lock behavior, error rate, and affected counters, then run the narrowest relevant project validation. Report what was measured, what changed, what was not changed, and any remaining uncertainty.
