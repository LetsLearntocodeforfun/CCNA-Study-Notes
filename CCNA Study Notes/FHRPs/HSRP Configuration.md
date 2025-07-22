#HSRP #Cisco #FHRP 

![[IMG_9720.jpeg]]

### HSRP Configuration & Command Guide

**CCNA Exam Objective:** 3.5 (Configure and verify first-hop redundancy protocols, such as HSRP)

This guide provides a concise and practical breakdown of the essential commands needed to configure, tune, and verify the Hot Standby Router Protocol (HSRP) on Cisco IOS devices.

***

### HSRP CLI Cheat Sheet ⚙️

| Command | Purpose |
| :--- | :--- |
| `standby version [1 \| 2]` | Sets the HSRP version (v2 is recommended). |
| `standby [group] ip [virtual-ip]` | **Enables HSRP** and defines the shared virtual IP address. |
| `standby [group] priority [value]` | Sets the priority (0-255) to influence the **Active router election**. Higher wins. |
| `standby [group] preempt` | Allows a higher-priority router to **forcibly take the Active role**. |
| `standby [group] track [object] [decrement]` | Links HSRP to an object (like an interface) to trigger a failover. |
| `show standby [brief]` | The **primary verification command** to check HSRP status. |

***

### Command Breakdown

#### `standby version [1 | 2]`
This command sets the HSRP version used by the router. It's a best practice to run **version 2** as it offers significant enhancements over version 1, including support for more groups (up to 4096), a different multicast address to avoid conflicts (`224.0.0.102`), and support for MD5 authentication. This must be set consistently across all routers in the group.

---

#### `standby [group] ip [virtual-ip]`
This is the core command that **enables the HSRP process**.
* **`[group]`**: A number from 0-4095 (for v2) that identifies the HSRP group. All routers participating in the same virtual gateway must use the same group number on that segment.
* **`[virtual-ip]`**: The shared IP address that all clients on the LAN will use as their default gateway. This IP address "floats" between the active and standby routers.

---

#### `standby [group] priority [value]`
This command is the primary tool for influencing the **Active/Standby election**.
* The router with the **highest priority value** (from 0 to 255) will be elected as the Active router.
* The default priority is **100**.
* By setting one router to a higher value (e.g., 110) and leaving the other at the default, you create a deterministic and predictable outcome for the election.

---

#### `standby [group] preempt`
This command allows a router to actively assert its right to become the Active router if its priority is higher.
* **Without preemption**, if your primary router (with a high priority) fails and then comes back online, it will not reclaim the Active role from the backup router (which now has a lower priority). It will wait until the current Active router fails.
* **With preemption**, the high-priority router will immediately initiate a takeover and become Active again once it's back online. This is almost always desired behavior for predictable network traffic flow.

---

#### `standby [group] track [object] [decrement]`
This powerful command provides intelligent failover. It links HSRP's status to the status of something else, most commonly another interface.
* **`[object]`**: Typically an interface, like your WAN link (`GigabitEthernet0/0`).
* **`[decrement]`**: The amount to subtract from the router's HSRP priority if the tracked object goes down.
* **Use Case:** Your primary router has a priority of 110. You track its WAN link with a decrement value of 20. If that WAN link fails, the router's priority becomes 90 (110 - 20). If the standby router's priority is 100, it will now have the higher priority and will take over, redirecting traffic through its own working WAN link and preventing a traffic black hole.
