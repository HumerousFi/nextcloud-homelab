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
- Secure administrative access via **Tailscale**
- Public HTTPS access via **Cloudflare Tunnel** (no open inbound ports)
- No direct public internet exposure

---

## Logical Architecture
```pgsql
         Client (Home)
              |
         Tailscale VPN
              |
         Server (Hostel)
              |
         Docker Compose
              |
+---------------------------+
| Nextcloud (Apache + PHP) |
|  - Redis file locking    |
|  - APCu local cache      |
|  - Cron container        |
+-------------+-------------+
              |
         MariaDB 10.6
```
---

## Key Stability Decisions

| Decision | Reason |
|--------|-------|
| Version-pinned Docker images | Prevents breaking updates |
| Dedicated cron container | Reliable background jobs |
| Redis file locking | Prevents file corruption |
| Volume-based storage | Safe container recreation |
| Tailscale access | Secure remote access without port forwarding |
| Manual backups | Disaster recovery readiness |

---

## Security Measures

- Exposed securely via Cloudflare Tunnel with a custom domain and HTTPS, without any open inbound ports (zero-trust access model)
- Explicit `trusted_domains` configuration
- No public IP exposure
- Access restricted via Tailscale private network
- Redis used for transactional file locking
- Database isolated in its own container

---

## Backup Strategy

Periodic backups are taken at the Docker volume level, covering:
- Nextcloud data
- MariaDB database

Example backup command:

```bash
docker run --rm \
  -v nextcloud:/data \
  -v db:/db \
  alpine tar czf nextcloud_backup.tar.gz /data /db
```

---

## Skills Demonstrated

- Docker & Docker Compose
- Linux system administration
- Stateful service management
- Secure remote access (Tailscale)
- Database-backed web applications
- Caching and file-locking strategies (Redis, APCu)
- Backup and recovery planning
- Real-world infrastructure troubleshooting

---

## Why This Project Matters

This project mirrors how self-hosted collaboration platforms are deployed in
small organizations and internal infrastructure environments.

The focus is on **long-term stability, security, and operational correctness**,
not just making the application run.
