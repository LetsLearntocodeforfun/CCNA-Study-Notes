#network #CLI #Cisco #eigrp #rip #OSPF 
![[IMG_9688.jpeg]]

### The `passive-interface` Command

**TLDR:** The `passive-interface` command stops a routing protocol from **sending or receiving** routing updates on a specific interface, usually for security and efficiency. The router **still advertises** the network connected to that passive interface out its other active ports.

***

### **CLI Command Cheat Sheet**

| Action | Command Syntax |
| :--- | :--- |
| Make one interface passive | `passive-interface [interface_name]` |
| Make all interfaces passive | `passive-interface default` |
| Re-enable an interface | `no passive-interface [interface_name]` |

***

### **Core Concept: What Does `passive-interface` Actually Do?**

When you enable a routing protocol on an interface (using the `network` command), that interface actively tries to form neighbor relationships by sending out "hello" packets and routing updates. This is great for links connecting to other routers, but it's unnecessary and insecure on a link connected to a Local Area Network (LAN) where only end-user devices reside.

The **`passive-interface`** command modifies this behavior:

1.  **Stops Outgoing Updates:** The router will no longer send any routing protocol packets (hellos, updates, queries) out of the passive interface. This saves bandwidth and prevents advertising your network topology to untrusted devices.
2.  **Ignores Incoming Updates:** The router will drop any incoming routing protocol packets on that interface, meaning it cannot form a neighbor relationship through a passive interface.
3.  **Still Advertises the Network:** This is the most important part. Even though the interface is "passive," the router continues to advertise the network segment connected to it through its other, non-passive interfaces.

***

### **How It's Used with IGPs (RIP, EIGRP, OSPF)**

The command's function and configuration are identical across all major IGPs. It is a best practice to use it on any interface that does not connect to another router.

**Example Scenario:**
A router is connected to another router on its `GigabitEthernet0/0` interface and to a user LAN on its `GigabitEthernet0/1` interface. We want to include the LAN network in our routing protocol but not send updates to the user devices.

**Configuration (OSPF Example):**
```cisco
router ospf 1
 router-id 1.1.1.1
 ! Make the LAN interface passive. No OSPF hellos will be sent to the LAN.
 passive-interface GigabitEthernet0/1
 ! Enable OSPF on both interfaces.
 network 10.10.10.0 0.0.0.255 area 0  ! Router-to-router link
 network 192.168.1.0 0.0.0.255 area 0 ! User LAN link

Result:
 * The router will form a neighbor relationship with the other router on GigabitEthernet0/0.
 * The router will not send any OSPF traffic out of GigabitEthernet0/1.
 * The router will advertise the 192.168.1.0/24 network to its neighbor on the GigabitEthernet0/0 link.
Security Best Practice: passive-interface default
For enhanced security, especially on routers with many interfaces, you can make all interfaces passive by default and then selectively enable only the necessary ones.
router eigrp 100
 ! Shut down EIGRP on all interfaces by default.
 passive-interface default
 ! Explicitly re-enable EIGRP only on the interface facing another router.
 no passive-interface GigabitEthernet0/0
 ! The network commands still define which networks to advertise.
 network 10.10.10.0
 network 192.168.1.0

This approach ensures that you never accidentally send routing updates out of a new interface you configure in the future.

