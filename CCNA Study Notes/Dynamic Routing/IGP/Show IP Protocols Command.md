#CLI #OSPF #rip #eigrp #Cisco 

![[IMG_9690.jpeg]]

### The `show ip protocols` Command

The **`show ip protocols`** command is a first-look, high-level verification tool that gives you a summary of all the dynamic IP routing protocols running on a router. It's one of the most useful commands for quickly understanding a router's configuration, which networks it's routing for, and who its neighbors are.

***

### **CLI Cheat Sheet**

| Action | Command |
| :--- | :--- |
| View all running protocols | `show ip protocols` |
| View only the OSPF section | `show ip protocols \| section ospf` |
| View only the EIGRP section | `show ip protocols \| section eigrp` |
| Find lines containing specific text | `show ip protocols \| include [text]` |

***

### **Dissecting the Output**

The output of `show ip protocols` provides a wealth of information. Let's break down a typical example for OSPF.

```cisco
R1# show ip protocols
*** IP Routing is NSF aware ***

Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 1.1.1.1
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    10.1.1.1 0.0.0.0 area 0
    10.12.1.0 0.0.0.255 area 0
  Passive Interface(s):
    GigabitEthernet0/1
  Routing Information Sources:
    Gateway         Distance      Last Update
    2.2.2.2              110      00:00:15
  Distance: (default is 110)

 * Routing Protocol: Identifies the protocol and its process ID or AS number (e.g., ospf 1).
 * Router ID: Shows the unique 32-bit ID for the router, critical for OSPF and EIGRP.
 * Maximum path: Shows how many equal-cost paths can be installed in the routing table (ECMP).
 * Routing for Networks: This is a crucial section. It shows the exact network commands that have been configured, telling you which interfaces are participating in the routing process and which networks are being advertised.
 * Passive Interface(s): Lists any interfaces that are not forming neighbor adjacencies, which is a key security and efficiency setting.
 * Routing Information Sources: This lists the router ID or IP address of every neighbor (gateway) the router is learning from. It also shows the neighbor's Administrative Distance and when the last update was received. This is one of the best ways to quickly verify that neighbor relationships are up.
 * Distance: Confirms the Administrative Distance being used for the protocol.
Practical Examples
The output format changes slightly based on the protocol, highlighting what's important for each.
EIGRP Example Output
Notice the differences: it shows the AS number, K-values for the metric calculation, and both internal and external ADs.
R1# show ip protocols
*** IP Routing is NSF aware ***

Routing Protocol is "eigrp 100"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Default networks are not set
  Redistributing: static
  EIGRP-IPv4 Protocol for AS(100)
    Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
    Router-ID: 1.1.1.1
    Topology : 0 (base)
    Active Timer: 3 min
    Distance: internal 90 external 170
    Maximum path: 4
    Maximum hopcount 100
    Routing for Networks:
      10.1.1.0
      10.12.1.0
    Routing Information Sources:
      Gateway         Distance      Last Update
      10.12.1.2             90      00:15:32

RIP Example Output
RIP's output is much simpler. It highlights the timers, the version, and whether summarization is on or off.
R1# show ip protocols
Routing Protocol is "rip"
  Sending updates every 30 seconds, next due in 12 seconds
  Invalid after 180 seconds, hold down 180, flushed after 240
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Redistributing: rip
  Default version control: send version 2, receive version 2
    Interface             Send  Recv  Triggered RIP  Key-chain
    GigabitEthernet0/0    2     2
    GigabitEthernet0/1    2     2
  Automatic network summarization is not in effect
  Routing for Networks:
    10.0.0.0
  Routing Information Sources:
    Gateway         Distance      Last Update
    10.12.1.2            120      00:00:05
  Distance: (default is 120)


