# 🖥️ Lab 1 — Introduction & Startup

> **Lab Tutorial** — Set up your VMware environment and explore both Linux and Windows Server for the first time.

← [Back to all labs](../README.md)

---

## What this lab covers

Lab 1 introduces the virtualization environment and walks through the initial startup and configuration of both CentOS and Windows Server. You will explore the boot process, system management tools, runlevels, log files, and basic command-line operations on both platforms.

---

## 📂 Parts

| Part | Platform | Topic |
| --- | --- | --- |
| [Lab 1a](./lab-1a.md) | Both | VMware Lab Familiarisation |
| [Lab 1b](./lab-1b.md) | CentOS 10 | System Startup, Runlevels & Log Files |
| [Lab 1c](./lab-1c.md) | Windows Server 2025 | Windows Server Initial Configuration |

---

## 📋 Lab 1a — VMware Lab Familiarisation

**Aim:** Install VMware, import VM images, and explore the environment for the first time.

| Task | Summary |
| --- | --- |
| **Lab Setup** | Download VMware Workstation Pro from Broadcom, register an account, import the CentOS and Windows Server `.ova` files |
| **Task 1 — Navigate the VMware Lab** | Open VMware, explore VM settings (RAM, CPU, disk, network adapters), observe the CentOS boot sequence (BIOS → GRUB → progress bar) |
| **Task 2 — Log in and explore** | Log in as root, complete GNOME initial setup, open a terminal, run `ifconfig` and `ls`, switch to superuser with `su` |
| **Task 3 — Reboot and shutdown** | Reboot with `reboot`, shut down with `shutdown -h now`, observe the shutdown sequence, understand why CLI shutdown matters |

---

## 📋 Lab 1b — System Startup, Runlevels & Log Files

**Aim:** Explore the Linux boot process, systemd targets, and system logs on CentOS.

| Task | Summary |
| --- | --- |
| **Task 1 — Boot into single-user mode** | Interrupt GRUB2, edit the kernel boot line to replace `ro` with `rw init=/sysroot/bin/sh`, boot into a passwordless root shell |
| **Task 2 — Explore system startup** | Use `systemctl get-default`, `set-default`, and `isolate` to switch between `graphical`, `multi-user`, `emergency`, and `rescue` targets; manage services with `start`, `stop`, `enable`, `disable`, `is-enabled`, `is-active` |
| **Task 3 — Examine log files** | View the kernel ring buffer with `dmesg`, read `/var/log/messages` and `/var/log/secure`, query the systemd journal with `journalctl` using time, unit, and priority filters |

---

## 📋 Lab 1c — Windows Server Initial Configuration

**Aim:** Start, configure, and explore Windows Server using Server Manager and the command line.

| Task | Summary |
| --- | --- |
| **Task 1 — Startup** | Boot Windows Server, log in as Administrator, configure time zone, network, computer name, and Windows Update settings via Local Server in Server Manager |
| **Task 2 — Server Management** | Explore Device Manager, find network IP, add Telnet Client and Simple TCP/IP Services features, test with `telnet localhost 13` and `telnet localhost 17`, test from host workstation |
| **Task 3 — Command line** | Use `net start` to list services, `net stop/start` to control them, add a Windows Firewall rule with `netsh advfirewall`, verify telnet from host workstation |
| **Task 4 — Documentation** | Explore Server Manager Help, note Internet Explorer Enhanced Security Configuration settings |
| **Task 5 — Shutdown** | Shut down via Start Menu, enter a shutdown reason in the Event Tracker dialog |

---

## 🚀 How to use this lab

1. Start with **Lab 1a** to get your VMware environment running
2. Work through **Lab 1b** (CentOS) and **Lab 1c** (Windows Server) in either order
3. Record all observations in your **Learning Journal**
4. Answer the reflection questions as you go

---

*Lab 1 · Introduction & Startup*
