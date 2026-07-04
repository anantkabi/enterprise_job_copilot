# Connector_Development_Guide.md

# Connector Development Guide

Version: 1.0

## Purpose

This guide explains how to develop a new connector (plugin) for the AI
Job Intelligence Platform.

A connector is responsible for communicating with one ATS platform or
career site. The Google ADK agent never interacts with ATS
implementations directly---it only communicates through the Connector
Registry.

------------------------------------------------------------------------

# Design Principles

-   One connector = one job source.
-   Connectors are independent plugins.
-   Connectors must not know about other connectors.
-   Connectors never orchestrate searches.
-   Connectors never call Gemini.
-   Connectors never write directly to TinyDB.
-   Connectors return normalized `Job` objects only.

------------------------------------------------------------------------

# Plugin Lifecycle

Startup: 1. Plugin discovery 2. Plugin loading 3. Validation 4. Health
check 5. Registration

Search: 1. Registry selects plugin 2. Plugin executes search 3. Returns
normalized jobs

Failure: 1. Retry 2. Repair 3. Disable if unrecoverable

Recovery: 1. Refresh registry 2. Validate 3. Re-enable

------------------------------------------------------------------------

# Required Interface

Every connector must implement:

``` python
class ConnectorPlugin:
    async def detect(self, url: str) -> bool: ...
    async def validate(self) -> bool: ...
    async def search(self, request): ...
    async def health_check(self): ...
    async def repair(self): ...
```

------------------------------------------------------------------------

# Connector Metadata

Each connector exposes metadata such as:

-   connector_name
-   version
-   supported ATS
-   priority
-   concurrency_limit
-   retry_attempts
-   supported_filters
-   supports_async

Example:

``` python
ConnectorMetadata(
    connector_name="Greenhouse",
    version="1.0.0",
    priority=1,
    concurrency_limit=20,
    retry_attempts=3,
    supports_async=True
)
```

------------------------------------------------------------------------

# Responsibilities

A connector is responsible for:

-   Detecting whether it supports a career page.
-   Querying the ATS or career site.
-   Parsing API or HTML responses.
-   Extracting job postings.
-   Mapping results to the common Job model.
-   Reporting health status.

It is NOT responsible for:

-   Search planning
-   Deduplication
-   Caching
-   AI summarization
-   Persistence
-   Response formatting

------------------------------------------------------------------------

# Common Job Model

Every connector must return the same schema.

Required fields:

-   id
-   company
-   designation
-   location
-   country
-   city
-   remote
-   employment_type
-   posted_date
-   description
-   apply_url
-   source
-   connector

------------------------------------------------------------------------

# Error Handling

Connectors should:

-   Handle transient failures.
-   Respect timeouts.
-   Retry configured operations.
-   Raise meaningful exceptions.
-   Never crash the orchestrator.

------------------------------------------------------------------------

# Health Checks

Every connector implements:

-   validate()
-   health_check()
-   repair()

Health states:

-   Healthy
-   Warning
-   Disabled

------------------------------------------------------------------------

# Testing Checklist

Each connector should pass:

-   ATS detection
-   Validation
-   Health check
-   Successful search
-   Empty search handling
-   Retry behaviour
-   Timeout handling
-   Normalized Job output
-   Duplicate-safe identifiers

------------------------------------------------------------------------

# Folder Structure

plugins/ ├── greenhouse.py ├── lever.py ├── ashby.py ├── workday.py └──
generic.py

------------------------------------------------------------------------

# Development Workflow

1.  Create a new plugin file.
2.  Implement the connector interface.
3.  Add metadata.
4.  Implement parsing.
5.  Write unit tests.
6.  Place the file in `plugins/`.
7.  Start the application.

The Connector Registry should discover the plugin automatically.

------------------------------------------------------------------------

# Best Practices

-   Keep connectors small and focused.
-   Use async HTTP clients.
-   Isolate parsing logic.
-   Log useful diagnostics.
-   Avoid hardcoded values.
-   Prefer configuration over code.
-   Return normalized data only.

------------------------------------------------------------------------

# Definition of Done

A connector is complete when:

-   It is auto-discovered.
-   Health checks pass.
-   Jobs are returned in the common model.
-   Unit tests pass.
-   No changes are required to the orchestrator or registry.
