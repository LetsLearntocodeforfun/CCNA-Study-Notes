#Cisco #eigrp #CLI 

![[IMG_9691.jpeg]]

### Enhanced Interior Gateway Routing Protocol (EIGRP)

EIGRP is an advanced distance-vector routing protocol developed by Cisco, often called a **hybrid** protocol because it combines the best features of both distance-vector and link-state protocols. Its key differentiator is the **Diffusing Update Algorithm (DUAL)**, which provides exceptionally fast convergence by pre-calculating loop-free backup paths.

***

### **What Makes EIGRP Unique?**

1.  **The DUAL Algorithm:** This is EIGRP's biggest advantage. DUAL allows a router to identify a backup path (a **Feasible Successor**) ahead of time. If the primary path (the **Successor**) fails, the router can switch to the backup path almost instantly, without needing to re-compute the network topology.
2.  **Protocol-Dependent Modules (PDMs):** EIGRP was designed to be modular. It can be extended to support different network layer protocols, not just IPv4. This is why you see specific configurations for EIGRP for IPv4 and EIGRP for IPv6.
3.  **Unequal-Cost Load Balancing:** While other protocols can only load balance over paths with the exact same metric (ECMP), EIGRP can be configured with the `variance` command to send traffic over paths with different metrics, as long as they meet a certain feasibility condition.
4.  **Partial & Bounded Updates:** Unlike RIP, which sends its full routing table every 30 seconds, EIGRP only sends updates when a change occurs. These updates are "partial" (containing only the changed information) and "bounded" (sent only to the routers that are affected by the change), making it highly efficient.

***

### **CLI Cheat Sheet**

| Action | Command |
| :--- | :--- |
| Enable EIGRP | `router eigrp [AS_number]` |
| Define a network | `network [address] [wildcard_mask]` |
| Verify neighbors | `show ip eigrp neighbors` |
| View the topology table | `show ip eigrp topology` |
| View DUAL computations | `debug eigrp fsm` |
| Verify EIGRP interfaces | `show ip eigrp interfaces` |
| Change unequal-cost variance | `variance [multiplier]` |

***

### **Core Components**

EIGRP relies on three main tables and a sophisticated algorithm to function.

* **Neighbor Table:** A list of all directly connected routers that have formed an EIGRP adjacency (a "neighborship"). This is maintained by sending and receiving **Hello** packets.
* **Topology Table:** This is the heart of EIGRP. It contains all the routes learned from all neighbors, including the best paths and any potential backup paths. It's built from the **Update** packets received from neighbors.
* **Routing Table:** This contains only the absolute best, loop-free path to a destination (the **Successor**). This is the table the router uses to actively forward traffic.

The DUAL algorithm runs against the topology table to select the Successor and Feasible Successor routes.
* **Successor:** The best route to a destination, installed in the routing table.
* **Feasible Successor (FS):** A pre-calculated, loop-free backup route. A route qualifies as an FS if its **Advertised Distance (AD)** is less than the **Feasible Distance (FD)** of the current successor. This is known as the **Feasibility Condition** and is crucial for preventing routing loops.

### **EIGRP Packet Types**

EIGRP uses its own reliable transport protocol (RTP) to deliver several types of packets:

| Packet Type | Purpose |
| :--- | :--- |
| **Hello** | Used to discover neighbors and ensure they are still available. |
| **Update** | Transmits routing information when a new route is discovered or an existing one changes. |
| **Query** | Sent to find a new path when a successor fails and **no** feasible successor is available. |
| **Reply** | Responds to a query, either providing a new path or indicating no path exists. |
| **Ack** | Acknowledges the receipt of Update, Query, and Reply packets. |

---

### **Basic Configuration**

Here's a quick example of a basic EIGRP configuration.

```cisco
router eigrp 100
 ! The AS number "100" must match on neighboring routers.
 ! A best practice to prevent unexpected summarization.
 no auto-summary
 ! Enable EIGRP on interfaces in the 10.1.1.0/24 network.
 network 10.1.1.0 0.0.0.255
 ! Enable EIGRP on the interface with the exact IP 192.168.1.1.
 network 192.168.1.1 0.0.0.0
