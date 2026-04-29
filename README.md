# Ansible Automation Portfolio
A collection of Ansible playbooks and a small **FastAPI web UI** for running them. Built and used in production to automate Windows and Linux fleet management at a media-tech company; this repo is a sanitized showcase of the patterns and tooling.
## What this demonstrates
- **Multi-OS automation** — Linux (apt, Docker) and Windows (WinRM/NTLM, MSI installs, SMB shares)
- **Idempotent software deployment** — installer copy → unblock → silent install → cleanup, with retry/until
- **Self-service web UI** — FastAPI app to list playbooks, set `--limit`, toggle `--check`, view live logs and run history
- **Containerized control node** — `Dockerfile` + `docker-compose.yml` with optional Nginx TLS reverse proxy
- **12-factor secrets** — credentials supplied via environment (`WINRM_USER`, `WINRM_PASSWORD`) or Ansible Vault, never committed
- **Authentication** — basic auth + signed session cookies on the UI
- **Reverse-proxy ready** — supports `PUBLIC_BASE_URL` subpath deployment
## Tech stack
`Ansible` · `Python 3.10+` · `FastAPI` · `Uvicorn` · `Docker` · `Docker Compose` · `Nginx` · `WinRM (pywinrm)` · `systemd`
## Repository tour
| Path | What it shows |
|---|---|
| `playbooks/docker-setup.yml` | Installs Docker Engine + Portainer on Ubuntu hosts from `[servers]` |
| `playbooks/windows-msi-install.yml` | Pulls an MSI from an SMB share, unblocks it, installs silently, drops a license file, retries on transient WinRM failures |
| `playbooks/after-effects-install.yml` | Cross-platform installer skeleton (Windows + macOS) |
| `playbooks/test.yml` | Minimal connectivity smoke test |
| `group_vars/windows_servers.yml` | WinRM/NTLM defaults pulling creds from env |
| `contrib/` | Example systemd unit and Nginx reverse-proxy snippet for the web UI |
| `web/` | FastAPI app that wraps `ansible-playbook` (see `web/README.md`) |
