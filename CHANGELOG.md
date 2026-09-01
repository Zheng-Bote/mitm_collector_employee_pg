# Changelog

All notable changes to the `mitm_collector_employee_pg` project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v0.4.1] - 2026-09-01

### Fixed

- **IPC SSLMode Fix**: Fixed an issue where `SSLMode` was not correctly parsed from the scheduler's JSON configuration and improved the `MITM_DB_SSLMODE` fallback logic to support proper PostgreSQL sslmode strings (e.g., `require`, `verify-full`).

## [v0.4.0] - 2026-08-31

### Added

- **IPC Socket as Credential Broker**: The collector now fetches database credentials and the master key at runtime from the Scheduler via a Unix Domain Socket request (`get_credentials` with `RUN_ID` and `SCHEDULER_SOCKET_PATH`), instead of holding them locally.

### Changed

- **SQL Injection Prevention**: Replaced the naive inline table name sanitization with a strict `validateIdentifier` allowlist applied to both the table name and the cursor column.

## [v0.3.0] - 2026-08-29

### Changed/Added

- Configured `pgxpool` connection limits (`MaxConns=20`, `MaxConnIdleTime=5m`, `MaxConnLifetime=1h`).
- Implemented graceful shutdown with context cancellation on `SIGINT`/`SIGTERM`.
- Optimized performance with batched operations.

## [v0.2.0] - 2026-07-29

### Changed

- **Components Logging**: Refactored component version logging mechanism across all layers (Collectors, Transformation, Delivery, Scheduler) to consistently output a clean `Major.Minor.Patch` version format.

## [0.1.0] - 2026-07-24

### Added

- Created `mitm_collector_employee_pg` as an independent collector from `mitm_collector_pg`.
- Implemented an inline SQL `LEFT JOIN` between the primary table and the `org` table to satisfy the 1:N enrichment constraint required for downstream processing.
- Automatically denormalizes `orgid`, `short`, and `parent_org_id` into the Employee fragment payloads.
