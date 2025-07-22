#FHRP #Cisco #virtualmac

![[IMG_9715.jpeg]]

![[IMG_9714.jpeg]]

### First Hop Redundancy Protocols (FHRP) Deep Dive

At its core, networking is about creating resilient and reliable paths. For end-user devices like laptops and servers, the single most critical path is the one to their **default gateway**. If that one router fails, they are cut off from the rest of the world. **First Hop Redundancy Protocols (FHRPs)** were created to solve this fundamental single point of failure by creating the illusion of a single, highly-available virtual router that never goes down.

***

### The Core Concept: The Virtual Router 🧠

Imagine two routers, R1 and R2, at the edge of a network. Instead of configuring half your PCs to use R1 as their gateway and the other half to use R2, you use an FHRP to create a **virtual router**.

* **Virtual IP Address:** This is the IP address you assign to the virtual router. All PCs on the LAN will use this single IP as their default gateway.
* **Virtual MAC Address:** The virtual router also has its own MAC address. When a PC sends traffic to the virtual IP, it sends an ARP request, and the FHRP provides this virtual MAC.

One of the physical routers is elected as the **active** or **master** router. It "owns" the virtual IP and MAC and is responsible for forwarding all traffic. The other router(s) act as **standby** or **backup** routers, constantly monitoring the health of the active one. If the active router fails, a standby router instantly takes ownership of the virtual IP and MAC, and traffic forwarding continues seamlessly. The end devices never even know a failure occurred.

---

### FHRP Comparison: HSRP vs. VRRP vs. GLBP

| Feature | HSRP (Hot Standby Router Protocol) | VRRP (Virtual Router Redundancy Protocol) | GLBP (Gateway Load Balancing Protocol) |
| :--- | :--- | :--- | :--- |
| **Standard** | Cisco Proprietary | **IETF Open Standard** | Cisco Proprietary |
| **Primary Goal** | Redundancy | Redundancy | **Redundancy & Load Balancing** |
| **Router Roles** | Active / Standby | Master / Backup | AVG (Manager) / AVF (Forwarders) |
| **Use Case** | The Cisco standard for simple failover. | Multi-vendor environments needing failover. | Cisco environments needing to use all available bandwidth. |

***

### Deep Dive: HSRP (Hot Standby Router Protocol)

HSRP is the most common FHRP found in Cisco environments. Its operation is straightforward and robust.

#### HSRP Concepts

* **Roles:** One router is **Active**, handling all traffic. One is **Standby**, ready to take over. All others are in a "Listen" state.
* **Election:** The router with the highest configured **priority** (0-255, default 100) wins the election to become Active. If priorities are tied, the router with the highest IP address wins.
* **Preemption:** If `preempt` is configured, a router that comes online with a higher priority can forcibly take the Active role from a current Active router with a lower priority. This ensures the "strongest" router is always in charge. **Without preemption, a router that was once Active but failed will not regain its role, even if its priority is higher.**

#### Practical Example: HSRP with Interface Tracking ⚙️

The biggest risk with a simple FHRP setup is if the Active router is up, but its connection to the internet goes down. The router itself hasn't failed, so it remains Active, but it silently drops all traffic meant for the outside world—a "black hole." **Interface tracking** solves this.

**Scenario:** R1 (priority 110) is the primary gateway. R2 (priority 100) is the backup. We will "track" R1's internet-facing interface (`GigabitEthernet0/0`). If this interface fails, we will decrease R1's priority by 20, making it 90. This is lower than R2's priority of 100, forcing R2 to become the Active router.

**Configuration on R1 (Primary):**
```cisco
interface GigabitEthernet0/1
 ! This is the LAN-facing interface
 ip address 192.168.1.2 255.255.255.0
 ! --- HSRP Configuration ---
 ! Enable HSRP group 1 with the virtual IP for clients
 standby 1 ip 192.168.1.1
 ! Set a high priority to make this the default Active router
 standby 1 priority 110
 ! Allow this router to reclaim the Active role if it recovers
 standby 1 preempt
 ! Track the WAN interface. If it goes down, decrease priority by 20.
 standby 1 track GigabitEthernet0/0 20
