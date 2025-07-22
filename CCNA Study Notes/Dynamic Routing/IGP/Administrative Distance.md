#administrativedistance #Cisco #IGP #OSPF #eigrp 

![[IMG_9680.jpeg]]

### Administrative Distance (AD)

**TLDR:** Administrative Distance (AD) is the score a router uses to determine which routing protocol to trust when multiple protocols provide a route to the same destination. **The lowest AD wins.**

***

### **Core Concept: The Trustworthiness Score**

A router might learn about the same destination network from several different sources simultaneously. For example, it could learn about the `10.10.10.0/24` network from OSPF, EIGRP, and a manually configured static route all at the same time. The router needs a way to decide which route is the most reliable.

Administrative Distance solves this problem by assigning a default "trustworthiness" score to every possible route source. It is **not** a metric like cost or hop count; it is simply an integer value that is only significant to the local router.

When a router has multiple paths to the same destination prefix, it will always prefer the one with the lowest AD and will install only that route into its routing table.

***

### **Default Administrative Distances (Most Trusted to Least)**

This chart lists the most common AD values you'll encounter. A lower number means the source is more trustworthy.

| Route Source | Administrative Distance | Trust Level |
| :--- | :--- | :--- |
| **Connected** | **0** | Highest (Cannot be beaten) |
| **Static Route** | **1** | Extremely High |
| **eBGP** | **20** | Very High |
| **EIGRP (Internal)** | **90** | High |
| **OSPF** | **110** | Medium |
| **IS-IS** | **115** | Medium |
| **RIP** | **120** | Low |
| **EIGRP (External)** | **170** | Very Low |
| **iBGP** | **200** | Very Low |
| **Unknown** | **255** | Untrusted (Route is ignored) |

***

### **A Practical Example**

Imagine a router, R1, needs to find the best path to the `192.168.1.0/24` network.

1.  R1's neighbor, R2, is running **OSPF** and advertises a path to `192.168.1.0/24`. R1 stores this route with its AD of **110**.
2.  R1's other neighbor, R3, is running **EIGRP** and also advertises a path to the same `192.168.1.0/24` network. R1 stores this route with its AD of **90**.

**The Decision:**
R1 compares the two administrative distances: OSPF (110) vs. EIGRP (90).

Since **90 is lower than 110**, the router trusts the EIGRP-learned route more. It will install the path learned from R3 into its routing table and begin forwarding traffic for `192.168.1.0/24` towards R3. The OSPF path is kept in the topology database as a backup but is not used unless the EIGRP path fails.

### **Floating Static Routes**

A key use for manually changing AD is creating a **floating static route**. This is a static route configured with a higher AD than the dynamically learned route.

* **Example:** You have a primary route learned via OSPF (AD 110). You can configure a static route for the same destination with an AD of 111.
* **Result:** The OSPF route is used normally. The static route "floats" and does nothing. If the OSPF route disappears from the routing table (e.g., the primary link fails), the static route will then be installed as the next-best option, providing a reliable backup path.
