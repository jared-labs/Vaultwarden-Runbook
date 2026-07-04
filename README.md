# Self-Hosted Password Management: Vaultwarden

## Overview

This document summarizes a Vaultwarden deployment used as the central credential store for a home lab. It is written for a portfolio audience, focusing on architecture, security posture, and the role self-hosted credential management plays in lab operations.

Vaultwarden is a lightweight, Rust-based implementation of the Bitwarden server API. It provides full Bitwarden client compatibility (browser extensions, mobile apps, CLI) with significantly lower resource requirements than the official Bitwarden server stack.

## Environment

| Field | Value |
|-------|-------|
| System | Dedicated VM |
| IP Address | (internal) |
| OS | Ubuntu/Debian |
| Runtime | Docker (single container) |
| Key Ports | 443 (HTTPS), 80 (HTTP redirect) |
| Clients | Bitwarden browser extension, mobile apps, CLI |
| Backup | Encrypted database export + VM snapshot |

## Architecture

```text
┌─────────────────────────────────────────────────────────┐
│  Vaultwarden VM                                          │
│                                                          │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Docker: vaultwarden/server                       │   │
│  │  ├── Rocket web server (Rust)                     │   │
│  │  ├── SQLite database (/data/db.sqlite3)           │   │
│  │  └── Attachments + sends (/data/)                 │   │
│  └──────────────────────────────────────────────────┘   │
│                       │                                  │
│                       │ HTTPS (:443)                     │
└───────────────────────┼──────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────┐
│  Clients                                                 │
│  • Bitwarden browser extensions                          │
│  • Mobile apps (iOS/Android)                             │
│  • Bitwarden CLI (bw)                                    │
│  • MCP integration (Kiro agent access)                   │
└─────────────────────────────────────────────────────────┘
```

## Role in the Lab

Vaultwarden is the source of truth for all lab credentials:

- Service account passwords (OpenVAS `vas_scan`, SSH accounts)
- API keys (Omada, TrueNAS, runZero, N8N)
- Certificate passwords (TAK Server `.p12` bundles)
- Web UI credentials (Graylog admin, Cribl, Proxmox, Semaphore)
- Infrastructure secrets (database passwords, SMTP credentials)

The Bitwarden CLI (`bw`) and MCP integration allow automation tools to retrieve credentials without hardcoding secrets in scripts or documentation.

## Design Decisions

- **Vaultwarden over official Bitwarden:** The official server requires MSSQL and significantly more RAM. Vaultwarden runs in a single container with SQLite, suitable for a single-user/small-team lab.
- **Docker deployment:** Single container with a persistent `/data` volume. Simple to backup (snapshot the volume), upgrade (pull new image), and restore.
- **Self-hosted over cloud Bitwarden:** Lab credentials include internal IPs, service accounts, and API keys that should not leave the local network. Self-hosting keeps the trust boundary local.
- **HTTPS required:** Even on a private LAN, credentials in transit should be encrypted. Self-signed cert or Let's Encrypt via reverse proxy.

## Quirks and Gotchas

- Admin panel is disabled by default — enable via `ADMIN_TOKEN` environment variable for initial setup, then disable in production.
- SQLite database should be backed up with proper locking (not just copying the file while running). Use the admin panel's backup feature or stop the container first.
- Bitwarden clients cache the server URL. If the IP changes, clients need manual reconfiguration.
- WebSocket notifications (live sync) require additional reverse proxy configuration if behind Nginx/Caddy.

## What This Demonstrates

Self-hosted credential management is foundational infrastructure. This deployment shows practical secrets management: centralized storage, client compatibility, backup strategy, and integration with automation tooling — the practices that prevent credential sprawl across scripts, docs, and sticky notes.

---

Sanitized for public portfolio use.

For step-by-step deployment instructions, see [OPERATIONS.md](./OPERATIONS.md).
