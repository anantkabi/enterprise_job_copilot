# Sprint 04 — Parallel Search

## Goal
Search all registered companies concurrently and return aggregated results.

## Single-response tasks
1. Implement the search planner that selects the connectors for a search request.
2. Add async execution support for connector searches.
3. Add concurrency controls so searches run in parallel without overwhelming the system.
4. Normalize raw connector results into the common job schema.
5. Aggregate results from all connectors into one response structure.
6. Add timeouts, retries, and isolation so one connector failure does not stop the full search.
7. Add tests for parallel execution, aggregation, and graceful degradation.

## Deliverables
- Parallel search execution is available.
- Results from multiple connectors are merged into one output.
- Connector failures are isolated and logged.

## Definition of Done
- Searches run across multiple connectors in parallel.
- One failing connector does not block the whole workflow.
- Search orchestration is tested and stable.
