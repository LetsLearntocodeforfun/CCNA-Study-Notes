#rip #IGP #distancevector 

![[IMG_9683.jpeg]]
![[IMG_9686.jpeg]]
![[IMG_9685.jpeg]]

### Routing Information Protocol (RIP)

**TLDR:** RIP is one of the oldest and simplest routing protocols. It uses **hop count** as its only metric and is best suited for small, simple networks or lab environments due to its significant limitations.

***

### **Core Characteristics**

| Feature | Value |
| :--- | :--- |
| **Protocol Type** | Distance-Vector |
| **Administrative Distance** | **120** |
| **Metric** | Hop Count |
| **Maximum Hops** | **15** (a hop count of 16 is unreachable) |
| **Update Timer** | 30 seconds (sends full routing table) |
| **Update Address** | Broadcast (v1) / Multicast `224.0.0.9` (v2) |

***

### **How It Works: Routing by Rumor**

RIP operates on a principle often called "routing by rumor." It's a classic distance-vector protocol, meaning routers only have knowledge of the networks their direct neighbors tell them about.

1.  **Periodic Updates:** Every **30 seconds**, a RIP-enabled router broadcasts its entire routing table to all of its neighbors.
2.  **Metric Calculation:** The only factor RIP considers is **hop count**. It doesn't care if a path is a super-fast 10 Gbps fiber link or a slow 1.5 Mbps T1 link; both are just "1" hop. The path with the fewest hops is always considered the best.
3.  **Information Propagation:** A router receives updates from its neighbors, adds one to the hop count for each route, and then passes this new information along to its other neighbors in the next update cycle.

This process is simple but inefficient and slow to adapt to network changes.

***

### **Versions of RIP**

There are three versions of RIP, with RIPv2 being the most relevant for modern studies.

* **RIPv1 (Classful)**
    * Does **not** send subnet mask information in its updates. This means it cannot support networks with Variable Length Subnet Masking (VLSM).
    * Uses **broadcast** (`255.255.255.255`) for its updates, which can disturb non-router devices on the network.
    * Does not support authentication.

* **RIPv2 (Classless)**
    * An improvement that **includes subnet mask information** with route updates, making it **classless** and compatible with VLSM.
    * Uses **multicast** (`224.0.0.9`) to send updates, so only other RIPv2 routers process them.
    * Supports MD5 authentication to ensure route updates are from a trusted source.

* **RIPng (Next Generation)**
    * Adapted from RIPv2 to support routing for **IPv6** networks.

***

### **Loop Prevention & Timers**

Because RIP routers only know what their neighbors tell them, routing loops can easily form. RIP relies on several mechanisms to prevent this.

* **Split Horizon:** A simple rule that states: Do not advertise a route back out of the same interface you learned it from. This is a primary defense against basic two-router loops.
* **Route Poisoning:** When a route fails, instead of just removing it, a router will immediately advertise it with a metric of **16** (unreachable). This "poisons" the route, telling all neighbors to remove it from their tables immediately.
* **Hold-down Timer:** After a route is marked as unreachable, the router will start a hold-down timer (180 seconds). During this time, the router will ignore any new information about that route that indicates it has a worse metric, preventing flapping routes from causing instability.

### **When to Use RIP**

* **Advantages:**
    * Extremely simple to configure.
    * Low resource usage on routers.
* **Disadvantages:**
    * Slow convergence time.
    * Limited scalability due to the 15-hop limit.
    * Inefficient periodic full-table updates consume bandwidth.
    * Basic metric (hop count) leads to suboptimal path selection.
* **Best Use Case:** Today, RIP is primarily used in educational labs or very small, simple, and stable networks where performance is not a critical factor. It is not recommended for modern production environments.
