# Coding Conventions

**Analysis Date:** 2026-03-17

## Project Type

This is a Docker-based deployment configuration repository for Authentik (https://goauthentik.io/), not a traditional source code project. The repository contains only Dockerfiles that reference the official Authentik container image.

## Conventions Not Applicable

No source code exists in this repository to establish coding conventions. The project:
- Contains no TypeScript, JavaScript, Python, Go, or other programming language source files
- Contains no linting configuration (`.eslintrc`, `.prettierrc`, etc.)
- Contains no import organization patterns
- Contains no function or variable naming conventions
- Contains no error handling patterns beyond Docker container configuration

## Available Files

**Docker Configuration:**
- `/home/nc773/Documents/authentik-railway-2026/Dockerfile`: Main server container configuration
- `/home/nc773/Documents/authentik-railway-2026/worker/Dockerfile`: Worker container configuration

Both Dockerfiles follow the same pattern:
- Base image: `ghcr.io/goauthentik/server:latest`
- Build arguments: Environment-based configuration for Redis, PostgreSQL, and application secrets
- Command: `server` for main container, `worker` for worker container

## Environment-Based Configuration

The Dockerfiles use build arguments and environment variables for configuration:
```dockerfile
ARG AUTHENTIK_REDIS__HOST
ARG AUTHENTIK_POSTGRESQL__HOST
ARG AUTHENTIK_SECRET_KEY
ARG PORT

ENV AUTHENTIK_REDIS__HOST=${AUTHENTIK_REDIS__HOST}
```

This follows the 12-factor app pattern with externalized configuration.

---

*Convention analysis: 2026-03-17*
