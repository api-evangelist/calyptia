---
name: Deploy a Calyptia telemetry pipeline
description: Create a core instance and deploy a Fluent Bit telemetry pipeline on it via the Calyptia Cloud API.
api: openapi/calyptia-cloud-openapi-original.yml
operations: [createAggregator, createPipeline, pipeline, pipelineConfigHistory]
---

# Deploy a Calyptia telemetry pipeline

Use the Calyptia Cloud API (`https://cloud-api.calyptia.com`) to stand up a core
instance and run a pipeline on it.

## Auth
Send a project API token on every request:

```
X-Project-Token: <token>
```

Get the token from the Calyptia Cloud UI (core.calyptia.com) → project Settings →
Generate API key.

## Steps
1. **Create a core instance (aggregator).** `createAggregator` —
   `POST /v1/projects/{projectID}/aggregators`. The core instance is where
   pipelines run.
2. **Create a pipeline on it.** `createPipeline` —
   `POST /v1/aggregators/{aggregatorID}/pipelines`. Supply the Fluent Bit config
   in the request body.
3. **Read it back.** `pipeline` — `GET /v1/aggregator_pipelines/{pipelineID}`.
4. **Inspect config revisions.** `pipelineConfigHistory` —
   `GET /v1/aggregator_pipelines/{pipelineID}/config_history`.

## Conventions
- List endpoints use cursor pagination: `last` (page size) + `before`/`after`
  cursors; responses carry `endCursor`.
- Errors return `{ "error": "<message>" }` (not RFC 9457). Handle 401/403/404/409.
- Writes are **not** idempotent by key — do not blindly retry a create; re-list
  to reconcile.
