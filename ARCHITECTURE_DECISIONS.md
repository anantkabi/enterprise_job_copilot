# ARCHITECTURE_DECISIONS.md

# Enterprise Job Copilot

## Architecture Decision Records (ADR)

**Version:** 1.0

**Status:** Active

**Last Updated:** July 2026

---

# Purpose

This document records every major architectural decision made during the design and implementation of Enterprise Job Copilot.

The objective is to:

- Explain why architectural decisions were made.
- Prevent repeated discussions about previously resolved problems.
- Help future contributors understand trade-offs.
- Ensure long-term architectural consistency.
- Provide guidance for future enhancements.

---

# ADR Format

Each Architecture Decision Record (ADR) contains:

- Context
- Problem
- Decision
- Alternatives Considered
- Consequences
- Future Considerations

---

# ADR-001

## Title

Single AI Orchestrator

### Status

Accepted

### Context

The platform requires an AI component capable of understanding natural language and coordinating complex workflows.

Without a centralized orchestrator, business logic becomes fragmented across connectors and services.

### Decision

Use exactly one Google ADK Agent as the primary orchestration layer.

The orchestrator is responsible for:

- Intent detection
- Planning
- Tool invocation
- Workflow coordination

The orchestrator MUST NOT:

- Scrape websites
- Parse ATS responses
- Execute connector logic

### Alternatives Considered

Multiple specialized agents.

Rejected because:

- Increased complexity
- Difficult debugging
- Context synchronization issues

### Consequences

Advantages

- Single entry point
- Easier maintenance
- Simpler debugging
- Better prompt management

Trade-offs

- Larger orchestrator prompts
- Requires well-defined tools

---

# ADR-002

## Title

Plugin-Based Connector Architecture

### Status

Accepted

### Context

Hundreds of ATS platforms exist.

Hardcoding integrations would create a monolithic codebase.

### Decision

Every ATS integration shall be implemented as a plugin.

Plugins implement a common interface.

### Alternatives

Hardcoded implementations.

Rejected.

### Consequences

Advantages

- Unlimited extensibility
- Independent testing
- Easier maintenance
- Third-party plugin support

Trade-offs

- Slight abstraction overhead

---

# ADR-003

## Title

Connector Registry as Single Source of Truth

### Status

Accepted

### Context

The orchestrator should never know connector implementation details.

### Decision

Introduce a Connector Registry.

Responsibilities

- Discover plugins
- Register plugins
- Validate plugins
- Track health
- Route searches

### Alternatives

Manual imports.

Rejected.

### Consequences

Advantages

- Dynamic discovery
- No code changes for new connectors

---

# ADR-004

## Title

Async-First Execution Model

### Status

Accepted

### Context

Searching 50 companies sequentially is slow.

### Decision

Use asyncio for all connector execution.

### Alternatives

Thread pools

Rejected due to:

- Higher memory
- Lower scalability

Sequential execution

Rejected.

### Consequences

Expected search latency

Under 10 seconds for 100 companies.

---

# ADR-005

## Title

Pydantic Domain Models

### Status

Accepted

### Context

Data validation is critical.

### Decision

All domain objects must inherit from Pydantic BaseModel.

### Alternatives

Dataclasses

Rejected.

Named tuples

Rejected.

### Consequences

Benefits

- Validation
- Serialization
- Type safety

---

# ADR-006

## Title

TinyDB for Version 1

### Status

Accepted

### Context

Version 1 is intended for local execution.

### Decision

Use TinyDB.

Collections

- companies
- jobs
- settings
- plugins

### Alternatives

SQLite

PostgreSQL

MongoDB

Rejected for Version 1.

### Future

Migration planned for Version 2.

---

# ADR-007

## Title

Gemini Reserved for AI Enrichment

### Status

Accepted

### Context

Using LLMs for every operation increases cost.

### Decision

Gemini is used only for:

- Job summaries
- Skill extraction
- Experience extraction
- Role classification

Gemini is NOT used for:

- Searching
- Scraping
- ATS detection
- Plugin selection

---

# ADR-008

## Title

Connector Isolation

### Status

Accepted

### Context

A broken connector must not affect others.

### Decision

Each connector executes independently.

Failures are isolated.

### Consequences

If Workday fails,

Greenhouse continues.

---

# ADR-009

## Title

Dependency Injection

### Status

Accepted

### Context

Tightly coupled services reduce testability.

### Decision

Services communicate through interfaces.

Dependencies are injected.

### Benefits

- Easier testing
- Easier mocking
- Cleaner architecture

---

# ADR-010

## Title

Repository Pattern

### Status

Accepted

### Context

Storage technology will evolve.

### Decision

Business logic never accesses TinyDB directly.

Repositories abstract persistence.

---

# ADR-011

## Title

Common Job Model

### Status

Accepted

### Context

Every ATS exposes different schemas.

### Decision

Normalize every job into a common schema.

Fields include:

- id
- company
- title
- location
- remote
- employment type
- description
- apply url
- source
- connector

---

# ADR-012

## Title

Incremental Search

### Status

Accepted

### Context

Repeatedly downloading identical jobs wastes bandwidth.

### Decision

Only persist new or updated jobs.

Duplicate detection uses:

- URL
- Hash
- Job ID

---

# ADR-013

## Title

Health Monitoring

### Status

Accepted

### Decision

Every connector implements:

- validate()
- health_check()
- repair()

Registry continuously tracks health.

---

# ADR-014

## Title

Structured Logging

### Status

Accepted

### Decision

Every component logs through a centralized logger.

Logs include:

- timestamp
- component
- request id
- severity
- execution time

---

# ADR-015

## Title

Configuration Driven System

### Status

Accepted

### Decision

Business rules must not be hardcoded.

Configuration lives in:

- YAML
- Environment variables

---

# ADR-016

## Title

Strong Typing Everywhere

### Status

Accepted

### Decision

All public APIs must include:

- Type hints
- Pydantic validation
- Static type checking

---

# ADR-017

## Title

One Responsibility Per Module

### Status

Accepted

### Decision

Every module should solve one problem.

Avoid "utility" dumping grounds.

---

# ADR-018

## Title

Production-Quality Testing

### Status

Accepted

### Decision

Every feature requires:

- Unit tests
- Integration tests
- Mock connector tests

No feature is complete without tests.

---

# ADR-019

## Title

Observability First

### Status

Accepted

### Decision

Every major workflow emits telemetry.

Metrics include:

- Search latency
- Connector failures
- Retry counts
- AI execution time

---

# ADR-020

## Title

Incremental Development

### Status

Accepted

### Context

Large-scale projects become difficult to stabilize when developed monolithically.

### Decision

Development proceeds strictly sprint by sprint.

No future sprint may depend on unfinished work from a previous sprint.

---

# Future ADRs

The following decisions will be recorded as the project evolves:

- PostgreSQL migration
- Redis caching
- Vector search
- Authentication
- Multi-user support
- Cloud deployment
- Kubernetes architecture
- Event-driven architecture
- Message queues
- Resume matching engine
- Recommendation engine
- Notification framework
- Plugin marketplace

---

# ADR Review Process

Any architectural change must:

1. Create a new ADR.
2. Reference affected ADRs.
3. Explain the rationale.
4. Describe migration strategy.
5. Be reviewed before implementation.

Existing ADRs are never modified retroactively.

Instead:

- Mark as Superseded
- Reference the replacing ADR

This preserves architectural history.

---

# Summary

This document represents the architectural contract of Enterprise Job Copilot.

Every implementation decision should align with these ADRs unless a newer ADR explicitly supersedes an earlier decision.
