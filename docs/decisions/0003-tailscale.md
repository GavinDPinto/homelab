# 0003 — Tailscale for remote access

- **Date:** 2026-11-23 (decided together with Proxmox)
- **Status:** accepted

## Context

I want to reach the server mainly from my own laptop, including the Proxmox web UI, no matter where the server is: at my house, or at the residence where I stay during university. It moves between those two networks, so access can't depend on either one.

## Options considered

1. **Cloudflare Tunnel:** no port forwarding, but my traffic passes through Cloudflare's servers, which I didn't like.
2. **Tailscale:** connects my devices and the server to each other (usually directly), wherever they are.

## Decision

Use Tailscale on the Proxmox host and my laptop.

## Why

- No port forwarding.
- Free.
- Easy to set up.
- Moves with me between networks.
- WATcloud uses it.

## Consequences

- **Easier:** reaching the server the same way from anywhere, without touching either network's router.
- **Harder:** every device that connects needs the Tailscale app installed. That's fine for my own devices.
- **Revisit:** Tailscale only covers my own devices, so visitors to my public website can't use it. For the website I'll use Cloudflare Tunnel with my own domain. Tailscale stays for everything private.
