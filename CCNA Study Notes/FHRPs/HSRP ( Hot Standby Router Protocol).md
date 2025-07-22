#HSRP #FHRP #Cisco 

![[IMG_9716.jpeg]]

### In-Depth Guide to HSRP Operations

**CCNA Exam Objective:** 3.5 (Configure and verify first-hop redundancy protocols, such as HSRP)

The **Hot Standby Router Protocol (HSRP)** is Cisco's flagship solution for providing redundant default gateways. It establishes a framework of active and standby routers that work together to create a single, highly available virtual gateway, ensuring that network access is never interrupted by the failure of a single device.

***

### Core Concepts & Communication 📡

At its heart, HSRP creates a **virtual router** by assigning a shared **virtual IP** and **virtual MAC address** to a group of physical routers.

* **Communication:** Routers in an HSRP group communicate using **Hello packets**, which are sent to a specific multicast address. This allows the routers to elect Active and Standby roles and to detect when a failure has occurred.
* **Active Router:** The router with the highest **priority** becomes the Active router and is responsible for forwarding all traffic sent to the virtual IP.
* **Standby Router:** The router with the second-highest priority becomes the Standby router. It listens for Hellos from the Active router and takes over if those Hellos stop.
* **Virtual MAC Address:** HSRP uses a special, well-known virtual MAC address. This allows switches to correctly forward traffic to the physical Active router without needing to relearn MAC addresses during a failover. The format is `0000.0C07.ACxx`, where `xx` is the HSRP group number in hexadecimal.

---

### HSRP Versions: v1 vs. v2

HSRP has two versions with important differences. HSRPv2 is the modern standard.

| Feature | HSRP Version 1 | HSRP Version 2 |
| :--- | :--- | :--- |
| **Group Numbers** | 0 - 255 | **0 - 4095** |
| **Multicast Address** | `224.0.0.2` | **`224.0.0.102`** |
| **Virtual MAC Address**| `0000.0C07.ACxx` | **`0000.0C9F.Fxxx`** |
| **Authentication** | Plaintext only | Plaintext and **MD5** |

---

### HSRP States 🚦

During its operation, an HSRP router progresses through several states.

| State | Description |
| :--- | :--- |
| **Disabled** | The HSRP process is not running, often because the interface is shut down. |
| **Init** | The router is starting up or has just been configured for HSRP. |
| **Listen** | The router knows the virtual IP but is neither the Active nor Standby router. It simply listens for Hellos. |
| **Speak** | The router is sending Hellos and actively participating in the election process. |
| **Standby** | The router has lost the election and is now the backup, ready to take over if the Active router fails. |
| **Active** | The router won the election and is actively forwarding traffic for the virtual IP address. |

---

### HSRP CLI Command Cheat Sheet ⚙️

| Command | Purpose |
| :--- | :--- |
| `standby version [1 \| 2]` | Sets the HSRP version. Best practice is to use version 2. |
| `standby [group] ip [virtual-ip]` | **Enables HSRP.** Assigns the interface to a group and defines the virtual IP. |
| `standby [group] priority [value]` | Sets the priority (0-255, default 100) to influence the Active router election. |
| `standby [group] preempt` | Allows a higher-priority router to forcibly take the Active role. |
| `standby [group] track [object]` | Links HSRP to an object (like an interface's line-protocol state) to trigger a failover if an upstream link fails. |
| `standby [group] authentication md5 key-string [key]` | Secures HSRP messages with MD5 authentication. |
| `show standby [brief]` | **The primary verification command.** Shows the state, virtual IP, timers, priority, and active/standby router IPs. |
| `debug standby packets` | Provides real-time debugging output for HSRP hello packets. |

#### **Example Configuration (HSRPv2)**

```cisco
! Configuration for the primary router (R1)
interface GigabitEthernet0/1
 ip address 192.168.1.2 255.255.255.0
 standby version 2
 standby 1 ip 192.168.1.1
 standby 1 priority 110
 standby 1 preempt
 standby 1 authentication md5 key-string MySecureKey

! Configuration for the standby router (R2)
interface GigabitEthernet0/1
 ip address 192.168.1.3 255.255.255.0
 standby version 2
 standby 1 ip 192.168.1.1
 standby 1 priority 100
 standby 1 preempt
 standby 1 authentication md5 key-string MySecureKey
