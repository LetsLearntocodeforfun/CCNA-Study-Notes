#ECMP #OSPF #dynamicrouting #Cisco 

### Equal-Cost Multi-Path (ECMP) Routing

**TLDR:** ECMP allows a router to use **multiple, equal-cost paths** to the same destination simultaneously. Instead of choosing just one "best" path, it installs all of them in the routing table and spreads traffic across them, improving speed and reliability.

***

### ECMP Protocol Support & Cheat Sheet

Most modern dynamic routing protocols support ECMP automatically. The key is simply to ensure that the metrics for multiple paths are truly identical.

| Protocol | Default Behavior & Key Facts |
| :--- | :--- |
| **OSPF** | ✅ **Supported by default.** If two paths have the same calculated **cost**, OSPF will use both. The number of default paths is usually 4, but can often be increased to 8, 16, or more depending on the hardware/software. |
| **EIGRP** | ✅ **Supported by default.** If two paths have the identical **composite metric**, EIGRP installs them. EIGRP also uniquely supports **unequal-cost** load balancing with the `variance` command, but that is not ECMP. |
| **BGP** | ✅ **Supported, but requires a specific command.** By default, BGP only chooses one best path. You must enable it using the `maximum-paths` command. This is crucial for load sharing to services hosted in multiple locations. |
| **RIP** | ✅ **Supported by default.** If multiple paths have the same **hop count**, RIP will use them. |

#### CCNA-Level Cheat Sheet

Here are some quick commands for verifying and configuring ECMP, primarily using Cisco IOS syntax.

**How to Check for ECMP:**

Use the `show ip route [destination_ip]` command. If you see multiple entries for a single destination prefix, ECMP is working.

