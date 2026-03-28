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

### Step 1 — Boot the VM and reach the login screen

Windows Server will boot to a GUI showing a lock screen with a prompt to press Ctrl+Alt+Delete.

> 📸 *Screenshot — Windows Server lock screen / Ctrl+Alt+Delete prompt*
> ![Windows Server lock screen](./screenshots/1c-01-lock-screen.png)

---

### Step 2 — Send Ctrl+Alt+Delete to the VM

Your host machine may intercept **Ctrl+Alt+Delete** before it reaches the VM. Use one of these alternatives:

- Press **Ctrl+Alt+Insert** on your keyboard, or
- Use the VMware menu: **VM → Send Ctrl+Alt+Delete**

> 📸 *Screenshot — VMware VM menu showing Send Ctrl+Alt+Delete option*
> ![VMware VM menu](./screenshots/1c-02-vmware-menu-cad.png)

---

### Step 3 — Log in as Administrator

Enter the Administrator password:

```
student123!
```

> 📸 *Screenshot — Windows Server Administrator password entry screen*
> ![Administrator login](./screenshots/1c-03-admin-login.png)

---

### Step 4 — Server Manager Dashboard

You should now see the desktop and the **Server Manager → Dashboard** panel.

> 📸 *Screenshot — Server Manager Dashboard*
> ![Server Manager Dashboard](./screenshots/1c-04-server-manager-dashboard.png)

---

### Step 5 — Open Local Server properties

Click **Local Server** in the left panel of Server Manager.

> 📸 *Screenshot — Local Server panel in Server Manager*
> ![Local Server panel](./screenshots/1c-05-local-server.png)

---

### Step 6 — Check and set the time zone

Find the **Time Zone** field. If it is not set to `(UTC+10:00) Canberra, Melbourne, Sydney`, click it to change it.

> 📸 *Screenshot — Time Zone setting in Local Server*
> ![Time zone setting](./screenshots/1c-06-timezone.png)

> 📸 *Screenshot — Time Zone change dialog with UTC+10 selected*
> ![Time zone change dialog](./screenshots/1c-07-timezone-dialog.png)

---

### Step 7 — Check network configuration

Find the **Ethernet** entries. Both Ethernet0 and Ethernet1 should show `IPv4 address assigned by DHCP, IPv6 enabled`.

Click the link to open the **Network Connections** page and view detailed configuration.

> 📸 *Screenshot — Network entries in Local Server showing DHCP*
> ![Network DHCP entries](./screenshots/1c-08-network-dhcp.png)

> 📸 *Screenshot — Network Connections detail page*
> ![Network Connections page](./screenshots/1c-09-network-connections.png)

📓 **Journal:** Record the IP address, subnet mask, and default gateway for both Ethernet adapters.

---

### Step 8 — Change the computer name

Click the current **Computer Name** in Local Server. Change it to something descriptive (e.g. `chrisserver`).

Leave the workgroup as `WORKGROUP` for now.

> 📸 *Screenshot — System Properties dialog for changing computer name*
> ![System Properties computer name](./screenshots/1c-10-computer-name.png)

> 📸 *Screenshot — Confirmation prompt to reboot after renaming*
> ![Reboot prompt after rename](./screenshots/1c-11-rename-reboot-prompt.png)

You will be prompted to reboot. You can reboot now or after you finish exploring — but do it before proceeding to Task 2.

---

### Step 9 — Check Windows Update settings

Scroll down in Local Server to find the **Windows Update** section. Note:

- When updates were last installed
- When updates were last checked
- How Windows Update is configured (automatic vs manual)

> 📸 *Screenshot — Windows Update section in Local Server*
> ![Windows Update settings](./screenshots/1c-12-windows-update.png)

📓 **Journal:** Record the update settings. Why is it important that a server does **not** reboot itself automatically to install updates?

---

### Step 10 — View events, services, and logs

Scroll further down in Local Server to view the **Events**, **Services**, and **Performance** sections.

> 📸 *Screenshot — Events and Services sections in Local Server (scrolled down)*
> ![Events and Services](./screenshots/1c-13-events-services.png)

📓 **Journal:** Record all the details you changed and anything else you find noteworthy.

Reboot your server now if you have not done so already after changing the computer name.

---

## Task 2: Server Management

The Server Manager is the main console for managing Windows Server. It allows you to:

1. View and update the **server summary** — computer info, security settings
2. View and update **server roles**
3. View and update **server features**
4. Access **common resources**, support links, and the **Best Practices Analyzer**

The **Tools** menu at the top provides access to additional management tools — Diagnostics, Configuration, Storage, and more.

---

### Activity 1 — Explore Device Manager

Open **Device Manager** via: Tools → Computer Management → Device Manager (or search for it).

> 📸 *Screenshot — Device Manager showing installed hardware*
> ![Device Manager](./screenshots/1c-14-device-manager.png)

📓 **Journal:**
- What hardware is installed on this Windows Server VM?
- Is it similar to the CentOS VM hardware?
- Record all devices listed.

---

### Activity 2 — Find your network IP address

Open the **Network Connections** page (via Local Server → Ethernet link, or via Tools → Computer Management).

> 📸 *Screenshot — Network connection details showing IP address*
> ![Network IP address](./screenshots/1c-15-network-ip.png)

📓 **Journal:**
- What is the IP address connecting this server to the internet?
- Compare it against your CentOS server — are they on the same subnet?
- What is the subnet mask and default gateway?

---

### Activity 3 — Add Telnet Client and Simple TCP/IP Services features

Navigate to **Manage → Add Roles and Features**.

> 📸 *Screenshot — Add Roles and Features Wizard opening page*
> ![Add Roles and Features wizard](./screenshots/1c-16-add-features-wizard.png)

Click through to the **Features** page (not Roles). Find and tick:

- `Telnet Client`
- `Simple TCP/IP Services`

> 📸 *Screenshot — Features page with Telnet Client ticked*
> ![Telnet Client feature ticked](./screenshots/1c-17-telnet-client.png)

> 📸 *Screenshot — Features page with Simple TCP/IP Services ticked*
> ![Simple TCP/IP Services ticked](./screenshots/1c-18-simple-tcpip.png)

Click **Install** and wait for installation to complete.

> 📸 *Screenshot — Installation progress / completion screen*
> ![Feature installation complete](./screenshots/1c-19-install-complete.png)

> ⚠️ Do **not** change Server Roles at this time.

---

### Activity 3 (continued) — Verify the service is running

Open the **Services panel**: Tools → Services.

Scroll down to find **Simple TCP/IP Services**. If it is not started, start it using one of:

- The green **Start** arrow in the toolbar
- The **More Actions** panel on the left
- Right-click → **Start**

> 📸 *Screenshot — Services panel with Simple TCP/IP Services visible*
> ![Services panel Simple TCP/IP](./screenshots/1c-20-services-panel.png)

> 📸 *Screenshot — Simple TCP/IP Services showing Running status*
> ![Simple TCP/IP Services running](./screenshots/1c-21-tcpip-running.png)

---

### Activity 4 — Test with telnet from the server itself

Open **Command Prompt** from Start Menu → **Windows System → Command Prompt**.

> Use Command Prompt, not PowerShell — the output of telnet is clearer in Command Prompt.

```cmd
telnet localhost 13
```

Port 13 = **Daytime** — returns the current date and time.

> 📸 *Screenshot — Command Prompt showing telnet localhost 13 output*
> ![telnet localhost 13](./screenshots/1c-22-telnet-13.png)

```cmd
telnet localhost 17
```

Port 17 = **Quote of the Day** — returns a random quote.

> 📸 *Screenshot — Command Prompt showing telnet localhost 17 output*
> ![telnet localhost 17](./screenshots/1c-23-telnet-17.png)

---

### Activity 5 — Test from your host workstation

From your **host workstation** (not the VM), open a command prompt and try:

```cmd
telnet <server-ip> 13
telnet <server-ip> 17
```

> 📸 *Screenshot — Host workstation command prompt showing telnet attempt to server*
> ![telnet from host to server](./screenshots/1c-24-telnet-from-host.png)

📓 **Journal question:** Can you connect from the host workstation? Why or why not? (Hint: think about the Windows Firewall.)

---

## Task 3: Command Line

Virtually all Windows Server configuration can be done from the command line.

| Command | Purpose |
| --- | --- |
| `net` | Manage local server — users, services, shares |
| `netdom` | Manage domain settings |
| `netsh` | Manage local network configuration |

Get help on any `net` subcommand:

```cmd
net help <command>
```

---

### Activity 1 — View running services

```cmd
net start
```

> 📸 *Screenshot — Command Prompt showing net start output listing all running services*
> ![net start output](./screenshots/1c-25-net-start.png)

📓 **Journal:** Is **Simple TCP/IP Services** listed in the output?

---

### Stop the service and verify

```cmd
net stop "Simple TCP/IP Service"
```

> 📸 *Screenshot — Command Prompt showing net stop output*
> ![net stop Simple TCP/IP](./screenshots/1c-26-net-stop.png)

Now test that port 13 is no longer reachable:

```cmd
telnet localhost 13
```

> 📸 *Screenshot — telnet failing after service is stopped*
> ![telnet fails after stop](./screenshots/1c-27-telnet-fail.png)

---

### Restart the service

```cmd
net start "Simple TCP/IP Service"
```

> 📸 *Screenshot — Command Prompt showing net start (restart) output*
> ![net start restart](./screenshots/1c-28-net-start-restart.png)

Verify it is working again with `telnet localhost 13`.

---

### Activity 2 — Allow a port through the Windows Firewall

Allow the **Quote of the Day** (port 17) through the firewall. Enter the entire command below as a **single line**:

```cmd
netsh advfirewall firewall add rule name="TCP Port 17" dir=in action=allow protocol=TCP localport=17
```

> 📸 *Screenshot — Command Prompt showing netsh advfirewall command and confirmation*
> ![netsh firewall rule added](./screenshots/1c-29-netsh-firewall.png)

Now test from your host workstation again:

```cmd
telnet <server-ip> 17
```

> 📸 *Screenshot — Host workstation successfully telnetting to port 17 after firewall rule added*
> ![telnet from host port 17 success](./screenshots/1c-30-telnet-host-port17.png)

📓 **Journal:** Did it work this time? What did the firewall rule change? Use the internet to explore the full syntax of `netsh advfirewall` commands.

---

## Task 4: Documentation

Windows Server has excellent built-in documentation:

- Use the **Help** menu inside Server Manager
- Server Manager links directly to Microsoft documentation

**Internet Explorer Enhanced Security Configuration**

Internet Explorer on Windows Server has Enhanced Security Configuration (IE ESC) enabled by default. When browsing, you may see prompts to add sites to the trusted zone — these are usually blocking ActiveX, Flash, or popups. You can generally cancel them.

If IE seems to block too much, you can adjust the Enhanced Security Configuration settings. Opening a fresh IE window will show you links explaining how.

> 📸 *Screenshot — Internet Explorer Enhanced Security Configuration prompt or settings*
> ![IE Enhanced Security](./screenshots/1c-31-ie-esc.png)

📓 **Journal:** Note what you find and record any changes you make to the IE Enhanced Security Configuration settings.

---

## Task 5: Shutdown

Shut down your VM via the **Start Menu**.

> 📸 *Screenshot — Start Menu with Shutdown option visible*
> ![Start Menu shutdown](./screenshots/1c-32-start-menu-shutdown.png)

---

Windows Server requires you to enter a **reason for shutdown** before it will proceed.

Choose a topic (e.g. `Other (Planned)`) and enter a brief descriptive reason.

> 📸 *Screenshot — Shutdown Event Tracker dialog with reason entered*
> ![Shutdown reason dialog](./screenshots/1c-33-shutdown-reason.png)

📓 **Journal:** This is good practice — it creates an audit trail so future admins can understand why the server was shut down.

---

## 💡 Tips

| Tip | Details |
| --- | --- |
| Login shortcut inside VM | Press `Ctrl+Alt+Insert` instead of `Ctrl+Alt+Delete` |
| Send Ctrl+Alt+Del via VMware | VMware menu: **VM → Send Ctrl+Alt+Delete** |
| Suspend VM (save state) | VMware menu: **VM → Power → Suspend** — saves to `*.vmem` and `*.vmss` |
| Get command help | `net help <command>` for syntax of any net subcommand |
| Find Services panel | Server Manager → Tools → Services |
| Find Device Manager | Server Manager → Tools → Computer Management → Device Manager |

---

## 💡 Command Reference

| Command | What it does |
| --- | --- |
| `net start` | List all currently running services |
| `net start "<service>"` | Start a named service |
| `net stop "<service>"` | Stop a named service |
| `net help <command>` | Show help and syntax for a net subcommand |
| `netsh advfirewall firewall add rule ...` | Add an inbound Windows Firewall rule |
| `telnet <host> <port>` | Test TCP connectivity to a host and port |

---

*Lab 1c · Windows Server Initial Configuration*
