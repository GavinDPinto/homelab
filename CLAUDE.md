# CLAUDE.md
 
Context for Claude Code working in this repo.
 
## What this is
Gavin's first homelab. This repo documents how it's built and will later hold the infra code that runs it. Goal: rebuild everything from this repo if the SSD dies. The Proxmox install is manual; everything after it gets documented, then automated.
 
## Hardware
- Lenovo ThinkCentre M710q Tiny, refurbished (Good), bought Nov 2026 for $219.78 CAD from Best Buy Canada (marketplace seller "AT Computer"); owner keeps order info privately for returns/warranty
- i5-7400T (4C/4T, 35W, HD 630 iGPU with Quick Sync incl. HEVC 10-bit)
- 16GB DDR4 (max 32GB), 256GB SSD, spare 2.5" bay (needs Lenovo SATA cable + tray)
- 1GbE Intel NIC; video out is 3× DisplayPort only (checked: no HDMI port)
- Power: Lenovo 65W slim-tip adapter (included, works)
- Owner's laptop runs Windows (use PowerShell for local commands)
## Decisions (see docs/decisions/)
- **Proxmox VE** on bare metal (LXC + VMs, snapshots)
- **Tailscale** for all private remote access (Proxmox UI, SSH, later Jellyfin/photos); no port forwarding
- **Cloudflare Tunnel** (planned) only for the public personal website, on the owner's own domain (don't put the domain in the repo unless the owner does)
- **DHCP on the host bridge (vmbr0)**: the server moves between off-campus student housing (wall Ethernet from the building's own network, **not** the university network/Wi-Fi) and home, so a static IP would break networking and Tailscale on every move
- Containers use DHCP too, or are reached only via Tailscale
- No media storage yet; Jellyfin is planned for later with an external USB HDD
## Status
- Done (2026-11-26): Proxmox VE 9.3-1 installed (ext4 on /dev/nvme0n1, hostname m710q.home.arpa, interface nic0) → no-subscription repo + updates → Tailscale on host (MagicDNS on, key expiry off; reach at https://m710q:8006) → vmbr0 switched to DHCP (backup at /etc/network/interfaces.static-backup). Docs: docs/02–04.
- To verify after first move: gets an address on the other network, Tailscale works, stale /etc/hosts line causes no issues
- Housing network: router is owned and controlled by the owner's family, so no third-party network rules apply
- Tailscale on laptop + phone. SSH over Tailscale works; laptop SSH config has `hl-*` aliases (`ssh hl-root`), Windows and WSL share one ed25519 key
- Next: first container
## Later purchases (not now)
Note: RAM/SSD/HDD prices are elevated in 2026 due to the memory shortage.
- Media drive for Jellyfin: Seagate/WD/Toshiba external **desktop** USB HDD from Amazon itself or a major store; 8TB was ~$355–395 CAD in Nov 2026. Never buy no-name "8TB portable SSD" listings (fake-capacity scam).
- Second internal drive: 2.5" SATA SSD (~$55, budget brands like Patriot). Needs a Lenovo-specific SATA cable + tray, often not included.
- RAM: up to 32GB DDR4 SO-DIMM
- Small UPS (battery backup), eventually
## Repo layout
```
README.md             overview, hardware summary, status, doc index
docs/01-hardware.md   hardware (owner-written)
docs/02-04-*.md       proxmox install, networking (DHCP), tailscale
docs/decisions/       ADR-style notes, numbered NNNN-title.md (template: 0000)
.gitignore         blocks secrets, ISOs/images, tfstate, OS junk
```
Do **not** add ansible/, terraform/ or services/ folders until asked. Planned later order: Ansible (host config) → OpenTofu/Terraform with bpg/proxmox provider → docker-compose per service.
 
## Rules
- Never commit secrets (passwords, Tailscale auth keys, API tokens, private keys). Use a gitignored `.env`.
- Repo is public: no MAC addresses, public IPs, housing-network details, or the owner's city/address (time zone America/Toronto is fine).
- **Docs come from the owner.** Claude writes docs only from what the owner tells it: interview them (a few questions at a time), keep their wording and reasons, and fill gaps by asking, not guessing. Facts the owner didn't give (versions, interface names, results, dates) stay as `____` or TODO. Show what was written so they can correct it.
- Keep docs practical: exact commands, choices made, problems hit and their fixes.
- Small commits, conventional style (`docs: ...`, `feat: ...`).
- Owner is a beginner at homelabbing: explain non-obvious steps briefly.
 