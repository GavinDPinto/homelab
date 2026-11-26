# 0002 — Proxmox VE as the base OS

- **Date:** 2026-11-23 (decided when I bought the server)
- **Status:** accepted

## Context

What I want the server to do:

- Host my personal website
- Photo backup (future)
- Jellyfin (future)
- Learning, especially virtualization: I'm taking an OS course next term and want to understand how VMs work by running them myself

## Options considered

1. **Plain Ubuntu + Docker:** simpler, and enough to run the apps. But I wanted to learn how to spin up VMs and virtualize, which this doesn't really give me.
2. **Proxmox VE:** a free hypervisor that runs VMs and containers. It's what WATcloud (the design team I'm on) uses.

## Decision

Install Proxmox VE on bare metal.

## Why

- I want to learn a real hypervisor, and that ties into my OS course next term.
- It's free.
- WATcloud uses Proxmox and VMs, so I wanted to play with it myself.

## Consequences

- **Easier:** learning virtualization hands-on, and the experience carries over to WATcloud.
- **Harder:**
  - The installer wipes the SSD, so the Windows 11 license that came with the PC is gone.
  - With 4 cores and 16GB RAM, I can only run a few full VMs at once (containers are much lighter).
- **Revisit:** ____
