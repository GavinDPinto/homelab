# 02 — Proxmox VE install

- **Date:** 2026-11-26
- **Time taken:** ~90 minutes, from flashing the USB to reaching the server over Tailscale (incl. [03](03-networking.md) and [04](04-tailscale.md))
- **Status:** done

## What I used

- The M710q, power adapter, Ethernet cable
- A borrowed USB stick (~32GB, empty; wiped by flashing)
- Monitor + DisplayPort-to-HDMI dongle + HDMI cable, USB keyboard (first install only)
- My Windows laptop to flash the USB

## 1. Make the installer USB (on the laptop)

1. Downloaded **Proxmox VE 9.3-1** (`proxmox-ve_9.3-1.iso`, 1.80 GB) from <https://www.proxmox.com/en/downloads>.
2. Checked the download against the SHA256 shown on the download page:
   ```powershell
   Get-FileHash "$HOME\Downloads\proxmox-ve_9.3-1.iso" -Algorithm SHA256
   ```
   Result matched: `cc4fc97b9ac3a5712ad5b665195753571923a18e11f5c2ebb113fdc29a50bd36`
3. Flashed it with **Rufus 4.16** (Standard, x64):
   - Device: the USB stick (only USB drive plugged in)
   - Boot selection: the Proxmox ISO; everything else left default (greyed out)
   - START → when asked about an ISOHybrid image, picked **Write in DD Image mode** (ISO mode may not boot)
   - Waited for the green **READY**, then ejected via the taskbar ("Eject Mass Storage")
   - Afterwards Windows shows the stick as two drives (D: and E:). That's normal after DD mode; don't format them.

> DD mode writes the image byte-for-byte from the start of the stick, so **everything on the stick is erased**. Back up anything on it first, or use an empty stick.

## 2. BIOS settings

Plugged in monitor (via the dongle), keyboard, Ethernet, USB stick, then power. Tapped **F1** right after pressing power to open the *Lenovo BIOS Setup Utility*.

- BIOS version: M1AKT39A (07/16/2018)
- [x] *Advanced → CPU Setup → Intel Virtualization Technology*: **Enabled**
- [x] *Advanced → CPU Setup → VT-d*: **Enabled**
- [x] *Power → After Power Loss*: **Power On** (comes back by itself after an outage)
- Secure Boot: left as is (Proxmox 9 supports it)
- **F10** → Yes to save and exit

Then tapped **F12** at boot for the boot menu and picked the USB stick.

## 3. Installer choices

Picked **Install Proxmox VE (Graphical)**.

| Screen | What I chose | Notes |
|---|---|---|
| Target disk | `/dev/nvme0n1` (238.47GiB, WD PC SN730 256GB NVMe) | Wipes the whole SSD (Windows is gone) |
| Filesystem | ext4 | Simple default for a single disk |
| Country / time zone / keyboard | Canada / America/Toronto / en-us | |
| Root password | _(in password manager, never here)_ | |
| Email | my email | For alerts |
| Management interface | `nic0` (Intel, `e1000e` driver) | |
| Hostname (FQDN) | `m710q.home.arpa` | Named after the hardware so it stays clear if I add more servers. `.home.arpa` is reserved for home networks. The name doesn't depend on which network the server is on. |
| IP / gateway / DNS | Kept what it detected | Switched to DHCP afterwards → [03-networking.md](03-networking.md) |
| Auto reboot | Left ticked | **Pull the USB out when it reboots** (see problems below) |

After the reboot the console shows `m710q login:` and the web UI address.

## 4. First login

- From the laptop on the same network: `https://<server-ip>:8006`
- Chrome warns "Your connection is not private": expected, the certificate is self-signed. **Advanced → Proceed**.
- User `root`, realm **Linux PAM standard authentication**.
- Click OK on the "No valid subscription" pop-up (shows on every login, harmless).
- Kernel after install: `7.0.14-30-pve`

## 5. Updates (free repository)

Proxmox points at the paid enterprise repository by default, so updates fail until it's switched.

1. *m710q → Updates → Repositories*
2. Disabled both `enterprise` repositories (`pve-enterprise` and Ceph)
3. **Add → No-Subscription → Add**
4. *m710q → Shell*:
   ```bash
   apt update && apt full-upgrade -y
   reboot
   ```
   No errors.

## Next

- [x] Switch host networking to DHCP → [03-networking.md](03-networking.md)
- [x] Install Tailscale → [04-tailscale.md](04-tailscale.md)
- [x] Unplug screen and keyboard; still reachable over Tailscale after a reboot

## Problems hit and how I fixed them

| Problem | Fix |
|---|---|
| Monitor said "no signal" after saving the BIOS with F10 | Power-cycled the server (held power ~5s, pressed again). The DP-to-HDMI dongle can lose the picture during a restart. |
| After the install it booted back into the installer menu, and the web UI timed out (`ping` said "Destination host unreachable") | The USB stick was still plugged in. Removed it and restarted; it booted Proxmox from the SSD and the web UI loaded. **Don't pick anything on the installer menu**: "Install" again would wipe the SSD. |
