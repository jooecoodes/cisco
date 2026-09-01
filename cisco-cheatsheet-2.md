# 🖥️ Cisco Packet Tracer - Beginner Command Cheat Sheet

## 🔑 The Golden Rules

| Rule                    | Explanation                                                                                                                   |
| :---------------------- | :---------------------------------------------------------------------------------------------------------------------------- |
| **Invisible Passwords** | When typing passwords, **nothing appears on screen**. No dots, no asterisks. Just type and press Enter.                       |
| **`cisco` vs `class`**  | `cisco` = Front door password (to get into the device). `class` = Manager's password (to make changes after typing `enable`). |
| **Save Your Work**      | Always type `copy running-config startup-config` and press Enter when you're done.                                            |
| **Check Your Mode**     | Look at the prompt: `>` = user mode, `#` = admin mode, `(config)#` = configuration mode.                                      |

---

## 🔄 Mode Navigation (Moving Between Prompts)

| To do this...                         | Type this...                 | Until you see...                                 |
| :------------------------------------ | :--------------------------- | :----------------------------------------------- |
| Enter Privileged (Admin) mode         | `enable`                     | `Router#` or `Switch#`                           |
| Enter Global Configuration mode       | `configure terminal`         | `Router(config)#` or `Switch(config)#`           |
| Enter Interface mode                  | `interface fastEthernet 0/1` | `Router(config-if)#` or `Switch(config-if)#`     |
| Enter Console Line mode               | `line console 0`             | `Router(config-line)#` or `Switch(config-line)#` |
| Enter VTY Line mode (remote access)   | `line vty 0 4`               | `Router(config-line)#` or `Switch(config-line)#` |
| Go back ONE level                     | `exit`                       | Previous mode                                    |
| Go back to Privileged mode (shortcut) | `end`                        | `Router#` or `Switch#`                           |

---

## 🔷 ROUTER COMMANDS (In Order)

**Note:** The router connects to **Switch 0** via `FastEthernet 0/0` and to **Switch 1** via `FastEthernet 0/1`.

```
enable
configure terminal
hostname R1
enable secret class
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
description Uplink to LAN A (Switch 0)
exit
interface fastEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
description Uplink to LAN B (Switch 1)
exit
end
copy running-config startup-config
```

---

## 🔷 SWITCH COMMANDS (In Order)

### For LAN A (SW1) - 5 PCs (Ports 1 to 5)

```
enable
configure terminal
hostname SW1
enable secret class
line console 0
password cisco
login
exit
line vty 0 4
password cisco
login
exit
service password-encryption
banner motd #Authorized Access Only#
interface vlan 1
ip address 192.168.1.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.1
! --- Configure the 5 ports for the 5 PCs ---
interface fastEthernet 0/1
no shutdown
exit
interface fastEthernet 0/2
no shutdown
exit
interface fastEthernet 0/3
no shutdown
exit
interface fastEthernet 0/4
no shutdown
exit
interface fastEthernet 0/5
no shutdown
exit
! --- Connect the Switch to the Router (usually use a high port like 0/24) ---
interface fastEthernet 0/24
no shutdown
exit
end
copy running-config startup-config
```

### For LAN B (SW2) - 3 PCs (Ports 1 to 3)

```
enable
configure terminal
hostname SW2
enable secret class
line console 0
password cisco
login
exit
line vty 0 4
password cisco
login
exit
service password-encryption
banner motd #Authorized Access Only#
interface vlan 1
ip address 192.168.2.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.2.1
! --- Configure the 3 ports for the 3 PCs ---
interface fastEthernet 0/1
no shutdown
exit
interface fastEthernet 0/2
no shutdown
exit
interface fastEthernet 0/3
no shutdown
exit
! --- Connect the Switch to the Router (usually use a high port like 0/24) ---
interface fastEthernet 0/24
no shutdown
exit
end
copy running-config startup-config
```

---

## 🔌 HOW TO CONNECT EVERYTHING (The Wiring Map)

### Step 1: Connect PCs to Switches

| PC              | Connect to Switch Port          | Cable Type              |
| :-------------- | :------------------------------ | :---------------------- |
| **PC0** (LAN A) | Switch 0 → **FastEthernet 0/1** | Copper Straight-Through |
| **PC1** (LAN A) | Switch 0 → **FastEthernet 0/2** | Copper Straight-Through |
| **PC2** (LAN A) | Switch 0 → **FastEthernet 0/3** | Copper Straight-Through |
| **PC3** (LAN A) | Switch 0 → **FastEthernet 0/4** | Copper Straight-Through |
| **PC4** (LAN A) | Switch 0 → **FastEthernet 0/5** | Copper Straight-Through |
| **PC5** (LAN B) | Switch 1 → **FastEthernet 0/1** | Copper Straight-Through |
| **PC6** (LAN B) | Switch 1 → **FastEthernet 0/2** | Copper Straight-Through |
| **PC7** (LAN B) | Switch 1 → **FastEthernet 0/3** | Copper Straight-Through |

### Step 2: Connect Switches to the Router

| Connection                                                            | Cable Type              |
| :-------------------------------------------------------------------- | :---------------------- |
| **Switch 0** (Port **0/24**) → **Router** (Port **FastEthernet 0/0**) | Copper Straight-Through |
| **Switch 1** (Port **0/24**) → **Router** (Port **FastEthernet 0/1**) | Copper Straight-Through |

> **Why Port 24?** We use ports `0/1` to `0/5` for the PCs. Port `0/24` is usually left open for the "uplink" to the router because it feels organized!

---

## 📊 IP ADDRESS PLANNING (For 5 PCs + 3 PCs)

### LAN A (192.168.1.0 / 24) - Connected to Router Fa0/0

| Device            | IP Address   | Subnet Mask   | Default Gateway |
| :---------------- | :----------- | :------------ | :-------------- |
| Router (Fa0/0)    | 192.168.1.1  | 255.255.255.0 | N/A             |
| Switch 0 (VLAN 1) | 192.168.1.2  | 255.255.255.0 | 192.168.1.1     |
| PC0               | 192.168.1.10 | 255.255.255.0 | 192.168.1.1     |
| PC1               | 192.168.1.11 | 255.255.255.0 | 192.168.1.1     |
| PC2               | 192.168.1.12 | 255.255.255.0 | 192.168.1.1     |
| PC3               | 192.168.1.13 | 255.255.255.0 | 192.168.1.1     |
| PC4               | 192.168.1.14 | 255.255.255.0 | 192.168.1.1     |

### LAN B (192.168.2.0 / 24) - Connected to Router Fa0/1

| Device            | IP Address   | Subnet Mask   | Default Gateway |
| :---------------- | :----------- | :------------ | :-------------- |
| Router (Fa0/1)    | 192.168.2.1  | 255.255.255.0 | N/A             |
| Switch 1 (VLAN 1) | 192.168.2.2  | 255.255.255.0 | 192.168.2.1     |
| PC5               | 192.168.2.10 | 255.255.255.0 | 192.168.2.1     |
| PC6               | 192.168.2.11 | 255.255.255.0 | 192.168.2.1     |
| PC7               | 192.168.2.12 | 255.255.255.0 | 192.168.2.1     |

---

## 🛠️ Useful Verification Commands

| Command                   | What it does                         |
| :------------------------ | :----------------------------------- |
| `show running-config`     | View the current configuration       |
| `show startup-config`     | View the saved configuration         |
| `show ip interface brief` | View all interfaces and their status |
| `show ip route`           | View the routing table               |
| `show vlan brief`         | View VLAN information (switches)     |
| `show interfaces status`  | View port status (switches)          |
| `ping <IP address>`       | Test connectivity to another device  |

---

## 🧪 Final Testing Checklist

| Test               | From | To  | Command             | Expected   |
| :----------------- | :--- | :-- | :------------------ | :--------- |
| Same LAN (A)       | PC0  | PC1 | `ping 192.168.1.11` | ✅ Replies  |
| Same LAN (B)       | PC5  | PC6 | `ping 192.168.2.11` | ✅ Replies  |
| Cross LAN (Router) | PC0  | PC5 | `ping 192.168.2.10` | ✅ Replies! |
| Switch Management  | PC0  | SW1 | `ping 192.168.1.2`  | ✅ Replies  |

---

## 🚨 Common Errors and Fixes

| Error                                    | What it means                                       | How to fix                                                  |
| :--------------------------------------- | :-------------------------------------------------- | :---------------------------------------------------------- |
| `% Invalid input detected at '^' marker` | You're in the wrong mode or typed the wrong command | Check your prompt! Are you in `(config)#` mode?             |
| `% Bad secrets`                          | You typed the wrong password                        | Try `cisco` or `class` depending on the prompt              |
| `Password:` and nothing shows up         | Normal behavior                                     | **Just type the password and press Enter** (invisible text) |
| `Administratively down`                  | Port is turned off                                  | Type `no shutdown` on the interface                         |
| `Destination filename [startup-config]?` | Saving the config                                   | Just press **Enter**                                        |

---

## 💾 How to Save Your Work

| Step | Action                                                    |
| :--- | :-------------------------------------------------------- |
| 1    | On each device, type `copy running-config startup-config` |
| 2    | Press **Enter** when it asks for the filename             |
| 3    | Wait for `[OK]` message                                   |
| 4    | In Packet Tracer, go to **File → Save As...**             |
| 5    | Name your file and save it as `.pkt`                      |

---

## 📝 Quick Password Reference

| Prompt Type                              | What to type |
| :--------------------------------------- | :----------- |
| Initial `Password:` (front door)         | **`cisco`**  |
| After typing `enable` (manager's office) | **`class`**  |

> **Remember**: You will NOT see any characters on screen when typing passwords. Just type and press Enter!
