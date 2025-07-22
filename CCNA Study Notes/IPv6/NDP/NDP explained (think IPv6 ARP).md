#arp #ipv6 #NDP #SLAAC 

![[IMG_9776.jpeg]]

![[IMG_9777.jpeg]]

### An In-Depth Guide to the Neighbor Discovery Protocol (NDP)

**CCNA Exam Objective:** 2.3 (Describe the function of ICMPv6 NDP)

**Neighbor Discovery Protocol (NDP)** is one of the most significant improvements in IPv6. It's not a single protocol but a collection of functions that use **ICMPv6** messages to handle communication and discovery between devices on the same local link. NDP is the engine that makes IPv6 "plug-and-play" and more efficient than IPv4.

***

### IPv4 vs. IPv6: What NDP Replaces

NDP consolidates several disjointed IPv4 protocols into one unified mechanism.

| IPv4 Protocol / Function | IPv6 NDP Equivalent |
| :--- | :--- |
| **ARP** (Address Resolution Protocol) | Neighbor Solicitation & Advertisement |
| **ICMP Router Discovery** | Router Solicitation & Advertisement |
| **DHCP** (for basic addressing) | Stateless Address Autoconfiguration (SLAAC) |

***

### The 5 Core Functions of NDP

NDP has five primary jobs, each handled by specific ICMPv6 message types.

#### 1. Router Discovery
![[IMG_9779.jpeg]]
* **Purpose:** Allows a host to discover the routers on its local link.
* **Messages Used:**
    * **Router Solicitation (RS - Type 133):** A host sends an RS message to the "all-routers" multicast group (`FF02::2`) to say, "Are there any routers here? I need a default gateway."
    * **Router Advertisement (RA - Type 134):** A router replies to an RS (or sends periodically) with an RA message. This RA contains critical information like the network prefix for addressing (for SLAAC), its own link-local address (to be used as the default gateway), and other network parameters.

#### 2. Neighbor Address Resolution
* **Purpose:** The replacement for ARP. It resolves an IPv6 address to its corresponding Layer 2 MAC address.
* **Messages Used:**
    * **Neighbor Solicitation (NS - Type 135):** When a host needs the MAC address for a known IPv6 address, it sends an NS message to the target's unique **solicited-node multicast address**.
    * **Neighbor Advertisement (NA - Type 136):** The target device, and only that device, replies directly with an NA message containing its MAC address. This is far more efficient than IPv4's broadcast-based ARP.

#### 3. Stateless Address Autoconfiguration (SLAAC)
* **Purpose:** Allows a host to automatically configure its own Global Unicast Address (GUA).
* **How it Works:** The host takes the `/64` network prefix provided in the router's RA message and combines it with a self-generated 64-bit Interface ID (either from EUI-64 or random generation) to create a complete, usable IPv6 address.

#### 4. Duplicate Address Detection (DAD)
![[IMG_9781.jpeg]]
* **Purpose:** Ensures that no two devices on the same link accidentally use the same IPv6 address.
* **How it Works:** Before a host finalizes the use of a new address (either from SLAAC or manually configured), it sends a Neighbor Solicitation for that very address. If any other device on the link replies with a Neighbor Advertisement, the host knows the address is already in use and will not use it.

#### 5. Redirect
* **Purpose:** Allows a router to inform a host of a better first-hop gateway for a specific destination.
* **Message Used:**
    * **Redirect (Type 137):** If a router receives a packet from a host on the same link and knows that a different router on that same link provides a better path, it will forward the packet and send a Redirect message back to the host, telling it to use the other router for future traffic to that destination.
