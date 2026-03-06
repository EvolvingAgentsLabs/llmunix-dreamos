---
id: seed_3_api-endpoint
version: 1
hierarchy_level: 3
title: REST API Endpoint Creation
trigger_goals: ["create endpoint", "API route", "REST API", "add route", "backend endpoint"]
preconditions: ["web framework configured", "project structure established"]
confidence: 0.5
success_count: 0
failure_count: 0
source_traces: ["seed"]
deprecated: false
---

# REST API Endpoint Creation

## Steps
1. Define the resource and HTTP method (GET/POST/PUT/DELETE)
2. Define the request schema (params, query, body) and response schema
3. Create the route handler file following project conventions
4. Implement input validation at the boundary (validate request body/params)
5. Implement the business logic (or call the service layer)
6. Implement proper error handling with appropriate HTTP status codes
7. Write integration tests covering: happy path, validation errors, not found, auth failures
8. Test manually with curl or a REST client
9. Add to API documentation if the project has it

## Negative Constraints
- Never trust user input without validation — always validate at the boundary
- Never return internal error details (stack traces, SQL errors) to the client
- Never hardcode configuration values (URLs, ports, secrets) in route handlers
- Never skip error handling — unhandled errors crash servers

## Notes
- Follow existing project patterns for consistency (middleware order, error format, etc.)
- Use appropriate status codes: 200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 404 Not Found, 500 Internal Server Error
- Consider rate limiting and authentication requirements before implementation
