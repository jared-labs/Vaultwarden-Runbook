# Vaultwarden — Self-Hosted Password Manager

> Runbook for deploying and managing Vaultwarden, a lightweight Bitwarden-compatible server for self-hosted credential management.

⚠️ **This runbook is a work-in-progress.** Sections marked TODO are not yet documented.

---

## At a Glance

| Field | Value |
|-------|-------|
| VM/CT Name | `TODO` |
| Host Node | TODO |
| IP Address | `TODO` |
| OS | TODO |
| vCPU / RAM / Disk | TODO |
| Key Ports | `443` (HTTPS), `80` (HTTP) |
| Credentials | Bitwarden: "Vaultwarden Admin" |
| Depends On | Proxmox, reverse proxy (TODO) |
| Upstream Project | [Vaultwarden](https://github.com/dani-garcia/vaultwarden) |
| Compatibility | Bitwarden clients (web, desktop, mobile, CLI) |

---

## Prerequisites

<!-- TODO: fill in once deployment target is decided -->

- [ ] Proxmox node with available resources
- [ ] Network: static IP reserved
- [ ] Reverse proxy configured (Caddy / Nginx / Traefik)
- [ ] DNS record pointing to instance
- [ ] Upstream dependencies running

---

## 0 — Create the VM

<!-- TODO -->

---

## 1 — Base OS Setup

<!-- TODO -->

---

## 2 — Install Vaultwarden

<!-- TODO: Docker-based or native install steps -->

---

## 3 — Configure Vaultwarden

<!-- TODO: environment variables, admin panel, SMTP, etc. -->

---

## 4 — Networking & Firewall

<!-- TODO: ports, reverse proxy config, TLS -->

---

## 5 — Validation

<!-- TODO: health checks, client connectivity -->

---

## 6 — Post-Install

<!-- TODO: hardening, backups, monitoring -->

---

## Troubleshooting

<!-- TODO -->

| Symptom | Cause | Fix |
|---------|-------|-----|
| — | — | — |

---

## Quick Reference

```bash
# TODO: add common commands once deployment method is decided
```

---

## Quirks & Gotchas

<!-- TODO -->

---

*Last updated: 2025-01-XX — WIP*
