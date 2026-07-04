# Sprint 06 — Gemini Summaries

## Goal
Add concise AI-generated summaries to job records.

## Single-response tasks
1. Add a Gemini client module for generating summaries from job descriptions.
2. Create a summarization service and prompt template for concise job summaries.
3. Integrate summary generation into the job processing pipeline for supported jobs.
4. Store generated summaries with the job record in local storage.
5. Add graceful fallback behavior when Gemini is unavailable or returns an error.
6. Add configuration for model selection and summary settings.
7. Add tests for summary generation and fallback behavior.

## Deliverables
- Jobs can be returned with concise AI-generated summaries.
- Summary generation failure is handled safely.

## Definition of Done
- Summary generation is integrated and configurable.
- Fallback behavior is tested and reliable.
- The feature does not block the main search workflow.
