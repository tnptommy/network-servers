# 🐧 Lab 1b — System Startup, Runlevels & Log Files

> **Lab Tutorial** — Explore the Linux boot process, modify systemd targets, and examine system log files on CentOS.

← [Back to Lab 1](./README.md)

---

## Aims

1. To be able to boot a machine into single-user mode using the bootloader
2. To explore and modify the system startup procedures in Linux
3. To be able to change runlevels (targets)
4. To become familiar with the system log files

---

## Task 1: Boot Single-User Shell Only, Using the Boot Loader

Boot up your CentOS VM (reboot it if already running).

You will see the BIOS screen briefly, then the **GRUB2 boot loader** will appear with the message `Press any key . . .` — press a key to interrupt the boot process.

> It may take several attempts to get your timing right.

### Steps to enter single-user mode

**1.** The GRUB2 screen shows all available kernels. You can install additional kernels by editing `/etc/grub2.cfg` later. Highlight the desired kernel and press **`E`** to edit its boot commands.

**2.** The screen changes to show several lines of boot configuration. Use the arrow keys to move to the line that starts with `linux....`. We want to pass a new option to the kernel to tell it to boot with just a simple Unix shell.

**3.** On that line, find the word `ro` as a word by itself. You need to:

```
Delete:   ro
Type:     rw init=/sysroot/bin/sh
```

**4.** When you have typed it correctly, press **`Ctrl+X`** to boot with your new parameters.

---

The machine will now boot and give you a **root shell without asking for a password**.

- Changing `ro` (read-only) to `rw` (read-write) means the filesystem is mounted in writable mode — you can edit files if needed
- The actual system files are inside `/sysroot` when you are in single-user mode

Single-user mode is mainly used for **system maintenance** — when you need to be sure no one else is using the system and only a minimum number of processes are running.

---

## 📸 Screenshot — GRUB2 edit screen

> *(Add your screenshot of the GRUB2 kernel edit screen here)*

---

📓 **Journal:** Document the full process of booting into single-user mode using GRUB2.

Also note:
- This process did **not** require a password — this is also how **password recovery** works on Linux via GRUB2
- **Security risk:** If an attacker has physical access to your machine, they can do the same thing. GRUB2 can be password-protected to prevent this
- There is also a **rescue mode** which still gives a single-user environment but **does** require the root password

---

## Task 2: Explore and Modify System Startup

From single-user mode, exit back to normal graphical mode using:

```bash
reboot
```

### Systemd targets (formerly runlevels)

There are different modes the system can boot into. Previously these were called **runlevels** — in modern Linux they are called **targets**.

| Target | Description |
| --- | --- |
| `emergency.target` | Emergency recovery — minimal environment, similar to single-user shell |
| `rescue.target` | Rescue mode — for system recovery, **requires root password** |
| `multi-user.target` | Multi-user mode with no graphical login |
| `graphical.target` | Full graphical mode (default) |

---

### Check and change the default target

Check the current default boot mode:

```bash
systemctl get-default
```

> Should return `graphical.target`.

Change the default with:

```bash
systemctl set-default multi-user.target
```

Switch to a target **immediately** (without rebooting):

```bash
systemctl isolate multi-user.target
```

Switch back:

```bash
systemctl isolate graphical.target
```

Also try:

```bash
systemctl isolate emergency.target
systemctl isolate rescue.target
```

---

## 📸 Screenshot — Target switching

> *(Add your screenshot of switching between targets here)*

---

📓 **Journal:** Document what happens visually when you isolate each target. What changes on screen? What disappears?

---

### Managing services with systemctl

`systemctl` is the command that manages the startup process and which services run at boot time. Now that we have seen targets, let's explore **services**.

List all units (targets, services, and other types):

```bash
systemctl list-unit-files
```

For services, this shows whether they are **enabled** (starts at boot) or **disabled** (does not start at boot).

📓 **Journal question:** Is the `sshd` service enabled or disabled by default? What about `httpd`?

---

Check a single service's enabled/disabled state:

```bash
systemctl is-enabled sshd
```

Check if a service is currently running (active) or stopped (inactive):

```bash
systemctl is-active sshd
```

Start, stop, enable, and disable services:

```bash
systemctl start <servicename>
systemctl stop <servicename>
systemctl enable <servicename>
systemctl disable <servicename>
```

After each change, verify using `is-enabled` and `is-active`.

📓 **Journal:** Document each command you run, the output you get, and what it means.

---

## 📸 Screenshot — systemctl output

> *(Add your screenshot of systemctl list-unit-files or service status here)*

---

## Task 3: Examine System Log Information

### Kernel ring buffer

Run:

```bash
dmesg
```

Also try:

```bash
journalctl --dmesg
```

📓 **Journal:** What kinds of information appear? Hardware detection? Driver loading? Error messages?

---

### System log files

```bash
cat /var/log/messages
```

```bash
cat /var/log/secure
```

📓 **Journal:** What kinds of messages appear in each file? What is the difference between them?

> 💡 **Hint:** If you cannot read a file, check its permissions with `ls -l`. You may need to `su` as root and use `chmod` to change permissions.

---

### Using journalctl

`journalctl` queries the systemd journal — the central log for everything managed by systemd.

Try each of the following commands and document what they show in your Learning Journal. If you are not sure what a command does, look it up.

```bash
journalctl --since "10 minutes ago"
```

```bash
journalctl --since "2020-01-01" --until "yesterday"
```

```bash
journalctl -u multi-user.target
```

```bash
journalctl -u sshd.service
```

```bash
journalctl -u sshd.service --since "10 minutes ago"
```

```bash
journalctl -p err
```

```bash
journalctl -p warning
```

```bash
journalctl -p err --since "1 hour ago"
```

---

## 📸 Screenshot — journalctl output

> *(Add your screenshot of journalctl output here)*

---

📓 **Remember:** Note anything new or interesting you find in your Learning Journal!

---

## 💡 Command Reference

| Command | What it does |
| --- | --- |
| `systemctl get-default` | Show the current default boot target |
| `systemctl set-default <target>` | Change the default boot target permanently |
| `systemctl isolate <target>` | Switch to a target immediately (no reboot) |
| `systemctl list-unit-files` | List all units and their enabled/disabled state |
| `systemctl is-enabled <svc>` | Check if a service starts automatically at boot |
| `systemctl is-active <svc>` | Check if a service is currently running |
| `systemctl start <svc>` | Start a service now |
| `systemctl stop <svc>` | Stop a service now |
| `systemctl enable <svc>` | Enable a service to start at boot |
| `systemctl disable <svc>` | Disable a service from starting at boot |
| `dmesg` | Show the kernel ring buffer |
| `journalctl` | Query the systemd journal (all logs) |
| `journalctl -u <unit>` | Show logs for a specific unit |
| `journalctl -p err` | Show only error-level log messages |
| `journalctl --since "..."` | Show logs since a specific time |

---

*Lab 1b · System Startup, Runlevels & Log Files*
