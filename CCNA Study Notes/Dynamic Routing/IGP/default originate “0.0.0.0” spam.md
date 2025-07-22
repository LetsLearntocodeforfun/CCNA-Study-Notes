#CLI #Cisco  #rip #OSPF #defaultroute

![[IMG_9689.jpeg]]

### The `default-information originate` Command

The **`default-information originate`** command is used to inject a **default route** (a path for traffic with an unknown destination, i.e., `0.0.0.0/0`) from one router into an entire routing domain. It's essentially how you tell all the other routers in your network, "If you don't know where to send a packet, send it to me." This is most commonly used on an edge router that has a path to the internet.

***

### **CLI Command Cheat Sheet**

| Protocol | Command Syntax | Notes |
| :--- | :--- | :--- |
| **OSPF** | `default-information originate` | Requires a default route to already exist in the routing table. |
| **OSPF (Force)** | `default-information originate always` | **Advertises a default route even if one doesn't exist.** Useful but can cause black holes if used incorrectly. |
| **EIGRP** | `ip summary-address eigrp [asn] 0.0.0.0 0.0.0.0` | In modern IOS, you must redistribute a static default route. The `default-information` command is not used for EIGRP. |

***

### **Core Concept: Creating a Gateway to the World**

Imagine a company network (an Autonomous System) connected to the internet through a single router. The internal routers running OSPF or EIGRP only know about internal company networks. They have no idea how to reach `google.com` or any other external address.

The `default-information originate` command solves this. You configure it on the **edge router** (the one connected to the internet). This command tells the edge router to generate a default route and advertise it to all other internal routers. Now, when an internal router receives a packet for an unknown destination, it sees the advertised default route and knows to forward it to the edge router, which can then send it to the internet.

---

### **OSPF `default-information originate`**

OSPF has a straightforward implementation of this command.

#### **How it Works**

By default, the `default-information originate` command has one critical requirement: the router you configure it on **must already have a default route in its routing table**. This is a safety feature. The router won't advertise itself as the gateway to the world unless it actually has a gateway itself (typically a static route to the ISP).

If you add the **`always`** keyword, you override this safety check. The router will advertise a default route no matter what, even if it has no path to the internet.

#### **Practical CLI Example**

* **R1** is the edge router connected to the ISP at `203.0.113.2`.
* **R2** is an internal router.

**Configuration on Edge Router (R1):**
```cisco
! First, create a static default route pointing to the ISP
ip route 0.0.0.0 0.0.0.0 203.0.113.2

! Then, enter OSPF configuration
router ospf 1
 ! Tell OSPF to advertise the default route we just created
 default-information originate

Verification on Internal Router (R2):
After the configuration, the routing table on R2 will show a new route. The * indicates it's a candidate for a default route, and O*E2 means it was learned via OSPF as an External Type 2 route.
R2# show ip route
Gateway of last resort is 10.1.1.1 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 10.1.1.1, 00:05:30, GigabitEthernet0/0
      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks

EIGRP: Redistribution Method
EIGRP does not use the default-information originate command. Instead, the standard and most reliable method is to redistribute a static route.
How it Works
You create a static default route on the edge router, just like with OSPF. Then, you enter the EIGRP configuration and use the redistribute static command. This tells EIGRP to take any static routes configured on the router and advertise them to all other EIGRP neighbors. EIGRP automatically summarizes this to 0.0.0.0/0 at the boundary.
Practical CLI Example
 * R1 is the edge router connected to the ISP.
 * R2 is an internal EIGRP router.
Configuration on Edge Router (R1):
! 1. Create a static default route
ip route 0.0.0.0 0.0.0.0 203.0.113.2

! 2. Enter EIGRP configuration and redistribute
router eigrp 100
 redistribute static
 ! You can optionally define a metric for the redistributed route
 default-metric 10000 100 255 1 1500

Note: Without setting a default-metric, the redistributed route's metric will be infinite, and it won't be advertised.
Verification on Internal Router (R2):
The routing table on R2 will now show an external EIGRP route, marked as D*EX.
R2# show ip route
Gateway of last resort is 10.1.1.1 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/2688256] via 10.1.1.1, 00:02:10, GigabitEthernet0/0


