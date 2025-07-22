#OSPF #linkstate #CLI #examday #dynamicrouting 

![[IMG_9699.jpeg]]
### Essential OSPF Verification Commands

These are the commands you will use constantly to check the status and health of your OSPF network.

---

#### `show ip ospf neighbor`
This is your most important verification command. It displays a table of all OSPF neighbors (adjacencies), their current state, the IP address of the neighbor, and the local interface on which they were discovered. You are always looking for the state to be **`FULL`** (or `2-WAY` on broadcast/NBMA networks for non-DR/BDR routers).

---

#### `show ip protocols`
This command gives a high-level summary of the entire OSPF process running on the router. It's the perfect first step to see key parameters at a glance, including the OSPF **process ID**, the **router ID**, the networks being advertised, and a list of any **passive interfaces**.

---

#### `show ip ospf interface [brief]`
This command provides OSPF-specific details on a per-interface basis. It's crucial for checking the **area** an interface belongs to, its OSPF **cost**, the configured **hello and dead timers**, and the number of neighbors found on that link. Using the `brief` keyword provides a clean, tabular summary of all OSPF-enabled interfaces.

---

#### `show ip route ospf`
This command filters the main routing table to display only the routes that have been learned via OSPF. It's the best way to confirm which OSPF routes have successfully been calculated by the SPF algorithm and installed for active use.

---

#### `show ip ospf database`
This command allows you to inspect the Link-State Database (LSDB) itself. It shows you all the Link-State Advertisements (LSAs) the router knows about. It's an advanced tool used to verify that all routers in an area have a synchronized and identical view of the network topology.

***

### Advanced & Troubleshooting Commands

These commands help you fine-tune OSPF behavior and diagnose complex problems.

---

#### `auto-cost reference-bandwidth [Mbps]`
This command, configured under `router ospf [process-id]`, adjusts the reference bandwidth used to calculate OSPF cost. Because the default is only 100 Mbps, this is essential for modern networks to differentiate between FastEthernet, GigabitEthernet, and 10-GigabitEthernet links.

---

#### `ip ospf priority [0-255]`
Configured on an interface, this command influences the **Designated Router (DR)** and **Backup Designated Router (BDR)** election. The router with the highest priority wins. A priority of **`0`** prevents the router from participating in the election at all.

---

#### `ip ospf hello-interval [seconds]`
This interface-level command changes how often Hello packets are sent. Lowering the timer can lead to faster failure detection but increases routing overhead. **The hello and dead timers must match exactly between two neighbors for an adjacency to form.**

---

#### `clear ip ospf process`
A powerful but disruptive command used to restart the entire OSPF process. This forces the router to drop all neighbor adjacencies and rebuild them from scratch. It's useful for forcing a new router-id to take effect or for clearing a "stuck" OSPF state during troubleshooting. **Use with caution in a production network.**
