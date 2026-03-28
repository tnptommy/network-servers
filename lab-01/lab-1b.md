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

You will see the BIOS screen briefly, then the **GRUB2 boot loader** will appear with the message `Press any key . . .`

Interrupt the boot process by pressing a key when you see this message.

> ⚠️ It may take several attempts to get your timing right.

---

### Step 1 — Get to the GRUB2 menu

When GRUB2 appears, you will see a list of available kernels. Highlight the desired kernel using the arrow keys.

> 📸 *Screenshot — GRUB2 boot loader kernel selection screen*
> ![GRUB2 kernel list](./screenshots/1b-01-grub2-menu.png)

---

### Step 2 — Edit the kernel boot entry

Press **`E`** to edit the boot commands for the selected kernel.

The screen changes to show several lines of boot configuration.

> 📸 *Screenshot — GRUB2 kernel edit screen (lines of boot config)*
> ![GRUB2 edit screen](./screenshots/1b-02-grub2-edit.png)

---

### Step 3 — Modify the boot parameters

Using the arrow keys, move the cursor to the line that starts with `linux....`.

On this line, find the word `ro` as a word by itself. Make the following change:

```
Delete:   ro
Type:     rw init=/sysroot/bin/sh
```

The line should now contain `rw init=/sysroot/bin/sh` where `ro` used to be.

> 📸 *Screenshot — The linux line with ro replaced by rw init=/sysroot/bin/sh*
> ![Modified boot parameters](./screenshots/1b-03-grub2-modified.png)

---

### Step 4 — Boot with new parameters

Press **`Ctrl+X`** to boot the machine with your modified parameters.

The machine will now boot and give you a **root shell without asking for a password**.

> 📸 *Screenshot — Single-user root shell prompt after booting*
> ![Single-user root shell](./screenshots/1b-04-single-user-shell.png)

---

### What just happened?

- Changing `ro` (read-only) to `rw` (read-write) means the filesystem is mounted in **writable** mode — you can edit files if needed
- The actual system files are inside `/sysroot` when you are in single-user mode
- Single-user mode is mainly used for **system maintenance** — when you need to be sure no one else is using the system and only a minimum number of processes are running

📓 **Journal:** Document the full step-by-step process of booting into single-user mode using GRUB2.

Also note the following security implications:

- This process did **not** require a password — this is also how **password recovery** works on Linux via GRUB2
- **Security risk:** If an attacker has physical access to your machine, they can do the same. GRUB2 can be password-protected to prevent this
- There is also a **rescue mode** which still gives a single-user environment but **does** require the root password

---

## Task 2: Explore and Modify System Startup

From single-user mode, exit back to normal graphical mode:

```bash
reboot
```

---

### Systemd targets (formerly runlevels)

There are different modes the system can boot into. Previously these were called **runlevels** — in modern Linux they are called **targets**.

| Target | Description |
| --- | --- |
| `emergency.target` | Emergency recovery — minimal environment, like single-user shell |
| `rescue.target` | Rescue mode — system recovery, **requires root password** |
| `multi-user.target` | Multi-user mode, no graphical login |
| `graphical.target` | Full graphical mode (default) |

---

### Step 1 — Check the current default target

```bash
systemctl get-default
```

> Should return `graphical.target`.

> 📸 *Screenshot — Terminal showing output of systemctl get-default*
> ![systemctl get-default output](./screenshots/1b-05-get-default.png)

---

### Step 2 — Switch to multi-user mode (no GUI)

```bash
systemctl isolate multi-user.target
```

> 📸 *Screenshot — Screen after isolating multi-user.target (GUI disappears)*
> ![multi-user.target active](./screenshots/1b-06-multi-user-target.png)

📓 **Journal:** What happened visually? What disappeared from the screen?

---

### Step 3 — Switch back to graphical mode

```bash
systemctl isolate graphical.target
```

> 📸 *Screenshot — Screen after returning to graphical.target (GUI restored)*
> ![graphical.target restored](./screenshots/1b-07-graphical-target.png)

---

### Step 4 — Try emergency and rescue targets

```bash
systemctl isolate emergency.target
```

> 📸 *Screenshot — emergency.target environment*
> ![emergency.target](./screenshots/1b-08-emergency-target.png)

```bash
systemctl isolate rescue.target
```

> 📸 *Screenshot — rescue.target prompt (requires root password)*
> ![rescue.target](./screenshots/1b-09-rescue-target.png)

📓 **Journal:** Document what happens with each target. What is different between them? What does each one give you access to?

---

### Step 5 — Change the default target permanently

```bash
systemctl set-default multi-user.target
```

> 📸 *Screenshot — Terminal showing systemctl set-default output*
> ![systemctl set-default](./screenshots/1b-10-set-default.png)

Change it back to graphical:

```bash
systemctl set-default graphical.target
```

---

### Step 6 — List all units

```bash
systemctl list-unit-files
```

This shows all **units** — units can be targets, services, or a few other types. For services, it shows whether they are **enabled** (starts at boot) or **disabled** (does not start at boot).

> 📸 *Screenshot — Terminal showing systemctl list-unit-files output*
> ![systemctl list-unit-files](./screenshots/1b-11-list-unit-files.png)

📓 **Journal question:** Is the `sshd` service enabled or disabled by default? What about `httpd`?

---

### Step 7 — Check individual service status

Check if a single service is enabled (will start at boot):

```bash
systemctl is-enabled sshd
```

> 📸 *Screenshot — Terminal showing systemctl is-enabled sshd output*
> ![is-enabled sshd](./screenshots/1b-12-is-enabled.png)

Check if a service is currently active (running right now):

```bash
systemctl is-active sshd
```

> 📸 *Screenshot — Terminal showing systemctl is-active sshd output*
> ![is-active sshd](./screenshots/1b-13-is-active.png)

---

### Step 8 — Start, stop, enable, and disable services

```bash
systemctl start sshd
systemctl stop sshd
systemctl enable sshd
systemctl disable sshd
```

After each command, verify the change using `is-enabled` and `is-active`.

> 📸 *Screenshot — Terminal showing start/stop/enable/disable commands and verification*
> ![service start stop enable disable](./screenshots/1b-14-service-control.png)

📓 **Journal:** Run each command and record the output. What does each one do? What is the difference between `start` and `enable`?

---

## Task 3: Examine System Log Information

### Step 1 — View the kernel ring buffer with dmesg

```bash
dmesg
```

> 📸 *Screenshot — Terminal showing dmesg output (hardware/driver messages)*
> ![dmesg output](./screenshots/1b-15-dmesg.png)

Also compare with:

```bash
journalctl --dmesg
```

> 📸 *Screenshot — Terminal showing journalctl --dmesg output*
> ![journalctl --dmesg](./screenshots/1b-16-journalctl-dmesg.png)

📓 **Journal:** What kinds of information appear? Hardware detection? Driver loading? Error messages? What is the difference between `dmesg` and `journalctl --dmesg`?

---

### Step 2 — Examine /var/log/messages

```bash
cat /var/log/messages
```

> 📸 *Screenshot — Terminal showing /var/log/messages content*
> ![/var/log/messages](./screenshots/1b-17-var-log-messages.png)

📓 **Journal:** What kinds of messages appear in this file?

> 💡 **Hint:** If you cannot read a file, check its permissions with `ls -l`. You may need to switch to root with `su` and use `chmod` to change permissions.

---

### Step 3 — Examine /var/log/secure

```bash
cat /var/log/secure
```

> 📸 *Screenshot — Terminal showing /var/log/secure content*
> ![/var/log/secure](./screenshots/1b-18-var-log-secure.png)

📓 **Journal:** What is in this file? How does it differ from `/var/log/messages`?

---

### Step 4 — Query logs by time with journalctl

```bash
journalctl --since "10 minutes ago"
```

> 📸 *Screenshot — journalctl --since "10 minutes ago" output*
> ![journalctl since 10 minutes](./screenshots/1b-19-journalctl-since.png)

```bash
journalctl --since "2020-01-01" --until "yesterday"
```

> 📸 *Screenshot — journalctl with date range output*
> ![journalctl date range](./screenshots/1b-20-journalctl-daterange.png)

---

### Step 5 — Query logs by unit with journalctl

```bash
journalctl -u multi-user.target
```

> 📸 *Screenshot — journalctl -u multi-user.target output*
> ![journalctl -u multi-user](./screenshots/1b-21-journalctl-unit-multiuser.png)

```bash
journalctl -u sshd.service
```

> 📸 *Screenshot — journalctl -u sshd.service output*
> ![journalctl -u sshd](./screenshots/1b-22-journalctl-unit-sshd.png)

```bash
journalctl -u sshd.service --since "10 minutes ago"
```

> 📸 *Screenshot — journalctl -u sshd.service --since output*
> ![journalctl -u sshd since](./screenshots/1b-23-journalctl-sshd-since.png)

---

### Step 6 — Filter logs by priority

```bash
journalctl -p err
```

> 📸 *Screenshot — journalctl -p err output (errors only)*
> ![journalctl -p err](./screenshots/1b-24-journalctl-err.png)

```bash
journalctl -p warning
```

> 📸 *Screenshot — journalctl -p warning output*
> ![journalctl -p warning](./screenshots/1b-25-journalctl-warning.png)

```bash
journalctl -p err --since "1 hour ago"
```

> 📸 *Screenshot — journalctl -p err --since "1 hour ago" output*
> ![journalctl -p err since 1 hour](./screenshots/1b-26-journalctl-err-since.png)

📓 **Journal:** For each `journalctl` command, document what it shows and what the output means. If you are unsure what a flag does, look it up.

---

## 💡 Command Reference

| Command | What it does |
| --- | --- |
| `systemctl get-default` | Show the current default boot target |
| `systemctl set-default <target>` | Permanently change the default boot target |
| `systemctl isolate <target>` | Switch to a target immediately without rebooting |
| `systemctl list-unit-files` | List all units and their enabled/disabled state |
| `systemctl is-enabled <svc>` | Check if a service starts automatically at boot |
| `systemctl is-active <svc>` | Check if a service is currently running |
| `systemctl start <svc>` | Start a service immediately |
| `systemctl stop <svc>` | Stop a service immediately |
| `systemctl enable <svc>` | Enable a service to start at boot |
| `systemctl disable <svc>` | Disable a service from starting at boot |
| `dmesg` | View the kernel ring buffer (hardware/driver messages) |
| `journalctl` | Query the systemd journal (all logs) |
| `journalctl --dmesg` | View kernel messages via journalctl |
| `journalctl -u <unit>` | Show logs for a specific unit |
| `journalctl -p err` | Show only error-level log messages |
| `journalctl -p warning` | Show only warning-level and above messages |
| `journalctl --since "..."` | Show logs since a specific time |
| `journalctl --until "..."` | Show logs up until a specific time |

---

*Lab 1b · System Startup, Runlevels & Log Files*
