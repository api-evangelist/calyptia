---
name: Live-trace a Calyptia pipeline
description: Start a trace session on a running pipeline and read trace records via the Calyptia Cloud API.
api: openapi/calyptia-cloud-openapi-original.yml
operations: [createTraceSession, traceSessions, activeTraceSession, terminateActiveTraceSession]
---

# Live-trace a Calyptia pipeline

Trace sessions capture live records flowing through a pipeline for debugging.
Base URL `https://cloud-api.calyptia.com`; auth `X-Project-Token: <token>`.

## Steps
1. **Start a trace session.** `createTraceSession` —
   `POST /v1/pipelines/{pipelineID}/trace_sessions`. Body sets the sampling
   window / plugins to trace.
2. **Find the active session.** `activeTraceSession` —
   `GET /v1/pipelines/{pipelineID}/trace_session`.
3. **List sessions** for history. `traceSessions` —
   `GET /v1/pipelines/{pipelineID}/trace_sessions` (cursor pagination).
4. **Terminate the active session** when done. `terminateActiveTraceSession` —
   `POST /v1/pipelines/{pipelineID}/trace_session/terminate`.

## Conventions
- Trace sessions are time-bounded; always terminate to stop sampling overhead.
- Errors: `{ "error": "<message>" }`; handle 404 (no active session) explicitly.
- Cursor pagination: `last` + `before`/`after` → `endCursor`.
