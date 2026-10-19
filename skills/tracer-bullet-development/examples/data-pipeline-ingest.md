# Ingest supplier price feeds

## Goal
Daily supplier price files (CSV over SFTP) land in the warehouse and appear on the pricing dashboard.

## Assumptions
- Airflow for scheduling, Postgres warehouse, Metabase dashboard; three suppliers with different CSV formats.

## Layers touched
| Layer | Location | Notes |
|---|---|---|
| Source | Supplier SFTP | Read files |
| Orchestration | `dags/supplier_prices.py` | New DAG |
| Transform | `pipelines/prices/transform.py` | New; normalise columns |
| Storage | `warehouse.supplier_prices` | New table |
| Output | Metabase "Pricing" dashboard | New card |

## Phase 1 — Tracer bullet
**User action:** Trigger the DAG and see one supplier's prices on the dashboard.

**Path:** DAG run → download one supplier's latest CSV → map 3 columns (sku, price, date) → insert into `supplier_prices` → dashboard card shows row count and latest prices.

**Shortcuts allowed:** one supplier only, manual trigger, no deduplication.

**Acceptance criteria:**
- [ ] Manual DAG run makes rows appear on the dashboard card
- [ ] End-to-end test: run the DAG against a test SFTP fixture and assert rows in the table

## Phase 2 — All suppliers
**Adds:** per-supplier column mappings for all three suppliers; daily schedule.

**Acceptance criteria:**
- [ ] All three suppliers load on schedule
- [ ] Test: fixture file per supplier maps to the same normalised schema

## Phase 3 — Data quality
**Adds:** deduplication on (supplier, sku, date); reject rows with bad prices into a quarantine table; alert on failures.

**Acceptance criteria:**
- [ ] Re-running the same day creates no duplicates
- [ ] Test: bad rows end up in quarantine

## Phase 4 — History and trends
**Adds:** price change history and a trend chart on the dashboard.

## Deferred
- Real-time feeds via API
- Currency conversion

## Risks and open questions
- SFTP credentials and network access from Airflow workers (Phase 1 exposes this)
- Inconsistent CSV encodings across suppliers
