# 0001 — Used business mini PC as the first server

- **Date:** 2026-11-23
- **Status:** accepted

## Context

I'm starting my first homelab on a budget of roughly $200–300 CAD. The server has to run 24/7 in my room in off-campus student housing, so it needs to be small, quiet and low-power, and easy to carry home and back. It should run Proxmox now and Jellyfin later, which means hardware video transcoding.

## Options considered

1. **Old desktop tower:** cheap or free, but big, louder and uses more power.
2. **Rack server:** lots of cores and RAM, but far too loud, big and power-hungry for a bedroom.
3. **Raspberry Pi:** tiny and efficient, but ARM, limited RAM and storage, and not a great fit for Proxmox.
4. **Old laptop:** built-in screen and battery, but often no Ethernet port, and keeping a battery charged 24/7 isn't great.
5. **New budget mini PC (Intel N150 etc.):** modern and efficient, but prices went up in 2026 with the RAM/SSD shortage, and RAM is often soldered.
6. **Used business mini PC (Lenovo ThinkCentre Tiny / HP EliteDesk / Dell OptiPlex Micro):** older CPUs, but cheap, well built, upgradeable, and a common homelab choice.

## Decision

A refurbished Lenovo ThinkCentre M710q Tiny with an i5-7400T, 16GB RAM and a 256GB SSD, for $219.78 CAD.

## Why

- **Small, quiet, low-power:** the 35W CPU barely spins the fan, idle power is low, and it fits on a shelf.
- **A real x86 PC:** runs Proxmox and normal Linux software without compatibility issues, with an Intel NIC that works out of the box.
- **Good value:** a 4-core i5 with 16GB RAM for about $220 CAD beat what new mini PCs cost at the time.
- **Built to last:** business machines are made to run all day for years, and refurbished units are easy to find.
- **Upgradeable:** standard DDR4 SO-DIMM slots (up to 32GB) and a spare 2.5" drive bay.
- **7th-gen Intel:** the oldest generation with hardware HEVC 10-bit decoding (Quick Sync), which matters for Jellyfin later. I skipped 6th-gen options for this reason.
- **Cheaper than the HP EliteDesk 800 G3 ($275.99):** its i5-7500T is only ~10% faster with the same iGPU, and I didn't need its bigger SSD since media will go on an external drive.
- **Well documented:** lots of homelab guides already cover these machines.

## Consequences

- Only 4 cores / 4 threads: fine for containers, limiting for many full VMs.
- The 256GB SSD can fill up; the spare bay is the fix (needs a Lenovo SATA cable and tray).
- Only one NIC, so it can't easily be a router or firewall later.
- Video out is DisplayPort only, so the first install needs a DisplayPort-to-HDMI adapter.
- Revisit if I outgrow it: upgrading to 32GB RAM is the cheap first step.
