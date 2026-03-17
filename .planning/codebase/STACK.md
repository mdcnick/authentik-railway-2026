# Technology Stack

**Analysis Date:** 2026-03-17

## Languages

**Primary:**
- Python - Authentik server base language (via container image)
- Dockerfile - Container orchestration

**Secondary:**
- Bash - Container entrypoint scripts

## Runtime

**Environment:**
- Docker containers - Production deployment via containerization
- Container images: `ghcr.io/goauthentik/server:latest`

**Package Manager:**
- Python pip - Managed within official Authentik image (not directly exposed)
- Lockfile: Not present in repository

## Frameworks

**Core:**
- Authentik Server - Identity and access management platform
  - Base image: `ghcr.io/goauthentik/server:latest`
  - Deployment modes: Server and Worker containers

**Infrastructure:**
- Docker - Containerization platform
- Docker Compose patterns - Implied multi-container setup

## Key Dependencies

**Critical:**
- Authentik Server Image (ghcr.io/goauthentik/server:latest) - Full IAM platform including authentication, authorization, and policy engine
- PostgreSQL - Database backend (configured via environment variables)
- Redis - Caching and session management (configured via environment variables)

**Infrastructure:**
- Railway Platform - Hosting/deployment platform (implied from repository context)
- Container Registry: GitHub Container Registry (ghcr.io)

## Configuration

**Environment:**
- Configuration via Docker build arguments and environment variables
- Build-time arguments: `AUTHENTIK_REDIS__*`, `AUTHENTIK_POSTGRESQL__*`, `AUTHENTIK_SECRET_KEY`, `PORT`
- Runtime environment: All arguments passed as `ENV` variables to containers

**Key Configuration Variables:**
- `AUTHENTIK_REDIS__HOST` - Redis connection host
- `AUTHENTIK_REDIS__PORT` - Redis connection port
- `AUTHENTIK_REDIS__PASSWORD` - Redis authentication
- `AUTHENTIK_POSTGRESQL__HOST` - PostgreSQL connection host
- `AUTHENTIK_POSTGRESQL__PORT` - PostgreSQL connection port
- `AUTHENTIK_POSTGRESQL__USER` - PostgreSQL authentication user
- `AUTHENTIK_POSTGRESQL__NAME` - PostgreSQL database name
- `AUTHENTIK_POSTGRESQL__PASSWORD` - PostgreSQL authentication password
- `AUTHENTIK_SECRET_KEY` - Application secret key for security
- `PORT` - Server listen port

**Build:**
- Dockerfiles: `Dockerfile`, `worker/Dockerfile`
- Both use official Authentik image as base

## Platform Requirements

**Development:**
- Docker installed and running
- Understanding of Authentik configuration patterns
- Access to PostgreSQL and Redis services

**Production:**
- Container orchestration platform (Railway, Kubernetes, Docker Swarm, etc.)
- PostgreSQL database instance
- Redis instance
- Network connectivity between containers
- Environment variable injection capability

---

*Stack analysis: 2026-03-17*
