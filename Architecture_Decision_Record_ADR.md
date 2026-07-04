# Architecture_Decision_Record_ADR.md

# Architecture Decision Record (ADR)

Project: AI Job Intelligence Platform Status: Accepted Version: 1.0

## Purpose

This document records the key architectural decisions made during the
design of the AI Job Intelligence Platform. These decisions act as
long-term guidance for future contributors and AI coding assistants.

------------------------------------------------------------------------

# ADR-001 --- Single Google ADK Orchestrator

## Decision

Use exactly one Google ADK agent as the orchestrator.

## Rationale

-   Simpler orchestration
-   Easier testing
-   Clear separation of concerns
-   Business logic lives in tools and services

## Consequence

All intelligence outside conversation planning belongs in services or
plugins.

------------------------------------------------------------------------

# ADR-002 --- Plugin-Based Connector Architecture

## Decision

Every job source must be implemented as a connector plugin.

## Rationale

-   Extensibility
-   Loose coupling
-   Easy maintenance

## Consequence

Adding a new ATS requires only a new plugin.

------------------------------------------------------------------------

# ADR-003 --- Dynamic Connector Registry

## Decision

Load connectors dynamically at startup.

## Rationale

-   No hardcoded connector list
-   Automatic discovery
-   Easier contribution model

------------------------------------------------------------------------

# ADR-004 --- Common Connector Contract

## Decision

Every connector implements:

-   detect()
-   validate()
-   search()
-   health_check()
-   repair()

## Rationale

Uniform behaviour across all job sources.

------------------------------------------------------------------------

# ADR-005 --- TinyDB for Persistence

## Decision

Use TinyDB for Version 1.

## Rationale

-   Zero configuration
-   Local-first
-   Lightweight
-   Easy migration later

------------------------------------------------------------------------

# ADR-006 --- Async Parallel Search

## Decision

Execute connector searches concurrently with asyncio.

## Rationale

Searching many companies is I/O bound.

------------------------------------------------------------------------

# ADR-007 --- Company Registration by Name

## Decision

Users register companies using company names rather than ATS URLs.

## Workflow

Company Name → Discover official careers page → Detect ATS → Validate →
Register

------------------------------------------------------------------------

# ADR-008 --- Automatic ATS Detection

## Decision

Detect the ATS automatically.

Initial support: - Greenhouse - Lever - Ashby - Workday -
SmartRecruiters - Generic HTML / JSON-LD

------------------------------------------------------------------------

# ADR-009 --- Connector Self-Test & Self-Healing

## Decision

Every connector exposes health_check() and repair().

## Behaviour

Startup: - Validate - Health check

Runtime: - Retry - Repair - Disable if necessary

Recovery: - Re-enable automatically when healthy.

------------------------------------------------------------------------

# ADR-010 --- Common Job Model

## Decision

Every connector returns the same normalized Job model.

Required fields: - Company - Designation - Location - Posted Date -
Description - Apply URL - Connector - Source

------------------------------------------------------------------------

# ADR-011 --- Gemini Usage Policy

## Decision

Gemini is used only for concise job description summaries.

Gemini must NOT be used for: - Search - Ranking - ATS detection -
Filtering - Connector logic

------------------------------------------------------------------------

# ADR-012 --- Incremental Search

## Decision

Only process new or modified jobs after initial discovery.

## Benefits

-   Faster searches
-   Reduced processing
-   Lower LLM usage

------------------------------------------------------------------------

# ADR-013 --- Graceful Degradation

## Decision

Failure of one connector must never fail the overall search.

The platform should: - Log failures - Continue processing - Return
partial results - Retry failed connectors

------------------------------------------------------------------------

# ADR-014 --- Configuration-Driven Design

## Decision

Behaviour should be driven by configuration wherever practical.

Examples: - Retry counts - Timeouts - Concurrency - Enabled connectors

------------------------------------------------------------------------

# ADR-015 --- Architecture Invariants

The following rules must remain true unless superseded by a future ADR:

1.  One Google ADK orchestrator.
2.  Plugin-based connector architecture.
3.  Registry-driven execution.
4.  Async parallel search.
5.  TinyDB for local persistence (v1).
6.  Common Job model.
7.  Self-testing connectors.
8.  Self-healing connectors.
9.  No ATS logic inside the orchestrator.
10. Modular, extensible, production-quality code.
