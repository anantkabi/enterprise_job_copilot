# PROJECT_BLUEPRINT.md

# AI Job Intelligence Platform

Version: 1.0

# Executive Summary

Build a production-quality AI Job Intelligence Platform using Google ADK
and MCP that discovers, registers, searches and aggregates jobs from
multiple company career sites and ATS providers in parallel.

The system is plugin-driven, extensible, self-healing, and local-first.

------------------------------------------------------------------------

# Vision

Provide a conversational AI agent capable of:

-   Registering companies by name.
-   Automatically discovering official career pages.
-   Detecting the underlying ATS.
-   Searching all registered companies concurrently.
-   Returning concise job results.

The platform should allow new job sources to be added simply by dropping
in a new connector plugin.

------------------------------------------------------------------------

# Scope (v1)

Included

-   Google ADK orchestration
-   MCP plugin architecture
-   Dynamic connector registry
-   Company registration
-   ATS detection
-   Parallel search
-   TinyDB persistence
-   Job normalization
-   Duplicate detection
-   Incremental search
-   Gemini job description summaries
-   Connector health checks
-   Connector self-healing

Excluded

-   Resume matching
-   Salary prediction
-   Notifications
-   Authentication
-   Multi-user
-   Cloud deployment
-   Admin UI
-   PostgreSQL
-   Vector database

------------------------------------------------------------------------

# Architecture Principles

1.  Exactly one Google ADK orchestrator.
2.  The orchestrator never knows connector implementation details.
3.  Every job source is implemented as a plugin.
4.  The Connector Registry is the single source of truth.
5.  Every plugin implements the same contract.
6.  Searches execute asynchronously.
7.  TinyDB provides persistence.
8.  Gemini is only used for JD summarization.

------------------------------------------------------------------------

# High-Level Architecture

User → Google ADK Agent → Search Planner → Connector Registry →
Connector Health Manager → MCP Plugins → Async Search Engine → Job
Normalizer → Duplicate Detector → TinyDB → Gemini Summary → Response
Formatter → User

------------------------------------------------------------------------

# Functional Requirements

FR-1 Register companies by company name.

FR-2 Discover official career pages automatically.

FR-3 Detect ATS automatically.

FR-4 Register companies without user knowing ATS.

FR-5 Load connector plugins dynamically.

FR-6 Search all registered companies concurrently.

FR-7 Normalize every job into one common schema.

FR-8 Remove duplicates.

FR-9 Cache jobs locally.

FR-10 Produce concise AI summaries.

FR-11 Self-test every connector.

FR-12 Self-heal connectors where possible.

FR-13 Gracefully degrade when connectors fail.

------------------------------------------------------------------------

# Non-Functional Requirements

-   Async-first
-   Modular
-   Extensible
-   Local-first
-   Plugin-driven
-   Production-quality
-   Unit-testable
-   Strong typing (Pydantic)
-   Configurable
-   Observable through logging

------------------------------------------------------------------------

# Plugin Lifecycle

Startup

Discover Plugins → Validate → Health Check → Register

Search

Planner → Registry → Plugins → Results

Failure

Health Check → Retry → Repair → Disable

Recovery

Refresh → Validate → Re-enable

------------------------------------------------------------------------

# Connector Interface

Required methods:

-   detect()
-   search()
-   validate()
-   health_check()
-   repair()

Metadata:

-   connector_name
-   version
-   priority
-   concurrency_limit
-   supported_filters
-   retry_policy

------------------------------------------------------------------------

# Common Job Model

-   id
-   company
-   designation
-   location
-   country
-   city
-   remote
-   employment_type
-   posted_date
-   summary
-   description
-   apply_url
-   source
-   connector
-   hash

------------------------------------------------------------------------

# TinyDB Collections

companies.json

jobs.json

settings.json

plugin_metadata.json

------------------------------------------------------------------------

# Folder Structure

job-finder-agent/

agent/ connector_sdk/ plugins/ registry/ services/ tools/ models/
storage/ config/ tests/ docs/

------------------------------------------------------------------------

# Sprint Plan

Sprint 0 Foundation

Sprint 1 Plugin Platform

Sprint 2 Company Registration

Sprint 3 Parallel Search

Sprint 4 Search Intelligence

Sprint 5 Gemini Summaries

Sprint 6 Google ADK Orchestration

Sprint 7 Registry Intelligence

Sprint 8 Testing & Production Polish

------------------------------------------------------------------------

# Coding Standards

-   Python 3.12+
-   Async wherever possible
-   Type hints everywhere
-   Pydantic models
-   Dependency injection preferred
-   No business logic in plugins beyond connector behavior
-   No ATS logic in orchestrator
-   Small focused modules
-   One responsibility per class

------------------------------------------------------------------------

# Error Handling

Never fail the overall search because of one connector.

Log failures.

Retry.

Repair.

Disable only if necessary.

Continue processing remaining connectors.

------------------------------------------------------------------------

# Definition of Done

A feature is complete when:

-   Unit tested.
-   Typed.
-   Logged.
-   Documented.
-   Configurable.
-   Integrated.
-   No lint errors.
-   No breaking architecture principles.

------------------------------------------------------------------------

# Future Roadmap

v2

-   Notifications
-   Public APIs
-   Additional connectors

v3

-   Multi-user
-   PostgreSQL
-   Vector search
-   Cloud deployment

------------------------------------------------------------------------

# Design Contract

The following principles must never be violated unless an ADR explicitly
supersedes them:

-   One orchestrator agent.
-   Plugin-based architecture.
-   Registry-driven execution.
-   Common job model.
-   Async parallel search.
-   Connector isolation.
-   Incremental implementation by sprint.

This document is the canonical architectural reference for the project.
