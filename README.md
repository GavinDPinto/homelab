# homelab

My first homelab: notes on how it's built, and later the code that runs it.

> **Goal:** if the SSD dies, I can rebuild everything from this repo.

## Hardware

| Part | Details |
|---|---|
| Machine | Lenovo ThinkCentre M710q Tiny (refurbished) |
| CPU | Intel Core i5-7400T (4C/4T, 35W, HD Graphics 630) |
| RAM | 16GB DDR4 (max 32GB) |
| Storage | 256GB NVMe SSD (WD PC SN730) |
| Network | 1GbE onboard Intel NIC |
| Hypervisor | Proxmox VE 9.3 |
| Remote access | Tailscale (`https://m710q:8006`) |

Full details: [docs/01-hardware.md](docs/01-hardware.md)

## Status

- [x] Server bought and arrived
- [x] Ethernet cable, USB stick, USB keyboard and mouse
- [X] DisplayPort → HDMI dongle (have a HDMI to HDMI cable already)
- [x] Install Proxmox VE
- [x] Switch host networking to DHCP (portable between housing and home)
- [x] Set up remote access (Tailscale)
- [ ] Test on the other network after the first move
- [ ] First container

## Docs

| Doc | What it covers |
|---|---|
| [Hardware](docs/01-hardware.md) | What I bought, specs, other gear, upgrade ideas |
| [Proxmox install](docs/02-proxmox-install.md) | Flashing the USB, BIOS, installer choices, updates |
| [Networking](docs/03-networking.md) | Switching the host to DHCP so it works on any network |
| [Tailscale](docs/04-tailscale.md) | Remote access from anywhere |
| [Decisions](docs/decisions/) | Short notes on *why* I chose things ([template](docs/decisions/0000-template.md)) |

## Repo layout

```
homelab/
├── README.md
├── docs/
│   ├── 01-hardware.md
│   ├── 02-proxmox-install.md
│   ├── 03-networking.md
│   ├── 04-tailscale.md
│   └── decisions/
│       ├── 0000-template.md
│       ├── 0001-used-mini-pc.md
│       ├── 0002-proxmox.md
│       └── 0003-tailscale.md
└── .gitignore
```

## Rules for this repo

- **No secrets.** Passwords, auth keys, API tokens and private keys stay out of git.
- **No identifying network info** (MAC addresses, public IPs), since the repo is public.
