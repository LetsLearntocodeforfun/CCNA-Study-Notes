#OSPF #networktype #IGP #Cisco 
#examday 

![[IMG_9708.jpeg]]

### OSPF Network Types

OSPF's behavior changes depending on the underlying Layer 2 technology of an interface. It classifies links into different **network types**, which dictates how OSPF discovers neighbors, disseminates routing information, and whether a Designated Router (DR) is needed. Understanding these types is key to configuring and troubleshooting OSPF.

---

### OSPF Network Types Cheat Sheet 📈

| Network Type | Default on... | DR/BDR Election? | Hello / Dead Timers | Key Characteristic |
| :--- | :--- | :--- | :--- | :--- |
| **Point-to-Point** | Serial Links, PPP, HDLC | **No** | 10 / 40 sec | Assumes only one other router can exist on the link. Most efficient type. |
| **Broadcast** | Ethernet (LAN) | **Yes** | 10 / 40 sec | Assumes multiple routers can communicate via a single broadcast message. |
| **Non-Broadcast** | Frame Relay, ATM | **Yes** | 30 / 120 sec | Assumes multiple routers exist but there is no broadcast capability. Neighbors must be manually defined. |
| **Point-to-Multipoint** | *(Manually Configured)* | **No** | 30 / 120 sec | A collection of point-to-point links treated as a single network. No DR needed. |

---

### Descriptions & Use Cases

#### Point-to-Point
This is the simplest and most efficient network type. OSPF assumes that only one other router is reachable on the interface. Because there's no possibility of having more than two routers, a DR/BDR election is unnecessary. Hello packets are sent to the AllSPFRouters multicast address `224.0.0.5`.
* **Best Use:** Standard for back-to-back serial links or MPLS connections.

#### Broadcast Multi-Access
This is the default type for Ethernet interfaces. OSPF assumes that many routers can be connected to the same segment (like a LAN switch) and that they can all hear a single broadcast/multicast message. To reduce the number of adjacencies, a **DR and BDR are elected**. All other routers (DROthers) form a full adjacency only with the DR and BDR.
* **Best Use:** Any LAN environment.

#### Non-Broadcast Multi-Access (NBMA)
This type is a relic of older WAN technologies like Frame Relay. It assumes multiple routers are on the same network, but there's no broadcast capability to automatically discover them. This means **neighbors must be statically configured**. Although a DR/BDR election occurs, the lack of broadcasting makes it complex to manage.
* **Best Use:** Legacy Frame Relay or similar WAN technologies where broadcast is not supported. It is rarely used in modern networks.

#### Point-to-Multipoint
This is not a default type but is manually configured to solve issues with NBMA networks. It treats the network as a collection of point-to-point links. It does **not** hold a DR/BDR election and instead forms a direct adjacency with each neighbor. It's more resilient than NBMA because it doesn't rely on a single DR.
* **Best Use:** An alternative for NBMA topologies to avoid manual neighbor configuration and DR complexities.

---

### Essential CLI Commands

You can view and change the OSPF network type on a per-interface basis.

* **View the Network Type:**
    ```cisco
    show ip ospf interface [interface_name]
    ```
    *(The output will clearly state the "Network Type" and associated timers.)*

* **Change the Network Type:**
    ```cisco
    interface [interface_name]
     ip ospf network [point-to-point | broadcast | etc.]
    ```
