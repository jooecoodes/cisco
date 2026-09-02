# 🖥️ Cisco Packet Tracer - The Ultimate Beginner Cheat Sheet

Welcome to your complete command reference! This guide covers everything from turning on a router to securing it with SSH.

---

## 🔑 The Golden Rules

| Rule                    | Explanation                                                                                             |
| :---------------------- | :------------------------------------------------------------------------------------------------------ |
| **Invisible Passwords** | When typing passwords, **nothing appears on screen**. No dots, no asterisks. Just type and press Enter. |
| **`cisco` vs `class`**  | `cisco` = Front door password (console/VTY). `class` = Manager's password (Enable mode).                |
| **Save or Lose it**     | Always type `copy running-config startup-config` before closing.                                        |
| **Check your Prompt**   | `>` = User mode, `#` = Admin mode, `(config)#` = Settings mode.                                         |

---

## 🔄 Mode Navigation (The Command Ladder)

| To do this...                    | Type this...                         | Prompt changes to...       |
| :------------------------------- | :----------------------------------- | :------------------------- |
| Enter Admin mode                 | `enable`                             | `Router#`                  |
| Enter Settings menu              | `configure terminal`                 | `Router(config)#`          |
| Configure a physical port        | `interface fastEthernet 0/1`         | `Router(config-if)#`       |
| Configure multiple ports         | `interface range fastEthernet 0/1-5` | `Router(config-if-range)#` |
| Configure the Switch's "Brain"   | `interface vlan 1`                   | `Switch(config-if)#`       |
| Configure Console Port           | `line console 0`                     | `Router(config-line)#`     |
| Configure Remote Access          | `line vty 0 4`                       | `Router(config-line)#`     |
| Go back ONE level                | `exit`                               | Previous mode              |
| Go back to Admin mode (Shortcut) | `end`                                | `Router#`                  |

---

## 🔷 BASIC ROUTER CONFIGURATION (Telnet - Quick Lab Setup)

**Note:** This version uses Telnet (insecure) and is great for quick Packet Tracer labs.

```
enable
configure terminal
hostname R1
enable secret class
service password-encryption
line console 0
password cisco
login
exit
line vty 0 4
password cisco
login
exit
banner motd #Authorized Access Only#
interface fastEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
description Uplink to LAN A
exit
interface fastEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
description Uplink to LAN B
exit
end
copy running-config startup-config
```

---

## 🔷 BASIC SWITCH CONFIGURATION (Telnet - Quick Lab Setup)

**Pro Tip:** For LAN A (5 PCs) use `interface range fastEthernet 0/1-5`. For LAN B (3 PCs) use `interface range fastEthernet 0/1-3`.

```
enable
configure terminal
hostname SW1
enable secret class
service password-encryption
line console 0
password cisco
login
exit
line vty 0 4
password cisco
login
exit
banner motd #Authorized Access Only#
interface vlan 1
ip address 192.168.1.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.1
interface range fastEthernet 0/1-5
no shutdown
exit
end
copy running-config startup-config
```

---

## 🔐 ADVANCED SSH CONFIGURATION (Secure - Real World Standard)

**Why SSH?** Telnet sends passwords in plain text. SSH encrypts everything. Hackers hate SSH.

### Router SSH Setup
```
enable
configure terminal
hostname R1
service password-encryption
ip domain-name mylab.local
crypto key generate rsa
username admin secret cisco
ip ssh version 2
line console 0
password cisco
login
exit
line vty 0 4
transport input ssh
login local
exit
banner motd #Authorized Access Only#
interface fastEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
description Uplink to LAN A
exit
interface fastEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
description Uplink to LAN B
exit
end
copy running-config startup-config
```

### Switch SSH Setup
```
enable
configure terminal
hostname SW1
service password-encryption
ip domain-name mylab.local
crypto key generate rsa
username admin secret cisco
ip ssh version 2
line console 0
password cisco
login
exit
line vty 0 4
transport input ssh
login local
exit
banner motd #Authorized Access Only#
interface vlan 1
ip address 192.168.1.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.1
interface range fastEthernet 0/1-5
no shutdown
exit
end
copy running-config startup-config
```

### ⚠️ SSH "Gotchas"

| Step  | Action                                                                                                                                            | Why it matters |
| :---- | :------------------------------------------------------------------------------------------------------------------------------------------------ | :------------- |
| **1** | When generating the RSA key (`crypto key generate rsa`), type **`1024`** when asked for bits. If you press Enter, it defaults to 512 (too weak!). |                |
| **2** | SSH uses `login local`, NOT `login`. Delete the old `password cisco` from the VTY lines.                                                          |                |
| **3** | To test SSH from a PC: `ssh -l admin 192.168.1.2` (Password is `cisco`).                                                                          |                |

---

## 🔌 THE WIRING MAP (How to connect everything)

### PC to Switch Connections

| PC                      | Connect to Switch Port             | Cable Type              |
| :---------------------- | :--------------------------------- | :---------------------- |
| PC0, PC1, PC2, PC3, PC4 | SW1 → Ports **Fa0/1** to **Fa0/5** | Copper Straight-Through |
| PC5, PC6, PC7           | SW2 → Ports **Fa0/1** to **Fa0/3** | Copper Straight-Through |

### Switch to Router Connections (Uplinks)

| Connection                                             | Cable Type              |
| :----------------------------------------------------- | :---------------------- |
| SW1 (Port **Gig0/1** or **Fa0/24**) → Router **Fa0/0** | Copper Straight-Through |
| SW2 (Port **Gig0/1** or **Fa0/24**) → Router **Fa0/1** | Copper Straight-Through |

### Console Access (For testing passwords)

| Connection                                              | Cable Type           |
| :------------------------------------------------------ | :------------------- |
| PC (Port **RS-232**) → Router/Switch (Port **Console**) | Console (Light Blue) |

---

## 📊 IP ADDRESS PLANNING (Your 8-PC Network)

### LAN A (Network: 192.168.1.0 / Subnet: 255.255.255.0)

| Device                  | IP Address   | Default Gateway |
| :---------------------- | :----------- | :-------------- |
| Router Fa0/0            | 192.168.1.1  | N/A             |
| SW1 Management (VLAN 1) | 192.168.1.2  | 192.168.1.1     |
| PC0                     | 192.168.1.10 | 192.168.1.1     |
| PC1                     | 192.168.1.11 | 192.168.1.1     |
| PC2                     | 192.168.1.12 | 192.168.1.1     |
| PC3                     | 192.168.1.13 | 192.168.1.1     |
| PC4                     | 192.168.1.14 | 192.168.1.1     |

### LAN B (Network: 192.168.2.0 / Subnet: 255.255.255.0)

| Device                  | IP Address   | Default Gateway |
| :---------------------- | :----------- | :-------------- |
| Router Fa0/1            | 192.168.2.1  | N/A             |
| SW2 Management (VLAN 1) | 192.168.2.2  | 192.168.2.1     |
| PC5                     | 192.168.2.10 | 192.168.2.1     |
| PC6                     | 192.168.2.11 | 192.168.2.1     |
| PC7                     | 192.168.2.12 | 192.168.2.1     |

---

## 🛠️ USEFUL VERIFICATION COMMANDS

| Command                   | What it shows you                                        |
| :------------------------ | :------------------------------------------------------- |
| `show running-config`     | The active config right now (RAM).                       |
| `show startup-config`     | The saved config (NVRAM).                                |
| `show ip interface brief` | All ports, their IPs, and if they are `up/up`.           |
| `show ip route`           | The routing table (how the router finds other networks). |
| `show vlan brief`         | VLANs and which ports are in them (Switches only).       |
| `show interfaces status`  | If the physical ports are connected or down.             |
| `show ip ssh`             | Shows SSH version and status (after SSH config).         |
| `show users`              | Shows who is logged in remotely (Telnet/SSH).            |

---

## 🧪 FINAL TESTING CHECKLIST

Run these pings from **PC0** to prove your network works:

| Test                        | Command                    | Why this matters                  |
| :-------------------------- | :------------------------- | :-------------------------------- |
| Same LAN A                  | `ping 192.168.1.11`        | SW1 is working.                   |
| Router Gateway              | `ping 192.168.1.1`         | Cable from SW1 to Router is good. |
| Switch Management           | `ping 192.168.1.2`         | SW1's "Brain" is reachable.       |
| **Cross LAN (The Big One)** | `ping 192.168.2.10`        | **ROUTER IS ROUTING!**            |
| Remote Switch               | `ping 192.168.2.2`         | Connection to SW2 is good.        |
| SSH Test (if configured)    | `ssh -l admin 192.168.1.2` | Remote security is working.       |

---

## 🚨 COMMON ERRORS & HOW TO FIX THEM

| Error Message                            | What it actually means                                    | The Fix                                                               |
| :--------------------------------------- | :-------------------------------------------------------- | :-------------------------------------------------------------------- |
| `% Invalid input detected at '^' marker` | You typed a command in the **wrong mode** or made a typo. | Look at your prompt! If it says `#`, type `configure terminal` first. |
| `% Bad secrets`                          | You typed the wrong password.                             | Type `cisco` for console/VTY; type `class` after `enable`.            |
| `Password:` (nothing shows up)           | **Normal behavior!** Cisco hides passwords.               | Just type the password and press Enter.                               |
| `% Connection refused` (Telnet)          | You configured SSH but tried Telnet.                      | Use `ssh -l admin <IP>` instead of `telnet`.                          |
| `Destination filename [startup-config]?` | Saving the config.                                        | Just press **Enter**.                                                 |
| `administratively down`                  | The port is turned off.                                   | Go to that interface and type `no shutdown`.                          |
| `Request timed out` (First ping)         | Normal! The PC is doing an ARP request.                   | Wait for ping #2, #3, #4, #5—they will reply!                         |

---

## 💾 HOW TO SAVE YOUR WORK

You have to save TWO things!

| Step                      | What to do                                | Where                         |
| :------------------------ | :---------------------------------------- | :---------------------------- |
| **1. Save Device Config** | Type `copy running-config startup-config` | On Router, SW1, and SW2 CLIs. |
| **2. Save Project File**  | Press `Ctrl + S` or go to File > Save.    | In Packet Tracer main window. |

---

## 🧠 BONUS: SSH vs Telnet Comparison

| Feature           | Telnet                                      | SSH                                                                 |
| :---------------- | :------------------------------------------ | :------------------------------------------------------------------ |
| **Security**      | ❌ Plain Text (Hackers can read passwords)   | ✅ Encrypted (Hackers see gibberish)                                 |
| **Port Used**     | Port 23                                     | Port 22                                                             |
| **Configuration** | `line vty 0 4` + `password cisco` + `login` | `line vty 0 4` + `transport input ssh` + `login local` + `username` |
| **When to use**   | Labs and practice only.                     | **Real World / Production.**                                        |

---

**Congratulations!** You now have a complete reference that takes you from a blank screen to a fully secured, multi-LAN network. Keep this file handy, and happy networking! 🚀
