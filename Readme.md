# Docker Control Script 

This repository provides a simple shell script to **strictly control Docker usage** on a systemd-based Linux distribution such as Nobara 43.

## Objective

* Keep Docker **installed but never auto-starting at boot**
* Allow **manual start only when needed**
* Provide a **simple command interface** for daily use

---

## How It Works

The script uses `systemctl` with **masking**:

* `mask` → completely prevents Docker from starting (even manually)
* `unmask` → allows Docker to start again

This ensures Docker is **strictly disabled unless explicitly enabled by the script**.

---

## Script Usage

### Commands

```bash
dockerctl start    # Start Docker (temporarily enables it)
dockerctl stop     # Stop Docker and lock it again
dockerctl status   # Check Docker state
```

---

## Installation

1. Save the script as `dockerctl`

2. Make it executable:

```bash
chmod +x dockerctl
```

3. Move it to a global path:

```bash
sudo mv dockerctl /usr/local/bin/dockerctl
```

---

## Initial Setup (Important)

Run this once to enforce strict behavior:

```bash
sudo systemctl stop docker
sudo systemctl stop docker.service
sudo systemctl disable docker
sudo systemctl disable docker.service
sudo systemctl mask docker
sudo systemctl mask docker.service
```

After this:

* Docker will **never start at boot**
* Docker cannot start unless the script unmaskes it

---

## Typical Workflow

Start Docker when needed:

```bash
dockerctl start
```

Stop and lock Docker again:

```bash
dockerctl stop
```

Check status:

```bash
dockerctl status
```

---

## Notes

* Requires `sudo` privileges
* Designed for **systemd-based systems** (Nobara, Fedora, Ubuntu, etc.)
* Does **not uninstall Docker**, only controls its runtime behavior
* Works with standard Docker service (`docker.service`)

---

## Why Mask Instead of Disable?

* `disable` → prevents auto-start at boot
* `mask` → prevents *any* start unless explicitly reversed

This script uses masking to enforce strict control.

---

## Optional Improvement

If you want to avoid typing `sudo` repeatedly, you can allow passwordless systemctl for your user via `sudoers` (advanced users only).

---


