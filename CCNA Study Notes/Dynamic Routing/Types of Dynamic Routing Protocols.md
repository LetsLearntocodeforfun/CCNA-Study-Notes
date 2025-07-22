#IGP #EGP #dynamicrouting #Cisco 
![[IMG_9676 1.jpeg]]
![[IMG_9677.jpeg]]


Routing Information Protocol (RIP)
 * TLDR: An older, straightforward distance-vector protocol that uses hop count as its routing metric. It's simple to set up but not very smart or scalable.
 * Advantages:
   * Easy to Configure: RIP is known for its simplicity and is very easy to implement, making it great for basic networks.
   * Low Resource Usage: It has minimal impact on CPU and memory.
 * Disadvantages:
   * Limited Scalability: With a maximum of 15 hops, it's unsuitable for large networks.
   * Slow Convergence: It can be slow to adapt to network changes, leading to potential routing loops and delays.
   * Basic Metric: Hop count isn't always the best path, as it ignores link speed and congestion.
 * Best Used: In small, simple, and stable networks or in lab environments for educational purposes. Avoid it for anything complex or large-scale.
Enhanced Interior Gateway Routing Protocol (EIGRP)
 * TLDR: A fast and efficient hybrid protocol (combining features of distance-vector and link-state) developed by Cisco. It's known for its rapid convergence and ease of use in Cisco-heavy environments.
 * Advantages:
   * Fast Convergence: Uses the DUAL algorithm to quickly find alternative paths if a primary route fails.
   * Efficient Bandwidth Use: Only sends updates when changes occur, not on a fixed schedule.
   * Advanced Metrics: Considers bandwidth, delay, reliability, and load for more intelligent path selection.
   * Unequal-Cost Load Balancing: Can distribute traffic across links with different speeds.
 * Disadvantages:
   * Vendor-Specific: While now an open standard, its advanced features are best optimized on Cisco devices, leading to potential vendor lock-in.
   * Complexity: Can be more complex to troubleshoot than simpler protocols like RIP.
 * Best Used: In medium to large-sized enterprise networks that primarily use Cisco hardware and require fast and reliable performance.
Open Shortest Path First (OSPF)
 * TLDR: A widely adopted, link-state protocol that builds a complete map of the network to calculate the best path. It's scalable, standardized, and robust, making it a go-to for many enterprise networks.
 * Advantages:
   * Scalable and Hierarchical: Uses "areas" to divide the network, which improves performance and management in large deployments.
   * Fast Convergence: Quickly recalculates routes when the network topology changes.
   * Standardized: As an open standard, it's supported by virtually all network vendors.
   * Efficient: Uses a cost metric based on bandwidth, leading to intelligent path selection.
 * Disadvantages:
   * Complex Configuration: Can be more difficult to set up and manage than protocols like RIP or even EIGRP, especially with multiple areas.
   * Resource Intensive: Requires more CPU and memory to maintain its link-state database and run the SPF algorithm.
 * Best Used: For large, scalable, and multi-vendor enterprise networks that need a robust and efficient interior gateway protocol. It's a very common choice for internal corporate networks.
Intermediate System to Intermediate System (IS-IS)
 * TLDR: A highly scalable and stable link-state protocol, similar to OSPF but often favored by large service providers for its ability to handle massive networks.
 * Advantages:
   * Extremely Scalable: Even more so than OSPF, making it ideal for massive and complex networks.
   * Protocol Independence: Was designed to route protocols other than just IP, making it very flexible.
   * Stable: Known for its stability in very large and dynamic environments.
 * Disadvantages:
   * Less Common in Enterprises: Not as widely known or deployed in typical enterprise networks, meaning fewer engineers have deep experience with it.
   * Perceived Complexity: Often seen as more complex to configure and troubleshoot than OSPF.
 * Best Used: Primarily in large service provider (ISP) networks and massive data centers where extreme scalability and stability are critical.
Border Gateway Protocol (BGP)
 * TLDR: The routing protocol of the internet. It's an exterior gateway protocol used to exchange routing information between different autonomous systems (networks). It's all about policy and making routing decisions based on paths, rules, and policies, not just speed.
 * Advantages:
   * Massively Scalable: Designed to handle the entire internet's routing table.
   * Policy-Based Routing: Allows for granular control over routing decisions based on various attributes.
   * Ensures Loop-Free Routing: Uses AS_PATH attribute to prevent routing loops between networks.
 * Disadvantages:
   * Slow Convergence: BGP is intentionally slower to converge to ensure stability across the vast and dynamic internet.
   * Complex: Extremely complex to configure and manage correctly. Misconfigurations can have widespread consequences.
   * High Resource Requirements: Needs significant memory to store large routing tables.
 * Best Used: For routing between different autonomous systems on the internet. It's essential for any organization that needs to connect to multiple ISPs or other external networks and control its own routing policies. It can also be used within very large private networks (iBGP).
