#VRRP #FHRP 

![[IMG_9717.jpeg]]

![[IMG_9718.jpeg]]

### A Concise Guide to VRRP

**CCNA Exam Objective:** 3.5 (Configure and verify first-hop redundancy protocols, such as VRRP)

**Virtual Router Redundancy Protocol (VRRP)** is the IETF **open-standard** equivalent of HSRP. Its primary function is identical: to provide a single, redundant virtual gateway for hosts on a network, ensuring high availability. Its main advantage is interoperability in multi-vendor environments.

***

### Key Differentiators from HSRP 🤝

VRRP operates very similarly to HSRP but with a few critical distinctions.

| Feature | VRRP (Virtual Router Redundancy Protocol) | HSRP (Hot Standby Router Protocol) |
| :--- | :--- | :--- |
| **Standard** | **IETF Open Standard** (multi-vendor) | Cisco Proprietary |
| **Router Roles** | **Master** (Active) & **Backup** (Standby) | Active & Standby |
| **Virtual IP** | **Can be a real interface IP address** | Must be a separate, unique IP |
| **Preemption** | **Enabled by default** | Disabled by default |
| **Multicast Address**| `224.0.0.18` | `224.0.0.2` (v1) / `224.0.0.102` (v2) |

---

### CLI Command Cheat Sheet ⚙️

The command syntax is very similar to HSRP, making it easy to learn.

| Command | Purpose |
| :--- | :--- |
| `vrrp [group] ip [virtual-ip]` | Enables VRRP for a group and sets the virtual IP address. |
| `vrrp [group] priority [value]` | Sets the priority (1-254, default 100) to influence the Master election. |
| `vrrp [group] preempt` | Confirms preemption is active (it is on by default). |
| `show vrrp [brief]` | The primary verification command to check state, priority, and timers. |

#### **Example Configuration**

Let's make R1 the Master router for the virtual IP `192.168.1.1`.

**Configuration on R1 (Master):**
```cisco
interface GigabitEthernet0/1
 ! R1's real IP address
 ip address 192.168.1.2 255.255.255.0
 ! Enable VRRP group 1
 vrrp 1 ip 192.168.1.1
 ! Set a higher priority to ensure it becomes Master
 vrrp 1 priority 110

Configuration on R2 (Backup):
interface GigabitEthernet0/1
 ip address 192.168.1.3 255.255.255.0
 ! Join the same VRRP group
 vrrp 1 ip 192.168.1.1
 ! Priority is left at the default of 100

Because preemption is on by default, R1 will always become the Master router as long as it's online, thanks to its higher priority.

