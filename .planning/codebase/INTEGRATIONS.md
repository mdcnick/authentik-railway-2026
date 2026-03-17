# External Integrations

**Analysis Date:** 2026-03-17

## APIs & External Services

**Identity & Authentication:**
- Authentik Server - Comprehensive identity and access management platform
  - Base image: `ghcr.io/goauthentik/server:latest`
  - Provides OIDC, SAML, LDAP, and OAuth2 authentication
  - Supports multiple protocol integrations

**OAuth2 & OIDC Clients:**
- Configurable through Authentik admin interface (not defined in code)
- Support for custom applications and protocol flows

## Data Storage

**Databases:**
- PostgreSQL
  - Connection configured via environment variables: `AUTHENTIK_POSTGRESQL__HOST`, `AUTHENTIK_POSTGRESQL__PORT`, `AUTHENTIK_POSTGRESQL__USER`, `AUTHENTIK_POSTGRESQL__NAME`, `AUTHENTIK_POSTGRESQL__PASSWORD`
  - Purpose: Primary data store for authentication, users, policies, applications, and configuration
  - Client: PostgreSQL driver (built into Authentik server)

**Caching:**
- Redis
  - Connection configured via environment variables: `AUTHENTIK_REDIS__HOST`, `AUTHENTIK_REDIS__PORT`, `AUTHENTIK_REDIS__PASSWORD`
  - Purpose: Session management, token caching, distributed cache for multi-instance deployments

**File Storage:**
- Local filesystem only - No external file storage integrations configured

## Authentication & Identity

**Auth Provider:**
- Authentik
  - Implementation: Self-hosted IAM platform
  - Protocols: OIDC (OpenID Connect), SAML 2.0, LDAP, OAuth2
  - Primary use: Central authentication and authorization for applications using this Authentik instance

**Identity Sources:**
- Configurable via Authentik UI (not defined in Docker configuration)
- Support for custom directory services, LDAP, SAML providers, OAuth2 providers

## Monitoring & Observability

**Error Tracking:**
- Not detected in configuration

**Logs:**
- Container logs via Docker/platform stdout stderr
- Authentik built-in logging to PostgreSQL database

**Metrics:**
- Authentik exposes Prometheus-compatible metrics (standard feature)
- Collection depends on platform monitoring setup

## CI/CD & Deployment

**Hosting:**
- Railway Platform - Cloud deployment platform (inferred from repository name/context)

**Container Registry:**
- GitHub Container Registry (ghcr.io) - Source of official Authentik images

**CI Pipeline:**
- Not detected in repository - Likely handled by Railway platform

**Deployment Pattern:**
- Multi-container deployment: Server and Worker containers
- Server container: `Dockerfile` - Handles HTTP requests and user-facing features
- Worker container: `worker/Dockerfile` - Handles background jobs and asynchronous tasks

## Environment Configuration

**Required env vars:**
```
# PostgreSQL
AUTHENTIK_POSTGRESQL__HOST        # Database hostname
AUTHENTIK_POSTGRESQL__PORT        # Database port
AUTHENTIK_POSTGRESQL__USER        # Database user
AUTHENTIK_POSTGRESQL__NAME        # Database name
AUTHENTIK_POSTGRESQL__PASSWORD    # Database password

# Redis
AUTHENTIK_REDIS__HOST             # Redis hostname
AUTHENTIK_REDIS__PORT             # Redis port
AUTHENTIK_REDIS__PASSWORD         # Redis password

# Application
AUTHENTIK_SECRET_KEY              # Secret key for cryptographic operations
PORT                              # HTTP server port
```

**Secrets location:**
- Environment variables injected at container runtime (Railway, Docker Compose, Kubernetes, etc.)
- No `.env` files in repository - configuration is external

## Webhooks & Callbacks

**Incoming:**
- OAuth2/OIDC token endpoints - Available at runtime via Authentik service
- SAML assertion consumer service (ACS) endpoints - Available at runtime
- LDAP endpoints - Configurable port binding

**Outgoing:**
- Event hooks - Authentik can send webhooks to external HTTP endpoints for policy execution and notifications (configured via admin UI, not in Docker config)
- No webhook configurations detected in this repository

## Image Registry

**Primary:**
- GitHub Container Registry (ghcr.io/goauthentik/server)
  - Official Authentik server images
  - Latest tag used (no version pinning in current configuration)

---

*Integration audit: 2026-03-17*
