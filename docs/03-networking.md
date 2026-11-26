# 03 — Networking (DHCP on the host)

- **Date:** 2026-11-26
- **Status:** done on the current network; not yet tested after a move

## The problem

The server moves between my off-campus student housing (wall Ethernet from the building's own network) and home. The installer sets a **static IP** for the network it was installed on. On the other network that address doesn't work, so the host gets no internet, Tailscale can't connect, and I'd need a screen to fix it.

## The fix: DHCP on the bridge

Let the host get its address automatically wherever it's plugged in, and reach it by name over Tailscale ([04-tailscale.md](04-tailscale.md)) instead of by local IP.

Done at the server's own keyboard and monitor, so a mistake couldn't lock me out.

### 1. Look first

```bash
cat /etc/network/interfaces
cat /etc/hosts
which dhclient dhcpcd
```

- `vmbr0` (the bridge Proxmox uses) was `inet static`, with the install-time address and gateway, `bridge-ports nic0`
- There's also a `nic1` set to `manual`, probably the Wi-Fi card. Not used, left alone.
- Both `dhclient` and `dhcpcd` are installed, so DHCP works out of the box

### 2. Back up, then edit

```bash
cp /etc/network/interfaces /etc/network/interfaces.static-backup
nano /etc/network/interfaces
```

Changed `static` to `dhcp` and deleted the `address` and `gateway` lines. The `vmbr0` section now looks like this:

```
auto vmbr0
iface vmbr0 inet dhcp
        bridge-ports nic0
        bridge-stp off
        bridge-fd 0
```

Saved with Ctrl+O, Enter, exited with Ctrl+X.

### 3. Apply and check

```bash
ifreload -a
ip -br a show vmbr0              # UP, got an address
ping -c 3 1.1.1.1                # 3/3 replies
tailscale status                 # m710q + laptop, still direct
ip a show vmbr0 | grep dynamic   # "dynamic" = the address came from DHCP
reboot                           # survived a reboot; https://m710q:8006 loads over Tailscale
```

The router handed out the same address as before, so the only way to tell it's DHCP now is the `dynamic` flag.

### Undo (if it ever breaks)

```bash
cp /etc/network/interfaces.static-backup /etc/network/interfaces
ifreload -a
```

## Notes

- **Don't edit `vmbr0` on the web UI's Network page.** It's built around static IPs and could undo this.
- **`/etc/hosts`** still has the install-time IP next to `m710q.home.arpa m710q`. Left as is for now: on a single server it mostly just means the console banner shows an old address, and Tailscale doesn't use it.
- Containers/VMs should use DHCP too, or only be reached over Tailscale, so nothing is tied to one network.

## Still to verify after the first move

- [ ] Host gets an address on the other network
- [ ] `https://m710q:8006` loads over Tailscale there
- [ ] Nothing breaks because of the out-of-date `/etc/hosts` line: ____

## Housing network rules

- [ ] Personal servers allowed? ____
- [ ] Device registration (by MAC) needed? ____
- [ ] Devices isolated from each other? ____ (Tailscale works around isolation)

## Problems hit and how I fixed them

| Problem | Fix |
|---|---|
| | |
