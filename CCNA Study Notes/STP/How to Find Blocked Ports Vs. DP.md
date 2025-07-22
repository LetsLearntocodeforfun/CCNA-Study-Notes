#stp #examday #blockedports #BPDU 
# STP Cheat Sheet: How to Find Blocked Ports

This is a step-by-step process for analyzing any Spanning Tree topology.

---

### Step 1: Find the Root Bridge

This is always the first action. The switch with the absolute **lowest Bridge ID (BID)** wins.

* **Tie-Breaker Logic:**
    * Compare **Priority** values (lowest wins).
    * If Priorities are tied, compare **MAC Addresses** (lowest wins).

> **Key fact:** All ports on the Root Bridge are **Designated Ports** and are never blocked.

---

### Step 2: Find ALL Root Ports

Every switch that is **not** the root must have **exactly one Root Port (RP)**. This is its single best path back to the Root Bridge.

* **Tie-Breaker Logic (in order):**
    1.  The path with the **Lowest Root Path Cost**.
    2.  The path from the neighbor with the **Lowest Sender Bridge ID**.
    3.  The path from the neighbor's **Lowest Sender Port ID**.

> **Key fact:** Root Ports are always in a forwarding state and are **never blocked**.

---

### Step 3: Find Blocked Ports on Remaining Links

Any link that is **not** connected to a Root Port is a redundant link where a port **must be blocked**. On these links, the two connected ports have a face-off. The winner is a Designated Port (DP); the loser is Blocked (BLK).

* **Tie-Breaker Logic (in order):**
    1.  The port on the switch with the **Lowest Path Cost to the Root** wins and becomes the DP.
    2.  If costs are tied, the port on the switch with the **Lowest Bridge ID** wins and becomes the DP.

> **Key fact:** The port that **loses** this face-off is your **Blocked Port**.