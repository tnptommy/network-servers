# 🪟 Lab 1c — Windows Server Initial Configuration

> **Lab Tutorial** — Start, configure, and explore Windows Server for the first time using Server Manager and the command line.

← [Back to Lab 1](./README.md)

---

## Aims

1. Set up the initial Windows Server configuration

---

## Task 1: Startup

The supplied VMware image of Windows Server has already been installed as an **evaluation version** (no license key). It will run for a limited time in evaluation mode.

Start your Windows Server VM in VMware Player (see [Lab 1a](./lab-1a.md) for instructions).

> **Note:** The Windows Server boot sequence is different from Linux — you cannot press `ESC` to see boot logging. Just be patient.

---

### Log in

Windows Server will boot to a GUI and prompt for the Administrator password.

You need to press **Ctrl+Alt+Delete** to bring up the login prompt — but your host machine may intercept this key combination. Use one of these alternatives instead:

- Press **Ctrl+Alt+Insert**
- Or use the VMware menu: **VM → Send Ctrl+Alt+Delete**

Enter the Administrator password:

```
student123!
```

You should now see the desktop and the **Server Manager → Dashboard** panel.

---

## 📸 Screenshot — Windows Server login and dashboard

> *(Add your screenshot of the Server Manager Dashboard here)*

---

### Initial configuration checklist

Click **Local Server** in the left panel of Server Manager. Check and change the following:

**1. Time zone** — Set to `(UTC+10:00) Canberra, Melbourne, Sydney` if not already set.

**2. Network** — Ethernet0 and Ethernet1 should both show `IPv4 address assigned by DHCP, IPv6 enabled`.  
Click `IPv4 address assigned by DHCP` to open the detailed Network Connections configuration page.

**3. Computer name** — Change to something more descriptive (e.g. `chrisserver`). Leave the workgroup as `WORKGROUP` for now. You will likely be prompted to reboot — you can do this now or after you finish exploring.

**4. Windows Update** — Note when updates were last installed or checked, and how Windows Update is configured. A server is different from a workstation — **you do not want the server rebooting itself unexpectedly** due to updates being installed.

📓 **Journal:** Record all the details you changed and anything else you find noteworthy.

Scroll down in Local Server to view **events (logs)**, **services**, and other information.

If you have not yet rebooted after changing the computer name, do so now before continuing.

---

## 📸 Screenshot — Local Server properties

> *(Add your screenshot of the Local Server configuration panel here)*

---

## Task 2: Server Management

The Server Manager is the main console for managing many aspects of your Windows Server. It allows you to:

1. View and update the **server summary** — computer info, security settings
2. View and update **server roles**
3. View and update **server features**
4. Access **common resources**, support links, and the **Best Practices Analyzer**

The **Tools** menu at the top gives access to additional management tools:

5. **Diagnostics** — Event Viewer, Device Manager
6. **Configuration** — Task Scheduler, Firewall, Services, Computer Management
7. **Storage** — Backup and Disk Management tools

---

### Activities

**Activity 1 — Device Manager**

Find and open Device Manager (hint: it is under the Tools menu).

- What hardware is installed on this machine?
- Is it similar to the CentOS VM's hardware?

📓 **Journal:** Write down all hardware you find.

---

**Activity 2 — Network address**

- What is the IP address of this server that connects to the internet?
- Compare it against your CentOS server — are they on the same subnet?
- What is the subnet mask and default gateway?

📓 **Journal:** Record the IP address, subnet mask, and gateway for both Ethernet0 and Ethernet1.

---

**Activity 3 — Add features**

Navigate to **Manage → Add Roles and Features** and add:

- `Telnet Client`
- `Simple TCP/IP Services`

> This starts common TCP/IP services including the **echo** (port 7), **daytime** (port 13), and **quote of the day** (port 17) services.  
> Do **not** change Server Roles at this time.

Once installed, open the **Services panel** (find it under Tools) and scroll down to find **Simple TCP/IP Services**. If it is not started, start it using:

- The green arrow button, or
- The **More Actions** panel on the left, or
- Right-click → Start

---

## 📸 Screenshot — Add Roles and Features wizard

> *(Add your screenshot of adding Telnet Client and Simple TCP/IP Services here)*

---

**Activity 4 — Test with telnet**

Open **Command Prompt** from the Start menu: **Windows System → Command Prompt**.

> Use Command Prompt, not PowerShell — this exercise works better with Command Prompt so you can see the telnet output.

```cmd
telnet localhost 13
telnet localhost 17
```

Port 13 returns the current **daytime**. Port 17 returns a **quote of the day**.

---

**Activity 5 — Test from your host workstation**

From your host workstation, try connecting to the server's IP address:

```cmd
telnet <server-ip> 13
telnet <server-ip> 17
```

📓 **Journal question:** Can you connect from the host workstation? Why or why not? (Hint: think about the Windows Firewall.)

---

## 📸 Screenshot — telnet test output

> *(Add your screenshot of the telnet output here)*

---

## Task 3: Command Line

Virtually all Windows Server configuration can be done from the command line. Key tools include:

| Command | Purpose |
| --- | --- |
| `net` | Manage local server — users, services, shares |
| `netdom` | Manage domain settings |
| `netsh` | Manage local network configuration |

Get help on any `net` command:

```cmd
net help <command>
```

---

### Activity 1 — View and control services

List all currently running services:

```cmd
net start
```

📓 **Journal:** Is **Simple TCP/IP Services** in the list?

Stop the service:

```cmd
net stop "Simple TCP/IP Service"
```

Test that it is no longer reachable — try `telnet localhost 13` again. Does it connect?

Restart the service:

```cmd
net start "Simple TCP/IP Service"
```

---

### Activity 2 — Allow a port through the firewall

Allow the **Quote of the Day** (port 17) through the Windows Firewall. Enter the following as a **single line**:

```cmd
netsh advfirewall firewall add rule name="TCP Port 17" dir=in action=allow protocol=TCP localport=17
```

Now try telnetting to port 17 from your host workstation again:

```cmd
telnet <server-ip> 17
```

📓 **Journal:** Did it work this time? What did the firewall rule change?

Use the internet to explore the full syntax of `netsh advfirewall` commands.

---

## 📸 Screenshot — Firewall rule and telnet from host

> *(Add your screenshot of the successful telnet from the host workstation here)*

---

## Task 4: Documentation

Windows Server has excellent built-in documentation:

- Use the **Help** menu inside Server Manager
- Server Manager links directly to Microsoft documentation
- Internet search is also your friend

**Internet Explorer Enhanced Security Configuration**

You may notice Internet Explorer has **Enhanced Security Configuration** enabled by default. When browsing, you may get prompts to add sites to the trusted list. Generally you can cancel these prompts — they are usually blocking ActiveX, Flash, or popups.

If IE seems to block too much, you can adjust the Enhanced Security Configuration settings — opening a fresh IE window will show you links explaining how to do this.

📓 **Journal:** Note anything you find interesting, and record any changes you make to the IE Enhanced Security Configuration.

---

## Task 5: Shutdown

Shut down your VM via the **Start Menu**.

> **Note:** Windows Server requires you to enter a **reason for shutdown**. This is good practice — it creates an audit trail so future admins can understand why the server was shut down.

Choose a topic (e.g. `Other (Planned)`) and enter a brief descriptive reason before shutting down.

---

## 📸 Screenshot — Windows Server shutdown dialog

> *(Add your screenshot of the shutdown reason dialog here)*

---

## 💡 Tips

| Tip | Details |
| --- | --- |
| Login shortcut inside VM | Press `Ctrl+Alt+Insert` instead of `Ctrl+Alt+Delete` |
| Suspend VM | VMware menu: **VM → Power → Suspend** — saves memory snapshot to `*.vmem` and `*.vmss` |
| Command help | `net help <command>` for syntax of any `net` subcommand |
| Find Services panel | Server Manager → Tools → Services |
| Find Device Manager | Server Manager → Tools → Computer Management → Device Manager |

---

## 💡 Command Reference

| Command | What it does |
| --- | --- |
| `net start` | List all currently running services |
| `net start "<service name>"` | Start a named service |
| `net stop "<service name>"` | Stop a named service |
| `net help <command>` | Show syntax and help for a net subcommand |
| `netsh advfirewall firewall add rule ...` | Add a Windows Firewall inbound rule |
| `telnet <host> <port>` | Test TCP connectivity to a host and port |

---

*Lab 1c · Windows Server Initial Configuration*
