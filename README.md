# Control Board - releases

Installers and signed updates for **Control Board**, a native Windows app
that watches a Proxmox homelab: hosts, guests, graphs, a network map,
Kubernetes, web checks, certificates, and alerts. The source is private; this
repository holds only builds.

**Status: private alpha.** Proprietary software: by installing it you accept
the [licence](LICENSE).

## What you need

- Windows 10 or 11 (64-bit)
- A Proxmox VE host or cluster that your PC can reach
- Optional: a Kubernetes cluster, websites to check, SSH access to the hosts

## What it connects to

Everything runs on your PC. The app talks directly from your PC to the
systems you add, and to nothing else of ours:

- your Proxmox, Kubernetes, hosts and web checks, with the read-only access
  you give it
- Cloudflare's API, only if you add a Cloudflare token
- GitHub, to check this repository for updates

There is no telemetry, no account and no server in the middle. Your settings,
history and secrets stay on your PC.

## Install

1. Download the newest `Control.Board_<version>_x64-setup.exe` from
   [Releases](../../releases/latest).
2. Run it. It installs for your user only, without admin rights. Windows
   SmartScreen warns that the publisher is unknown (the installer is not
   code-signed yet): choose **More info > Run anyway**.
3. Control Board opens on **Nothing is set up yet**.

## Set it up

Everything is added from **Sources > Add a source**. Each source is tested
before it can be saved: it must answer, and it must be **unable to change
anything**. Secrets go into Windows Credential Manager, never into a file.

**Proxmox** (start here). On any node, as root, make a read-only token:

```
pveum user add board@pve --comment "Control Board"
pveum aclmod / -user board@pve -role PVEAuditor
pveum user token add board@pve ro --privsep 0
```

The last command prints the secret once. In the app enter a name, the API
address (`https://<node-ip>:8006`), the token ID `board@pve!ro` and the
secret, then **Test** and **Save**. Hosts and guests appear within seconds.

**Host access (SSH)**, optional: lets the board read failed services, ZFS
pools, pending updates and backups on each host. The app makes its own key and
shows the line to add to `/root/.ssh/authorized_keys` (on a Proxmox cluster
that file is shared, so once per cluster). Replace `<your LAN>` in it with
your network, e.g. `192.168.1.0/24`. Without it, the host probe stays off.

**Kubernetes** and **web addresses**, optional: a read-only kubeconfig (list
and watch, no Secrets), or any URL to check every 20 seconds.

Guest **roles** come from Proxmox tags you choose. The settings file
(`%APPDATA%\com.parakalix.controlboard\board.yaml`, with comments) says what
a stopped guest with each role raises, and holds anything the wizard doesn't
cover.

## Updates

New versions appear in the title bar (`v0.10.0 → v0.10.1`). Click it, then
**Install & restart**. Every update is signed, and the app refuses one whose
signature does not match.

## Uninstall

Settings > Apps > Control Board. Your settings and history stay in
`%APPDATA%\com.parakalix.controlboard`; the secrets are in Credential Manager
(Windows Credentials) under names ending in `.Control Board`. Delete both for
a clean slate.

## Found a bug?

Open an [issue](../../issues) with what you did, what you expected and what
happened; a screenshot helps. Please leave out passwords, tokens, addresses
and anything else private: issues here are public.
