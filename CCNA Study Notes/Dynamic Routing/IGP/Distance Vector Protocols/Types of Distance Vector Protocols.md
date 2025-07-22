#rip #eigrp #distancevector #Cisco 
![[IMG_9678.jpeg]]

| Protocol | Administrative Distance (AD) | Key CCNA Facts |
| :--- | :--- | :--- |
| **RIP** | **120** | **Metric:** Hop Count (Max 15).<br>**Updates:** Broadcasts its full table every 30 seconds (v1).<br>**Versions:** v1 is classful, v2 is classless.<br>**Loop Prevention:** Uses split horizon & route poisoning. |
| **EIGRP** | **90** (Internal)<br>**170** (External) | **Metric:** Composite (Bandwidth & Delay by default).<br>**Algorithm:** DUAL ensures fast, loop-free backup paths.<br>**Key Terms:** Successor (best path), Feasible Successor (backup path).<br>**Updates:** Partial & bounded updates are triggered by topology changes.<br>**Transport:** Uses its own Reliable Transport Protocol (RTP), IP Protocol 88. |


Routing Information Protocol (RIP)
The Routing Information Protocol (RIP) is one of the oldest and simplest distance-vector routing protocols. It was designed for small, straightforward networks and serves as a foundational concept in networking education.
History & Evolution
RIP's origins trace back to the Xerox Network Systems (XNS) protocol suite in the 1970s. It was first formally specified in 1988 in RFC 1058 as RIPv1. Its simplicity led to wide adoption in the early days of IP networking.
 * RIPv1: A classful protocol, meaning it doesn't send subnet mask information in its updates. This makes it incompatible with modern networks that use Variable Length Subnet Masking (VLSM) and Classless Inter-Domain Routing (CIDR). It uses broadcast (255.255.255.255) for updates.
 * RIPv2: Introduced in 1994 (RFC 2453), this version added support for VLSM by including subnet mask information in routing updates, making it a classless protocol. It uses multicast (224.0.0.9) for updates to reduce unnecessary traffic on non-router hosts and supports authentication.
 * RIPng (Next Generation): Defined in RFC 2080, this version was adapted to support IPv6 networks.
Core Concepts & Operation
RIP determines the best path to a destination based on a single metric: hop count. A hop is any router a packet crosses. The path with the fewest hops is considered the best.
 * Routing by Rumor: RIP operates on the principle of "routing by rumor." Routers only know the information told to them by their immediate neighbors. They trust this information, update their own routing table, and pass it along to their other neighbors.
 * Full Routing Table Updates: By default, a RIP router broadcasts its entire routing table to its neighbors every 30 seconds. This can be inefficient in larger networks.
 * Maximum Hop Count: RIP has a maximum hop count of 15. Any route that requires 16 hops is considered unreachable or infinitely far away. This is a major limiting factor for scalability.
CCNA Key Facts
 * Administrative Distance (AD): The default AD for RIP is 120. This is a measure of trustworthiness; a lower AD is preferred. Since 120 is relatively high, routes learned from more advanced protocols like OSPF (110) or EIGRP (90) will be chosen over a RIP route to the same destination.
 * Loop Prevention Mechanisms:
   * Split Horizon: A rule that prevents a router from advertising a route back out of the same interface from which it was learned. This is a primary defense against routing loops.
   * Route Poisoning / Poison Reverse: When a router detects that a route has failed, it immediately advertises that route with a metric of 16 (unreachable). With poison reverse, it specifically sends this "poisoned" route back to the router that originally advertised it, overriding the split horizon rule to ensure the bad route is quickly purged from the network.
 * Key Timers:
   * Update Timer: 30 seconds. The interval between sending full routing table updates.
   * Invalid Timer: 180 seconds. The time a router waits without hearing an update for a route before marking it as invalid (metric 16).
   * Hold-down Timer: 180 seconds (Cisco implementation). After a route is marked invalid, the router won't accept any new information about that route (unless it's from the original source with a better metric) for this duration to prevent flapping routes from causing instability.
   * Flush Timer: 240 seconds. The time after which a route is completely removed from the routing table.
Enhanced Interior Gateway Routing Protocol (EIGRP)
EIGRP is an advanced distance-vector protocol, often called a hybrid protocol because it combines the best aspects of both distance-vector and link-state protocols. It offers rapid convergence and efficient operation, making it a popular choice in Cisco-centric networks.
History & Evolution
EIGRP was developed by Cisco in the early 1990s as a proprietary successor to the outdated Interior Gateway Routing Protocol (IGRP). For decades, it was a major competitive advantage for Cisco. In 2013, Cisco released the core functionality of EIGRP as an open standard (RFC 7868), though some advanced features remain Cisco-proprietary.
Core Concepts & Operation
EIGRP is more sophisticated than traditional distance-vector protocols. Instead of just hop count, it uses a composite metric and a powerful algorithm to determine the best path.
 * DUAL Algorithm (Diffusing Update Algorithm): This is the engine behind EIGRP's fast convergence. DUAL allows a router to pre-calculate backup paths. If a primary path fails, the router can immediately switch to a pre-determined, loop-free backup path without needing to re-calculate the entire network topology.
 * Successor & Feasible Successor:
   * Successor: The primary, best route to a destination. This is the route installed in the routing table.
   * Feasible Successor (FS): A loop-free backup route that is stored in the topology table and can be used instantly if the successor fails. A route qualifies as a Feasible Successor if its advertised distance is less than the feasible distance of the current successor route (this is known as the Feasibility Condition).
 * Neighbor & Topology Tables: EIGRP maintains three tables:
   * Neighbor Table: A list of directly connected, adjacent EIGRP routers with which it has formed a neighborship.
   * Topology Table: A list of all routes learned from its neighbors, including successors and feasible successors. It contains the "big picture" of the network.
   * Routing Table: The list of the best, loop-free routes (the successors) that are actually used to forward traffic.
 * Partial & Bounded Updates: Unlike RIP, EIGRP does not send full routing table updates periodically. It sends partial updates only when a metric or topology change occurs. These updates are bounded, meaning they are only sent to the routers that are affected by the change, saving significant bandwidth.
CCNA Key Facts
 * Administrative Distance (AD):
   * Internal EIGRP: 90
   * External EIGRP: 170 (for routes redistributed into EIGRP from another protocol)
   * The AD of 90 is lower than both OSPF (110) and RIP (120), making it the most preferred IGP on a Cisco router by default.
 * Composite Metric: The metric is calculated using a formula that, by default, considers bandwidth and delay. Reliability, load, and MTU can also be used but are not recommended. The lowest bandwidth and cumulative delay along a path determine the final metric.
 * Protocol Number: EIGRP uses protocol number 88.
 * Packet Types: EIGRP uses the Reliable Transport Protocol (RTP) for communication and has several packet types:
   * Hello: Used to discover neighbors and ensure they are still available. Sent via multicast (224.0.0.10).
   * Update: Transmits routing information.
   * Query: Sent to find a new path when a successor fails and no feasible successor is available.
   * Reply: Responds to a query.
   * Ack: Acknowledges the receipt of reliable packets.
 * Autonomous System (AS): Routers must be configured with the same AS number to become neighbors and exchange routes. This is a critical configuration parameter.
