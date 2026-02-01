# Nextcloud Home Lab – Stable Dockerized Deployment

This repository documents a **production-style Nextcloud deployment** built and operated in a home lab environment.

The objective of this project was to deploy Nextcloud in a **stable, secure, and maintainable** manner, following real-world infrastructure practices rather than a quick demo setup.

---

## Architecture Overview

**Host OS**
- Ubuntu 24.04 LTS (bare metal laptop used as a server)

**Containerized Stack (Docker Compose)**
- Nextcloud (`nextcloud:32-apache`)
- MariaDB (`mariadb:10.6`)
- Redis (`redis:7-alpine`)
- Dedicated Nextcloud cron container

**Networking**
- Secure remote access via **Tailscale**
- Optional Cloudflare Tunnel support
- No direct public internet exposure

---

## Logical Architecture
