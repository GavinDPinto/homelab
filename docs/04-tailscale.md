# 04 — Tailscale

- **Date:** 2026-11-26
- **Status:** done

## Why

Reach the Proxmox web UI and SSH from my laptop no matter which network the server is on, without port forwarding. See [decisions/0003-tailscale.md](decisions/0003-tailscale.md).

## Before

- [x] Made a Tailscale account
- [x] Installed the Tailscale app on my laptop and signed in (did this while Proxmox was installing)

## Install on the Proxmox host

Needs the no-subscription repository set up first ([02-proxmox-install.md](02-proxmox-install.md#5-updates-free-repository)), since the install script uses `apt`.

In *m710q → Shell* (web UI), using Tailscale's official script:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
tailscale up
```

`tailscale up` prints a login link. Copied it into my laptop's browser and clicked **Connect** (same account as the laptop). No problems.

Check:

```bash
tailscale status
```

Shows `m710q` (linux) and `gavinlaptop` (windows), each with a `100.x.x.x` Tailscale address. The laptop connection says **direct**, so traffic goes straight between the two, not through Tailscale's relays.

Then `https://m710q:8006` worked from the laptop.

## Admin console settings

In <https://login.tailscale.com/admin>:

- [x] **MagicDNS:** already on by default, which is why `https://m710q:8006` works by name
- [x] **Key expiry disabled for `m710q`** (*Machines → m710q → ⋯ → Disable key expiry*). Otherwise it logs out after ~6 months and needs physical access to log back in. Left key expiry **on** for the laptop.

## Daily use

- Web UI: `https://m710q:8006` (click through the self-signed certificate warning)
- SSH: `ssh root@m710q` (not tested yet)

## Notes

- If a network blocks direct connections, Tailscale falls back to its relay servers automatically: slower, still encrypted end to end.
- **Never commit auth keys.** If I use a pre-generated auth key later, it goes in a gitignored `.env`.
- The public website will use Cloudflare Tunnel instead, since visitors won't have Tailscale (see 0003).

## Later (optional)

- Subnet router, to reach containers without installing Tailscale in each one: ____
- Proper HTTPS certificate for the web UI via Tailscale: ____

## Problems hit and how I fixed them

| Problem | Fix |
|---|---|
| | |
