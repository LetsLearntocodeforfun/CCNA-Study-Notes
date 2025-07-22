#OSPF #Cisco #CLI #examday #linkstate #ISIS 

![[IMG_9697.jpeg]]

### Understanding Link-State Protocols

Link-state routing protocols represent a significant evolution from their distance-vector predecessors. Instead of relying on "routing by rumor," every link-state router builds a complete and synchronized topographical map of the network. Using this map, each router independently calculates the shortest, loop-free path to every destination, making them highly scalable, fast to converge, and incredibly robust.

***

### **The Link-State Operational Model**

All link-state protocols, including OSPF and IS-IS, operate on the same fundamental three-step process. Think of it as **Meet, Map, and Math**.

#### **1. Meet the Neighbors (Neighbor Discovery)**
* A newly active router begins by sending **Hello packets** out of its link-state enabled interfaces.
* When two adjacent routers receive each other's Hello packets and agree on certain parameters (like the area ID and timers), they form a formal neighbor relationship, or **adjacency**. This is the foundational step. They are now direct "friends."

#### **2. Map the Network (Database Exchange)**
* Once adjacencies are formed, routers exchange their knowledge of the network using **Link-State Advertisements (LSAs)** in OSPF or **Link-State Packets (LSPs)** in IS-IS. Each LSA/LSP is a small piece of the puzzle, describing a router, its connected links (interfaces), their status, and their "cost."
* Routers flood these LSAs/LSPs to all their neighbors. The neighbors, in turn, flood them to their neighbors, and so on, until every router within a given **area** has an identical copy of every other router's LSAs.
* This complete collection of all LSAs is called the **Link-State Database (LSDB)**. At this point, every router in the area has the same, complete map of the network topology.

#### **3. Do the Math (Path Calculation)**
* With a synchronized LSDB, each router independently runs the **Shortest Path First (SPF) algorithm**, also known as Dijkstra's algorithm.
* The SPF algorithm uses the LSDB to build a tree of the shortest paths to every destination, with itself as the **root** of the tree.
* The resulting best, loop-free paths are then installed into the router's **routing table**. Because every router runs the same algorithm on the same database, the resulting paths are consistent and loop-free.

When a network change occurs (a link goes down), a new LSA is generated and flooded. Every router updates its LSDB and re-runs the SPF algorithm to quickly converge on a new set of best paths.

***

### **Link-State vs. Distance-Vector**

| Feature | Distance-Vector (e.g., RIP) | Link-State (e.g., OSPF, IS-IS) |
| :--- | :--- | :--- |
| **Network View** | "Routing by Rumor" - Only knows what neighbors say. | Has a complete topographical map of the area. |
| **Updates** | Sends its entire routing table periodically. | Sends small, triggered updates only when a change occurs. |
| **Convergence** | Slow, prone to loops during convergence. | Very fast, as a new SPF run calculates new paths instantly. |
| **Algorithm** | Bellman-Ford (RIP) or DUAL (EIGRP). | Dijkstra's Shortest Path First (SPF). |
| **Resources** | Low CPU/Memory usage. | Higher CPU/Memory usage due to LSDB and SPF calculation. |
| **Scalability** | Limited (e.g., RIP's 15-hop limit). | Highly scalable through the use of hierarchical areas. |

***

### **General CLI Verification Guide**

While the exact commands differ slightly between OSPF and IS-IS, the verification *process* is the same. You check the three core components: neighbors, the database, and the interfaces.

| Verification Step | OSPF Command | IS-IS Command | What It Shows |
| :--- | :--- | :--- | :--- |
| **1. Verify Neighbors** | `show ip ospf neighbor` | `show isis neighbors` | Confirms that adjacencies have been successfully formed. |
| **2. Inspect the Database** | `show ip ospf database` | `show isis database` | Lets you view the LSDB to ensure it's synchronized. |
| **3. Check Interfaces** | `show ip ospf interface` | `show isis interface` | Shows which interfaces are participating in the protocol and their status. |
| **4. Check the Result** | `show ip route ospf` | `show ip route isis` | Displays the final routes installed in the routing table by the protocol. |
