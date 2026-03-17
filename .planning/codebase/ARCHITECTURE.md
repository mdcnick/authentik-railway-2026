# Architecture

**Analysis Date:** 2026-03-17

## Pattern Overview

**Overall:** Containerized Service Deployment Pattern

**Key Characteristics:**
- Multi-container Docker-based architecture
- Separation of concerns: server and worker processes
- Environment-driven configuration via build arguments
- Stateless service design with external dependency coordination

## Layers

**Application Layer:**
- Purpose: Authentik server and worker services
- Location: `Dockerfile` (server), `worker/Dockerfile` (worker)
- Contains: Docker image configuration and service entry points
- Depends on: External base image from `ghcr.io/goauthentik/server:latest`
- Used by: Runtime Docker container orchestration

**Infrastructure Layer:**
- Purpose: External service integration and configuration
- Dependencies: PostgreSQL database, Redis cache, application secrets
- Accessed via: Environment variables injected at container build/runtime

**Configuration Layer:**
- Purpose: Runtime behavior and service connectivity
- Location: Build arguments passed to Dockerfile
- Contains: Database credentials, Redis connection details, secret keys, port configuration

## Data Flow

**Service Initialization:**

1. Docker container starts with Authentik base image
2. Build arguments injected as environment variables (AUTHENTIK_* prefixed)
3. Service reads environment variables for:
   - Database connection (PostgreSQL host, port, credentials, database name)
   - Cache layer (Redis host, port, password)
   - Application secret key
   - Service port binding
4. Service starts with appropriate command (`server` or `worker`)
5. Service connects to PostgreSQL for persistent state
6. Service uses Redis for caching and session management

**Request Flow (Server):**

1. Incoming requests hit server container on configured PORT
2. Server authenticates requests using stored credentials
3. Server queries PostgreSQL for user and policy data
4. Server uses Redis for session caching
5. Server returns authentication response

**Background Job Flow (Worker):**

1. Worker process polls for background tasks
2. Worker reads task queue from Redis
3. Worker executes tasks (e.g., user provisioning, cleanup)
4. Worker updates state in PostgreSQL
5. Worker acknowledges task completion

## Key Abstractions

**Server Process:**
- Purpose: HTTP service accepting authentication requests
- Entry point: `CMD ["server"]` in `Dockerfile`
- Pattern: Standard Authentik server listening on configurable PORT

**Worker Process:**
- Purpose: Background task processing and async operations
- Entry point: `CMD ["worker"]` in `worker/Dockerfile`
- Pattern: Long-running process consuming task queue from Redis

**Configuration Container:**
- Purpose: Centralized environment injection for multi-environment deployments
- Pattern: Build-time arguments (ARG) converted to runtime environment variables (ENV)
- Benefit: Same image artifact deployable across development, staging, production

## Entry Points

**Server Service:**
- Location: `Dockerfile` line 25
- Triggers: Container startup/restart
- Responsibilities:
  - Bind to configured PORT
  - Accept incoming authentication requests
  - Manage user sessions
  - Serve administrative UI

**Worker Service:**
- Location: `worker/Dockerfile` line 25
- Triggers: Container startup/restart
- Responsibilities:
  - Process background tasks
  - Handle user provisioning
  - Execute scheduled jobs
  - Maintain system health via cleanup tasks

## Error Handling

**Strategy:** Container-level resilience with orchestrator supervision

**Patterns:**
- Service crashes trigger container restart (handled by orchestrator)
- Connection failures to PostgreSQL or Redis propagate as runtime errors
- Failed authentication requests return HTTP error responses
- Background job failures retry via task queue (Redis/Celery mechanism)

## Cross-Cutting Concerns

**Configuration Management:** Environment variable substitution at build/runtime. All services access credentials and connection strings via `AUTHENTIK_*` environment variables.

**Logging:** Standard output and error streams captured by container runtime. No explicit logging configuration in these Dockerfiles.

**Service Discovery:** Hard-coded hostnames injected via environment variables. All services connect using provided hostnames for PostgreSQL and Redis.

**Authentication:** Delegated to Authentik application logic. Build arguments provide secret key and database credentials for encrypted authentication state storage.

---

*Architecture analysis: 2026-03-17*
