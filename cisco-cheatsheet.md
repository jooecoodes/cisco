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

| To do this...                       | Type this...                 | Until you see...                                 |
| :---------------------------------- | :--------------------------- | :----------------------------------------------- |
| Enter Privileged (Admin) mode       | `enable`                     | `Router#` or `Switch#`                           |
| Enter Global Configuration mode     | `configure terminal`         | `Router(config)#` or `Switch(config)#`           |
| Enter Interface mode                | `interface fastEthernet 0/1` | `Router(config-if)#` or `Switch(config-if)#`     |
| Enter Console Line mode             | `line console 0`             | `Router(config-line)#` or `Switch(config-line)#` |
| Enter VTY Line mode (remote access) | `line vty 0 4`               | `Router(config-line)#` or `Switch(config-line)#` |
| Go back ONE level                   | `exit`                       | Previous mode                                    |
| Go back to Privileged mode          | `end`                        | `Router#` or `Switch#`                           |

---

## 🔷 ROUTER COMMANDS (In Order)

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
description LAN Connection
exit
interface fastEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
description LAN Connection
exit
end
copy running-config startup-config
```

### Router Command Reference Table

| Command                                | What it does                                   |
| :------------------------------------- | :--------------------------------------------- |
| `enable`                               | Enter admin mode                               |
| `configure terminal`                   | Enter configuration mode                       |
| `hostname R1`                          | Rename the router to "R1"                      |
| `enable secret class`                  | Set admin password to "class"                  |
| `line console 0`                       | Configure the physical console port            |
| `password cisco`                       | Set console password to "cisco"                |
| `login`                                | Enforce the password                           |
| `line vty 0 4`                         | Configure remote access (Telnet/SSH) lines 0-4 |
| `password cisco`                       | Set remote access password to "cisco"          |
| `login`                                | Enforce the password                           |
| `banner motd #Your Message#`           | Display warning message before login           |
| `interface fastEthernet 0/0`           | Configure port 0/0                             |
| `ip address 192.168.1.1 255.255.255.0` | Assign IP and subnet mask to the port          |
| `no shutdown`                          | Turn the port ON                               |
| `description LAN Connection`           | Add a note to the port                         |
| `end`                                  | Exit to Privileged mode                        |
| `copy running-config startup-config`   | **SAVE** the configuration                     |

---

## 🔷 SWITCH COMMANDS (In Order)

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
interface fastEthernet 0/1
no shutdown
exit
interface fastEthernet 0/2
no shutdown
exit
end
copy running-config startup-config
```

### Switch Command Reference Table

| Command                                | What it does                             |
| :------------------------------------- | :--------------------------------------- |
| `enable`                               | Enter admin mode                         |
| `configure terminal`                   | Enter configuration mode                 |
| `hostname SW1`                         | Rename the switch to "SW1"               |
| `enable secret class`                  | Set admin password to "class"            |
| `line console 0`                       | Configure the physical console port      |
| `password cisco`                       | Set console password to "cisco"          |
| `login`                                | Enforce the password                     |
| `line vty 0 4`                         | Configure remote access lines 0-4        |
| `password cisco`                       | Set remote access password to "cisco"    |
| `login`                                | Enforce the password                     |
| `service password-encryption`          | Hide all passwords in the config file    |
| `banner motd #Your Message#`           | Display warning message before login     |
| `interface vlan 1`                     | Configure the management interface (SVI) |
| `ip address 192.168.1.2 255.255.255.0` | Assign management IP                     |
| `no shutdown`                          | Turn the management interface ON         |
| `ip default-gateway 192.168.1.1`       | Set the switch's default gateway         |
| `interface fastEthernet 0/1`           | Configure a physical port                |
| `no shutdown`                          | Turn the port ON                         |
| `end`                                  | Exit to Privileged mode                  |
| `copy running-config startup-config`   | **SAVE** the configuration               |

---

## 📊 IP Address Planning

### For LAN A (Behind Switch 1)

| Device                     | IP Address   | Subnet Mask   | Default Gateway |
| :------------------------- | :----------- | :------------ | :-------------- |
| Router Fa0/0               | 192.168.1.1  | 255.255.255.0 | N/A             |
| Switch Management (VLAN 1) | 192.168.1.2  | 255.255.255.0 | 192.168.1.1     |
| PC0                        | 192.168.1.10 | 255.255.255.0 | 192.168.1.1     |
| PC1                        | 192.168.1.11 | 255.255.255.0 | 192.168.1.1     |
| PC2                        | 192.168.1.12 | 255.255.255.0 | 192.168.1.1     |
| PC3                        | 192.168.1.13 | 255.255.255.0 | 192.168.1.1     |
| PC4                        | 192.168.1.14 | 255.255.255.0 | 192.168.1.1     |

### For LAN B (Behind Switch 2)

| Device                     | IP Address   | Subnet Mask   | Default Gateway |
| :------------------------- | :----------- | :------------ | :-------------- |
| Router Fa0/1               | 192.168.2.1  | 255.255.255.0 | N/A             |
| Switch Management (VLAN 1) | 192.168.2.2  | 255.255.255.0 | 192.168.2.1     |
| PC5                        | 192.168.2.10 | 255.255.255.0 | 192.168.2.1     |
| PC6                        | 192.168.2.11 | 255.255.255.0 | 192.168.2.1     |
| PC7                        | 192.168.2.12 | 255.255.255.0 | 192.168.2.1     |

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
| `show mac address-table`  | View MAC address table (switches)    |
| `ping <IP address>`       | Test connectivity to another device  |

---

## 🔌 Cable Reference

| Connection                  | Cable Type              |
| :-------------------------- | :---------------------- |
| PC to Switch                | Copper Straight-Through |
| Switch to Router            | Copper Straight-Through |
| PC to Router                | Copper Straight-Through |
| Switch to Switch            | Copper Crossover        |
| Router to Router            | Copper Crossover        |
| PC to PC                    | Copper Crossover        |
| PC to Switch/Router Console | Console (Blue)          |

---

## 🧪 Testing Checklist

| Test              | From | To       | Command             | Expected                  |
| :---------------- | :--- | :------- | :------------------ | :------------------------ |
| Same LAN          | PC0  | PC1      | `ping 192.168.1.11` | ✅ Replies                 |
| Cross LAN         | PC0  | PC5      | `ping 192.168.2.10` | ✅ Replies (router works!) |
| Router Gateway    | PC0  | R1 Fa0/0 | `ping 192.168.1.1`  | ✅ Replies                 |
| Switch Management | PC0  | SW1      | `ping 192.168.1.2`  | ✅ Replies                 |

---

## 🚨 Common Errors and Fixes

| Error                                    | What it means                                       | How to fix                                                              |
| :--------------------------------------- | :-------------------------------------------------- | :---------------------------------------------------------------------- |
| `% Invalid input detected at '^' marker` | You're in the wrong mode or typed the wrong command | Check your prompt! Are you in `(config)#` mode?                         |
| `% Bad secrets`                          | You typed the wrong password                        | Try `cisco` or `class` depending on the prompt                          |
| `% Incomplete command`                   | You missed something in the command                 | Check the syntax (e.g., forgot the subnet mask)                         |
| `% Ambiguous command`                    | You typed too few letters                           | Type more of the command (e.g., `copy run start` instead of `copy r s`) |
| `Cannot ping`                            | Network issue                                       | Check cables, IP addresses, and `no shutdown`                           |
| `Administratively down`                  | Port is turned off                                  | Type `no shutdown` on the interface                                     |
| `Destination filename [startup-config]?` | Saving the config                                   | Just press **Enter**                                                    |

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

---

## 📂 Files to Save

| File Type             | Extension | What it contains                        |
| :-------------------- | :-------- | :-------------------------------------- |
| Packet Tracer Project | `.pkt`    | Entire network topology + saved configs |
| This Cheat Sheet      | `.md`     | All commands and notes                  |
