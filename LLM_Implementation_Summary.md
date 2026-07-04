# LLM_Implementation_Summary.md

# AI Job Intelligence Platform - Implementation Summary

## Purpose

This document is intended to be uploaded into any LLM (ChatGPT, Gemini,
Claude, Cursor, Copilot, etc.) so it can immediately understand the
project architecture, constraints, and implementation strategy.

------------------------------------------------------------------------

# Objective

Build a production-grade **AI Job Intelligence Platform** using:

-   Google ADK
-   FastMCP
-   TinyDB
-   Gemini
-   Python 3.12+

The platform discovers, registers, and searches job opportunities across
multiple ATS platforms and company career sites in parallel using a
plugin-based architecture.

------------------------------------------------------------------------

# Core Architecture

Exactly **one** Google ADK orchestrator agent.

    User
        ↓
    Google ADK Agent
        ↓
    Search Planner
        ↓
    Connector Registry
        ↓
    Connector Health Manager
        ↓
    MCP Connector Plugins
        ↓
    Async Search Engine
        ↓
    Job Normalizer
        ↓
    Duplicate Detector
        ↓
    TinyDB
        ↓
    Gemini JD Summarizer
        ↓
    Response Formatter
        ↓
    User

------------------------------------------------------------------------

# Design Principles

1.  Only one Google ADK agent.
2.  No ATS-specific logic inside the agent.
3.  Every job source is implemented as a plugin.
4.  Plugins are discovered automatically.
5.  Connector Registry is the single source of truth.
6.  All connector searches run asynchronously.
7.  TinyDB provides local persistence.
8.  Gemini is used only for job-description summaries.
9.  All connectors implement the same interface.
10. New job sources require only a new plugin.

------------------------------------------------------------------------

# Primary User Features

-   Register a company by name.
-   Register multiple companies at once.
-   Automatically discover the official careers page.
-   Automatically detect the ATS.
-   Search all registered companies concurrently.
-   Return concise job results containing:
    -   Company
    -   Designation
    -   Location
    -   Posted Date
    -   2--3 line AI summary
    -   Apply URL

------------------------------------------------------------------------

# Connector Contract

Every connector plugin must implement:

-   detect()
-   search()
-   validate()
-   health_check()
-   repair()

The orchestrator must never call ATS-specific logic directly.

------------------------------------------------------------------------

# Initial Connectors

-   Greenhouse
-   Lever
-   Ashby
-   Workday
-   SmartRecruiters
-   Generic HTML / JSON-LD parser

Future connectors can include LinkedIn, Naukri, Wellfound, Google Jobs,
RemoteOK, etc.

------------------------------------------------------------------------

# Storage

TinyDB collections:

-   companies.json
-   jobs.json
-   settings.json
-   plugin_metadata.json

------------------------------------------------------------------------

# Search Workflow

User Query → Parse intent → Load registered companies → Group by
connector → Execute searches concurrently → Normalize jobs → Remove
duplicates → Cache results → Summarize descriptions → Return formatted
response

------------------------------------------------------------------------

# Sprint Roadmap

Sprint 0 - Project foundation

Sprint 1 - Plugin ecosystem

Sprint 2 - Company registration

Sprint 3 - Parallel search

Sprint 4 - Search intelligence

Sprint 5 - Gemini summarization

Sprint 6 - Google ADK orchestration

Sprint 7 - Registry intelligence

Sprint 8 - Testing, documentation, and production polish

------------------------------------------------------------------------

# Mandatory Architectural Rules

-   One orchestrator agent only.
-   Plugin-based connector architecture.
-   Registry-driven execution.
-   Async parallel processing.
-   Common Job model for every connector.
-   Self-testing and self-healing connectors.
-   Graceful degradation when a connector fails.
-   Incremental implementation sprint by sprint.

------------------------------------------------------------------------

# Expected Code Quality

-   Python 3.12+
-   Pydantic models
-   Type hints
-   Async-first
-   Modular design
-   Unit tests
-   Structured logging
-   Configuration-driven
-   Production-quality code

------------------------------------------------------------------------

# Success Criteria

The platform should allow developers to add a new job source simply by
creating a new connector plugin, without modifying the Google ADK agent
or the orchestration logic.
