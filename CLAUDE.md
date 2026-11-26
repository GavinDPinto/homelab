# CLAUDE.md
 
Context for Claude Code working in this repo.
 
## What this is
Gavin's first homelab. This repo documents how it's built and will later hold the infra code that runs it. Goal: rebuild everything from this repo if the SSD dies. The Proxmox install is manual; everything after it gets documented, then automated.
 
## Hardware
- Lenovo ThinkCentre M710q Tiny, refurbished, bought Nov 2026 for $219.78 CAD
- i5-7400T (4C/4T, 35W, HD 630 iGPU with Quick Sync incl. HEVC 10-bit)
- 16GB DDR4 (max 32GB), 256GB SSD, spare 2.5" bay (needs Lenovo SATA cable + tray)
- 1GbE Intel NIC; video out is DisplayPort (no HDMI by default)
- Owner's laptop runs Windows (use PowerShell for local commands)
## Decisions (see docs/decisions/)
- **Proxmox VE** on bare metal (LXC + VMs, snapshots)
- **Tailscale** for all remote access; no port forwarding
- **DHCP on the host bridge (vmbr0)**: the server moves between off-campus student housing (wall Ethernet from the building's own network, **not** the university network/Wi-Fi) and home, so a static IP would break networking and Tailscale on every move
- Containers use DHCP too, or are reached only via Tailscale
- No media storage yet; Jellyfin is planned for later with an external USB HDD
## Status
- Have: server, Cat5e cable, USB stick
- Waiting on: DisplayPort-to-HDMI cable (needed only for the first install)
- Next: flash Proxmox ISO (Rufus, DD mode) → BIOS (F1: VT-x, VT-d on; F12 boot menu) → install → no-subscription repo + updates → switch vmbr0 to DHCP → install Tailscale on host (disable key expiry, enable MagicDNS)
## Repo layout
```
README.md             overview, hardware summary, status, doc index
docs/01-hardware.md   hardware (owner-written)
docs/decisions/       ADR-style notes, numbered NNNN-title.md (template: 0000)
.gitignore         blocks secrets, ISOs/images, tfstate, OS junk
```
Do **not** add ansible/, terraform/ or services/ folders until asked. Planned later order: Ansible (host config) → OpenTofu/Terraform with bpg/proxmox provider → docker-compose per service.
 
## Rules
- Never commit secrets (passwords, Tailscale auth keys, API tokens, private keys). Use a gitignored `.env`.
- Repo is public: no MAC addresses, public IPs or housing-network details.
- **The owner writes the docs in their own words.** Don't write or fill in doc content (READMEs, guides, decision notes) unless asked; help with research, explanations, review and commands instead. Never invent values (versions, interface names, results).
- Keep docs practical: exact commands, choices made, problems hit and their fixes.
- Small commits, conventional style (`docs: ...`, `feat: ...`).
- Owner is a beginner at homelabbing: explain non-obvious steps briefly.
 