# 🖥️ Lab 1a — VMware Lab Familiarisation

> **Lab Tutorial** — Install VMware, import your CentOS and Windows Server VMs, then explore the environment for the first time.

← [Back to Lab 1](./README.md)

---

## Aims

1. To explore the VMware software
2. To be able to start and shutdown your image running under VMware

---

## Lab Setup — Install VMware & Download VM Images

### Step 1 — Check system requirements

Your host machine needs at least **8GB RAM** (16GB recommended) — VMware needs memory alongside your host OS.

> Skip this step if you are using a lab machine.

---

### Step 2 — Download VMware Workstation Pro

Go to the [Broadcom website](https://www.broadcom.com) to download VMware Workstation Pro.

> ⚠️ The UTS software portal no longer hosts this download.

---

### Step 3 — Register a Broadcom account

Log in to the Broadcom dashboard:

1. Click **My Downloads**
2. Choose **VMware Cloud Foundation**

---

### Step 4 — Find VMware Workstation Pro

Search for `VMware Workstation` and click **VMware Workstation Pro**.

---

### Step 5 — Download for your platform

Choose **VMware Workstation Pro for Personal Use (Windows)** if you are on a Windows laptop or PC.

Tick the **Terms & Conditions** checkbox to enable the download button at the bottom right.

---

### Step 6 — Import the `.ova` file

1. Open VMware Workstation Pro
2. Click **Open a Virtual Machine**
3. Select the `.ova` file (CentOS or Windows Server)
4. Provide a **name** and **storage path** for the new VM
5. Click **Import**

> ✅ You only ever need to import each image **once**.

Storage path examples:

```
/class/NetServers/home/nnnnnnnn/centos64   ← LAN lab machines only
/media/XYZ/centos64                        ← USB hard disk (XYZ = disk label)
```

---

### Step 7 — Log in to the VMs

Once imported, power on the VM and log in:

```
username: root
password: student123!
```

> ⚠️ Never use a trivial password like this in production. These are disposable lab VMs only.

---

## 📸 Screenshot — VMware import dialog

> *(Add your screenshot of the Import Virtual Machine dialog here)*

---

## Task 1: Navigate through the VMware Lab

On your lab workstation, open VMware Player:

- **Linux lab machine:** Applications → System Tools → VMplayer
- **Windows:** Launch VMware Workstation Pro from the Start menu

From VMware Player/Workstation, click **Open a Virtual Machine** → select `CentOS.ova` or `WindowsServer.ova` → provide the VM name and storage path → click **Import**.

### Starting your VMs

Once imported, click **Power on this virtual machine** to start.

> To get mouse/keyboard access inside the VM window, click inside it.  
> To return to your host desktop from console mode, press **Ctrl+Alt**.

### Exploring VM settings

After importing, click **Edit VM Settings** and look at the device list. For each device, note:

- What is it?
- What could you change?

📓 **Journal:** Record the full hardware configuration of your VM — RAM, processors, hard disk, network adapters, and any other devices listed. In real organisations this is done via an enterprise configuration manager or a formal database. We use a Learning Journal.

---

### Boot sequence observations

When you start CentOS, observe and document each stage:

**1. BIOS startup** — flashes quickly on screen.

> Optional: Press **F2** as soon as the logo appears to enter the BIOS setup screen. Examine settings such as time/date and boot sequence.

**2. GRUB boot loader** — you can let it boot automatically, or press any key quickly to see the GRUB menu.

> Optional: Press **Enter** quickly when the GRUB screen appears to get the GRUB menu. What boot parameters can you see in the Linux configuration?

**3. Boot progress bar** — press **ESC** to see the detailed startup sequence. Observe what services and processes start up.

---

## 📸 Screenshot — CentOS boot sequence

> *(Add your screenshot of the CentOS boot progress or GRUB menu here)*

---

## Task 2: Log in as Root and Explore

Log in as the root user:

```
Username: root
Password: student123!
```

You may be taken through the GNOME initial setup (language, keyboard, location services, online accounts).

> ⚠️ Do **not** connect any online or social media accounts. Press the blue **Next** or grey **Skip** button at each step.

Do not change the password.

### Open a terminal

Go to **Activities** → choose the icon that looks like a grey command window.

Try these commands and observe the output:

```bash
ifconfig
```

What do you see? Can you identify the default TCP/IP address assigned to this machine?

```bash
ls
```

📓 **Journal:** Record what `ifconfig` shows — interface names, IP addresses, and any other details you notice. Recall your Unix commands from Web Systems or Unix Systems Programming.

---

### Logging on as root — a word of caution

Logging on directly as root is extremely rare in practice. We only do it here because these are disposable lab VMs. In real life:

- The root password must be very hard to guess and crack
- Root has full system control — one wrong command can easily cripple or destroy your installation
- Best practice: log in as a regular user and use `sudo` for privileged commands
- Alternative: log in as yourself and switch to root using `su`

Switch to superuser now:

```bash
su
```

Enter the root password when prompted. Notice the prompt changes to `#` — this confirms superuser privileges are active.

---

## Task 3: Reboot and Shutdown

### Reboot

```bash
reboot
```

Observe the screen carefully during shutdown and reboot. Make notes of what happens.

📓 **Journal question:** Why would a system administrator **not** normally just reboot a running production server?

---

### Shutdown

```bash
shutdown -h now
```

Observe what happens on screen and document it.

> Note: You don't always need the command line — your Linux distribution also lets you shut down via the GUI power button near the top right of the screen.

📓 **Journal question:** Why is it important to know how to shut down from the command line, even when GUI menus are available?

---

## 📸 Screenshot — Shutdown process

> *(Add your screenshot of the shutdown sequence here)*

---

## If you move to another machine

If you take your USB/SSD to a different workstation:

1. Open VMware Player
2. Go to **File → Open**
3. Navigate to your USB hard disk directory
4. Select the `.vmx` file for your VM (normally there is only one)

You will see the startup panel showing the state, OS, version, and RAM.

---

## 💡 Tips

| Tip | Details |
| --- | --- |
| Release mouse/keyboard from VM | Press `Ctrl+Alt` |
| Enter BIOS setup | Press `F2` immediately when the boot logo appears |
| See GRUB menu | Press any key quickly when the GRUB screen appears |
| See detailed boot log | Press `ESC` during the boot progress bar |
| Suspend VM (save state) | VMware menu: **VM → Power → Suspend** — saves to `*.vmem` and `*.vmss` |

---

## 💡 Command Reference

| Command | What it does |
| --- | --- |
| `ifconfig` | Show network interface configuration |
| `ls` | List files and directories |
| `su` | Switch to superuser (root) |
| `reboot` | Reboot the system |
| `shutdown -h now` | Shut down the system immediately |

---

*Lab 1a · VMware Lab Familiarisation*
