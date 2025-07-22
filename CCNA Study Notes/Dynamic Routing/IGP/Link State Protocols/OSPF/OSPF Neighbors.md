#OSPF #IGP #Cisco #examday 

![[IMG_9705.jpeg]]

![[IMG_9706.jpeg]]

###  OSPF Neighbors

In OSPF, a **neighbor** relationship (or **adjacency**) is a formal, two-way agreement between routers to exchange routing information. This relationship is the absolute foundation of an OSPF network. No adjacency means no route exchange. This process is managed through **Hello packets**.

---

### How Neighbors Are Formed 🤝

Routers discover each other by sending Hello packets to the multicast address **224.0.0.5** (AllSPFRouters). For two routers to become neighbors, several parameters sent within their Hello packets **must match exactly**:

1.  **Area ID:** They must be in the same OSPF area.
2.  **Authentication:** If used, the password and type must be identical.
3.  **Hello and Dead Timers:** The timers must be the same (e.g., 10-second hello, 40-second dead).
4.  **Subnet Mask:** They must be on the same subnet.
5.  **Stub Area Flag:** Both routers must agree on whether the area is a stub area or not.

---

### OSPF Neighbor States

From discovery to full communication, routers progress through several states. The `show ip ospf neighbor` command lets you see the current state.

| State | What's Happening |
| :--- | :--- |
| **Down** | The initial state. No Hello packets have been received from the neighbor. |
| **Init** | A Hello packet has been received, but the local router's ID was not in it. This means communication is still one-way. |
| **2-Way** | Communication is now two-way. Each router has seen its own ID in the other's Hello packet. **On multi-access networks, DR/BDR election happens here.** This is the final state for non-DR/BDR routers. |
| **ExStart** | The routers are preparing to exchange their Link-State Databases (LSDBs). They elect a master/slave relationship to control the exchange. |
| **Exchange** | Routers send each other Database Descriptor (DBD) packets, which are summaries of their LSDBs. |
| **Loading** | Routers request and receive more detailed information (LSAs) for any database entries that are out of date. |
| **Full** | The LSDBs are fully synchronized. The routers are now fully adjacent, and routing can proceed. **This is the desired state for most adjacencies.** |

---

### DR & BDR Election

On multi-access networks like Ethernet, OSPF elects a **Designated Router (DR)** and a **Backup Designated Router (BDR)** to manage the adjacency process. Instead of every router forming a `Full` adjacency with every other router (which would be inefficient), they only form a `Full` adjacency with the DR and BDR.

* **Election Process:**
    1.  The router with the highest **OSPF priority** (1-255) on the interface wins. The default priority is 1.
    2.  If priorities are tied, the router with the highest **Router ID** wins.
* **Important Note:** A router with an interface priority of **0** is ineligible to become DR or BDR.

---

### Essential CLI Commands

#### **Verification**

* `show ip ospf neighbor`
    **The most important command.** Shows all neighbors, their priority, their state (`Full`, `2-Way`, etc.), their IP address, and the local interface.

* `show ip ospf interface [name]`
    Displays OSPF details for an interface, including its configured priority, the network type, and the Router ID of the segment's DR and BDR.

#### **Configuration & Troubleshooting**

* `ip ospf priority [0-255]`
    (Interface-level command) Manually sets the priority to influence the DR/BDR election.

* `debug ip ospf adj`
    (Use with caution) Provides real-time debug output of the adjacency-forming process, showing state changes as they happen. It's invaluable for troubleshooting why neighbors are stuck.

