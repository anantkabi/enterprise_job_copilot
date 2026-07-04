# Sprint 03 — Company Registration

## Goal
Enable users to register companies and map them to the correct connector with minimal manual input.

## Single-response tasks
1. Implement the company registration service for creating a company record by name.
2. Add company storage persistence and lookup helpers.
3. Implement automatic career page discovery logic using the company name.
4. Implement ATS detection heuristics for supported company cases.
5. Add company-to-connector mapping and persistence in storage.
6. Add fallback handling and logging for failed discovery or detection attempts.
7. Add tests for registration, mapping, and ATS detection flows.

## Deliverables
- Companies can be registered and stored.
- The platform can infer a connector for a company.
- Registration flows are resilient and tested.

## Definition of Done
- Company registration works end to end.
- ATS discovery and mapping are functional for supported scenarios.
- Tests cover the main registration behaviors.
