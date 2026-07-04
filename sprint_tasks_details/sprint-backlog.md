# Sprint Backlog for AI Job Intelligence Platform

This backlog is derived from the project blueprint and organizes the implementation into incremental sprints.

## Sprint 0 — Foundation

Goals:
- Establish the project skeleton and core architecture.

Tasks:
- Create repository structure for agent, connector_sdk, plugins, registry, services, tools, models, storage, config, tests, and docs.
- Set up Python 3.12+ project configuration and dependency management.
- Define the common job model and base Pydantic schemas.
- Create TinyDB storage layout for companies, jobs, settings, and plugin metadata.
- Set up logging, configuration loading, and environment variables.
- Define coding standards, linting, and test conventions.
- Draft initial architecture documentation and ADR placeholders.

Definition of done:
- Project scaffolding exists.
- Core models and storage structure are in place.
- Basic tests and linting setup are running.

## Sprint 1 — Plugin Platform

Goals:
- Implement the plugin architecture and connector contract.

Tasks:
- Define the connector interface with detect, search, validate, health_check, and repair methods.
- Implement the connector registry and plugin discovery mechanism.
- Create a plugin metadata model and registration flow.
- Build plugin loading and validation logic.
- Add health-check and plugin status tracking.
- Create a stub connector example for local testing.
- Add unit tests for plugin discovery and registration.

Definition of done:
- New connectors can be added without changing the orchestrator.
- Plugin registration and validation work reliably.

## Sprint 2 — Company Registration

Goals:
- Support registering companies and mapping them to source connectors.

Tasks:
- Implement company registration by name.
- Add automatic career page discovery logic.
- Implement ATS detection workflow.
- Add company-to-connector mapping in storage.
- Support registering companies without requiring ATS knowledge from the user.
- Add fallback behavior when discovery or detection fails.
- Add tests for registration and ATS detection flows.

Definition of done:
- A company can be registered and associated with a connector.
- ATS detection and discovery are functional for supported cases.

## Sprint 3 — Parallel Search

Goals:
- Search all registered companies concurrently.

Tasks:
- Implement the search planner and execution orchestration.
- Add async execution for connector searches.
- Handle concurrent connector calls with bounded parallelism.
- Normalize raw search results into the common job schema.
- Aggregate results from all connectors into a single response.
- Add timeout, retry, and error isolation behavior.
- Implement graceful degradation when one connector fails.

Definition of done:
- Searches run in parallel across multiple connectors.
- One connector failure does not block the whole workflow.

## Sprint 4 — Search Intelligence

Goals:
- Improve relevance, deduplication, and incremental search behavior.

Tasks:
- Implement duplicate detection via hashing or canonicalization.
- Add local caching and persistence of jobs.
- Support incremental search so previously seen jobs are not reprocessed unnecessarily.
- Add filtering and ranking logic for search results.
- Improve normalization for company, location, remote, and employment type.
- Add observability around search results and connector performance.

Definition of done:
- Results are deduplicated and persisted.
- Incremental search behavior is stable and test-covered.

## Sprint 5 — Gemini Summaries

Goals:
- Add AI-generated job description summaries.

Tasks:
- Integrate Gemini for concise job summary generation.
- Add summary generation only for supported job records.
- Handle API failures gracefully and avoid blocking search results.
- Store summaries with each job record.
- Add prompt and configuration management.
- Add tests for summary generation fallback behavior.

Definition of done:
- Jobs can be returned with concise AI-generated summaries.
- Summary generation failure is handled safely.

## Sprint 6 — Google ADK Orchestration

Goals:
- Connect the platform to an ADK-based conversational agent experience.

Tasks:
- Build the Google ADK agent entry point.
- Implement tool interfaces for company registration and job search.
- Connect orchestration flow to the plugin registry and search planner.
- Define the conversation flow for registration and search requests.
- Add response formatting for end-user output.
- Add tests for agent-level workflows.

Definition of done:
- A user can interact with the system through the ADK agent.
- The agent can register companies and trigger searches.

## Sprint 7 — Registry Intelligence

Goals:
- Make the registry smarter and more adaptive.

Tasks:
- Add connector scoring and prioritization.
- Track connector health and reliability over time.
- Implement self-healing and repair strategies for connectors.
- Add automatic disable/re-enable logic where appropriate.
- Improve logging and diagnostics for registry decisions.
- Add configuration for retry policies and concurrency limits.

Definition of done:
- The registry can adapt to connector health and performance.
- Self-healing behavior improves reliability over time.

## Sprint 8 — Testing and Production Polish

Goals:
- Harden the platform for reliability and maintainability.

Tasks:
- Add end-to-end tests for core workflows.
- Improve error handling and resilience across the stack.
- Review and optimize performance for production use.
- Add documentation for connector development and operational usage.
- Verify architecture principles are still respected.
- Perform code cleanup, typing review, and dependency review.
- Prepare deployment and operational runbooks for local-first use.

Definition of done:
- The platform is stable, documented, and test-covered.
- It is ready for local validation and future extension.
