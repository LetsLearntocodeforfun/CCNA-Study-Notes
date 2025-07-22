#metric #dynamicrouting #Cisco #eigrp #OSPF 

![[IMG_9679.jpeg]]


### IGP Metrics & Administrative Distance

This note covers how the main Interior Gateway Protocols (IGPs) covered in the CCNA—RIP, EIGRP, OSPF, and IS-IS—calculate their metrics to determine the "best" path to a destination.

***

### **Quick Reference Chart**

| Protocol | Administrative Distance | Metric Name | How it's Calculated |
| :--- | :--- | :--- | :--- |
| **RIP** | **120** | **Hop Count** | The number of routers a packet must cross. Lowest number wins. |
| **EIGRP** | **90** (Internal) | **Composite** | By default, uses the slowest bandwidth and cumulative delay of a path. |
| **OSPF** | **110** | **Cost** | Calculated based on interface bandwidth. `Cost = Ref Bandwidth / Intf Bandwidth`. |
| **IS-IS** | **115** | **Cost** | A simple dimensionless metric. By default, all links are 10. Lowest sum wins. |

***

### **RIP: Hop Count**

RIP uses the simplest metric possible: **hop count**.

* **Definition:** A "hop" is a router. The hop count is simply the number of routers a packet must pass through to reach its destination network.
* **Path Selection:** The path with the lowest hop count is always chosen as the best path and installed in the routing table.
* **Limitation:** RIP has a maximum hop count of **15**. A route that is 16 hops away is considered unreachable. This makes RIP unsuitable for large networks. It also ignores important factors like link speed; a slow 56kbps serial link is considered just as "good" as a fast 10 Gbps fiber link because they both count as one hop.

***

### **EIGRP: Composite Metric**

EIGRP uses a complex **composite metric** that, by default, provides a much more sophisticated view of the network path. The calculation is based on values known as **K-values**.

* **Default Metrics (K-values):** By default, only **Bandwidth (K1)** and **Delay (K3)** are used in the calculation. While the formula also includes variables for Reliability (K4, K5) and Load (K2), they are turned off by default because they can cause frequent route recalculations if they fluctuate.
* **How it Works:**
    * **Bandwidth:** EIGRP considers the *slowest* bandwidth link along the entire path. A path is only as fast as its slowest segment.
    * **Delay:** EIGRP calculates the *cumulative* (sum) of the delay values for every interface the packet exits along the path.
* **Result:** These two values are plugged into a complex formula to produce a single numeric metric. The route with the **lowest** resulting metric is chosen as the best path. This approach is much more intelligent than RIP's simple hop count.

***

### **OSPF: Cost**

OSPF uses a metric called **cost**, which is inversely proportional to the bandwidth of an interface. A lower cost is better.

* **The Formula:** OSPF calculates the cost of traversing a link using the following formula:
    `Cost = Reference Bandwidth / Interface Bandwidth`
* **Reference Bandwidth:** This is a configurable value that defaults to **100 Mbps**.
* **How it Works:**
    * For a 100 Mbps FastEthernet link: `Cost = 100 Mbps / 100 Mbps = 1`
    * For a 10 Mbps Ethernet link: `Cost = 100 Mbps / 10 Mbps = 10`
* **The Modern Problem:** With the default reference bandwidth, any link faster than 100 Mbps (like GigabitEthernet or 10-GigabitEthernet) will also have a cost of 1.
    `Cost = 100 Mbps / 1000 Mbps = 0.1`, which OSPF rounds up to **1**.
* **Solution:** To fix this and allow OSPF to differentiate between fast links, network administrators must manually change the reference bandwidth to a higher value (e.g., 10,000 for 10 Gbps) across all OSPF routers using the `auto-cost reference-bandwidth` command. The total cost of a route is the sum of the costs of all outgoing interfaces along the path.

***

### **IS-IS: Cost**

IS-IS (Intermediate System to Intermediate System) is another link-state protocol, similar to OSPF. Its metric is also called **cost**, but it is calculated much more simply by default.

* **Definition:** The IS-IS metric is a dimensionless number assigned to each interface. By default, Cisco routers assign a cost of **10** to every interface, regardless of its bandwidth.
* **Path Selection:** The total cost of a route is the sum of the costs on all outgoing interfaces along the path to the destination. Just like with OSPF, the path with the lowest total cost wins.
* **Simplicity:** Because every link starts with the same cost, IS-IS by default behaves similarly to RIP, preferring the path with the fewest hops. However, this cost is fully customizable by the administrator to influence path selection based on bandwidth or other policies.
* **CCNA Context:** For the CCNA, it's important to know IS-IS is a link-state protocol like OSPF, its AD is **115** (placing it between OSPF and RIP in trustworthiness), and its metric is a simple "cost."
