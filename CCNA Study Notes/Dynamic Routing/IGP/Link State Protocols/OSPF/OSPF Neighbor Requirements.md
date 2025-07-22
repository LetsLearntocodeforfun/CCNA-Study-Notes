#OSPF #Cisco #IGP #networktype 

![[IMG_9709.jpeg]]

### The 8 Rules for OSPF Adjacency

For two OSPF routers to become fully adjacent neighbors, they must agree on eight specific parameters sent within their **Hello packets**. If any one of these rules is violated, the adjacency will fail, preventing route exchange.

***

#### 1. Unique Router ID
Each router in the OSPF domain must have a **unique Router ID**. If two routers have a duplicate ID, routing becomes unstable and adjacencies will flap. The Router ID is the highest IP on a loopback interface or, if none exists, the highest IP on an active physical interface.

---

#### 2. Matching Area ID
The routers' interfaces must belong to the **same OSPF area**. A router in Area 0 cannot form a neighbor relationship with a router in Area 1 on the same link.

---

#### 3. Matching Subnet
The routers' interfaces must be on the **same IP subnet**. For example, `10.1.1.1/24` can become a neighbor with `10.1.1.2/24`, but not with `10.1.2.1/24`.

---

#### 4. Matching Hello & Dead Timers
The **Hello interval** (how often Hello packets are sent) and the **Dead interval** (how long to wait before declaring a neighbor down) must be identical. By default, this is 10/40 seconds on broadcast and point-to-point networks, and 30/120 seconds on non-broadcast networks.

---

#### 5. Matching Authentication
If OSPF authentication is configured, both the **password** and the **authentication type** (e.g., plaintext or MD5) must match exactly on both routers.

---

#### 6. Matching Stub Area Flag
Both routers must agree on the **stub area flag**. One router cannot be configured for a stub area while the other is configured for a standard area. This prevents discrepancies in the Link-State Database.

---

#### 7. Matching MTU (Maximum Transmission Unit)
By default, the **MTU** (Maximum Transmission Unit) must match between neighbors to ensure they can exchange database information without fragmentation issues. A mismatch can cause the adjacency to get stuck in the `ExStart/Exchange` state.

---

#### 8. Matching Network Type
The routers must agree on the **OSPF network type** (e.g., Broadcast, Point-to-Point). This is implicitly checked as different network types have different default timers and DR/BDR election rules. A mismatch in network types will often cause a violation of another rule, like the timers.
