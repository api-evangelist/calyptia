---
name: Manage a Calyptia agent fleet
description: Create a fleet and register/list Fluent Bit agents against it via the Calyptia Cloud API.
api: openapi/calyptia-cloud-openapi-original.yml
operations: [createFleet, fleets, registerAgent, agents, updateAgent]
---

# Manage a Calyptia agent fleet

A fleet is a managed group of Fluent Bit agents. Base URL
`https://cloud-api.calyptia.com`; authenticate with `X-Project-Token: <token>`.

## Steps
1. **Create a fleet.** `createFleet` — `POST /v1/projects/{projectID}/fleets`.
2. **List fleets.** `fleets` — `GET /v1/projects/{projectID}/fleets` (cursor
   pagination via `last` + `before`/`after`).
3. **Register an agent** into the fleet. `registerAgent` —
   `POST /v1/projects/{projectID}/agents`; the body carries `fleetID`,
   `machineID`, and `environmentID`.
4. **List agents.** `agents` — `GET /v1/projects/{projectID}/agents`.
5. **Update an agent.** `updateAgent` — `PATCH /v1/agents/{agentID}`.

## Conventions
- Filter list results by `name` where supported.
- Errors: `{ "error": "<message>" }`. A 409 conflict on membership deletes means
  delete the project instead of the last creator.
- No idempotency keys — reconcile by re-listing rather than retrying writes.
