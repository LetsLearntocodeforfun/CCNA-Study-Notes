#FHRP #Cisco #HSRP 

![[IMG_9713.jpeg]]

![[IMG_9712.jpeg]]

![[IMG_9719.jpeg]]
###  First Hop Redundancy Protocols (FHRP)

A **First Hop Redundancy Protocol (FHRP)** creates a redundant default gateway, preventing a single point of failure for end-user devices. FHRPs create a **virtual router**, with a virtual IP address that clients use as their gateway, which can be shared between two or more physical routers. If one router fails, another instantly takes over, ensuring connectivity is maintained.

---

### FHRP Comparison at a Glance 📊

| Feature | HSRP (Hot Standby Router Protocol) | VRRP (Virtual Router Redundancy Protocol) | GLBP (Gateway Load Balancing Protocol) |
| :--- | :--- | :--- | :--- |
| **Standard** | Cisco Proprietary | **IETF Open Standard** | Cisco Proprietary |
| **Router Roles** | Active / Standby | **Master** / Backup | **AVG** / **AVF** |
| **Load Balancing** | No (Active router handles all traffic) | No (Master router handles all traffic) | **Yes** (Multiple routers can forward traffic simultaneously) |
| **Virtual MAC** | One per group | One per group | **Multiple** (One per forwarder) |
| **Communication**| Multicast `224.0.0.2` | Multicast `224.0.0.18` | Multicast `224.0.0.102` |

---

### HSRP: Hot Standby Router Protocol

HSRP is Cisco's solution for gateway redundancy. One router is elected **Active** and handles all forwarding, while another is elected **Standby** and waits to take over if the Active router fails. The election is based on **priority** (higher wins), with the highest IP address breaking ties.

#### **Basic HSRP Configuration**
This configuration is applied to the Layer 3 interface on the participating routers.

* **Primary Router (R1):**
    ```cisco
    interface GigabitEthernet0/1
     ip address 192.168.1.2 255.255.255.0
     standby 1 ip 192.168.1.1
     standby 1 priority 110
     standby 1 preempt
    ```
* **Standby Router (R2):**
    ```cisco
    interface GigabitEthernet0/1
     ip address 192.168.1.3 255.255.255.0
     standby 1 ip 192.168.1.1
     standby 1 priority 100
     standby 1 preempt
    ```

---

### VRRP: Virtual Router Redundancy Protocol

VRRP is the **open-standard** equivalent of HSRP, making it ideal for multi-vendor environments. Functionally, it's very similar.

* **Router Roles:** It uses the terms **Master** (equivalent to HSRP's Active) and **Backup** (equivalent to Standby).
* **Virtual IP:** The virtual IP address can be the **same as the real IP address** of the Master router's interface, which is a key difference from HSRP.
* **Preemption:** Preemption is **enabled by default**, so a router with a higher priority will always become the Master.

#### **Basic VRRP Configuration**

```cisco
interface GigabitEthernet0/1
 ip address 192.168.1.2 255.255.255.0
 ! The command set is very similar to HSRP
 vrrp 1 ip 192.168.1.1
 vrrp 1 priority 110
 ! Preemption is on by default but can be explicitly configured
 vrrp 1 preempt
