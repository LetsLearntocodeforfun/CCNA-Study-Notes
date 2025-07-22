#examday #rootcost #rootport #stp #BPDU
## STP (Spanning Tree Protocol) Quick Reference Guide

### The Purpose of STP (The "Why")

STP is a **Layer 2** protocol that ensures a loop-free network topology in a switched environment with redundant links.

- **Problem:** Redundant links between switches create Layer 2 loops.
    
- **Result of Loops:** Without STP, a single broadcast frame can loop forever, amplifying itself until it consumes all available bandwidth and crashes the network. This is called a **broadcast storm**.
    
- **STP's Solution:** STP intelligently detects redundant paths and puts one of the ports into a **blocking state**. This breaks the loop while still allowing the link to be available as a backup if the primary path fails.
    

---

### How STP Works: The 3-Step Process

STP builds a loop-free map of the network in three steps:

1. **Elect a Root Bridge:** The switches elect a single "boss" switch, called the **Root Bridge**. This becomes the central point of the STP topology.
    
2. **Determine Best Paths:** Every non-root switch calculates its single best, shortest path to the Root Bridge. The port used for this path becomes the **Root Port**.
    
3. **Block Redundant Links:** Any other ports that are not a Root Port or a Designated Port (see below) are put into a **blocking state** to prevent loops.
    

---

### Core STP Concepts & Terminology

#### BPDUs (Bridge Protocol Data Units)

These are the messages switches use to share information with each other to build the STP map.

#### Bridge ID (BID)

The BID is how a switch identifies itself. It consists of two parts. The switch with the **lowest overall BID** becomes the Root Bridge.

- **Bridge ID** = `Bridge Priority (2 bytes)` + `MAC Address (6 bytes)`
    
- The default priority is **32768**. You can manually lower the priority to influence which switch becomes the root.
    

#### Path Cost

The "distance" to the Root Bridge. STP uses path cost to determine the best path. Lower cost is better.

|Link Speed|Path Cost (802.1D)|
|---|---|
|**10 Gbps**|2|
|**1 Gbps**|4|
|**100 Mbps**|19|
|**10 Mbps**|100|

Export to Sheets

#### Port Roles (The Result of STP)

Once STP converges, every active port will have one of these roles:

|Port Role|Purpose|
|---|---|
|**Root Port (RP)**|The single port on a _non-root_ switch with the best path (lowest cost) to the Root Bridge.|
|**Designated Port (DP)**|The port on a network segment that has the best path to the Root Bridge. All ports on the Root Bridge are Designated Ports. Every active network segment has one DP.|
|**Blocked Port (BLK)**|A port that is logically shut down by STP to prevent a loop. It doesn't forward traffic but still listens to BPDUs.|

Export to Sheets

#### Port States (The Process of STP)

These are the steps a port goes through when it's first enabled (classic 802.1D STP).

|State|Forwards Data?|Learns MACs?|Description|
|---|---|---|---|
|**Blocking**|No|No|Port is logically shut down to prevent a loop.|
|**Listening**|No|No|Port participates in the STP election process.|
|**Learning**|No|Yes|Port begins building its MAC address table.|
|**Forwarding**|Yes|Yes|Port is fully operational.|

Export to Sheets

---

### Flavors of Spanning Tree

|Protocol|Standard|Convergence|Description|
|---|---|---|---|
|**STP**|802.1D|Slow (~50 sec)|The original, slow-converging standard.|
|**PVST+**|Cisco|Slow (~50 sec)|Cisco's version that runs a separate STP instance **P**er **V**LAN.|
|**RSTP**|802.1w|Fast (~1-2 sec)|**R**apid STP. A major improvement with faster convergence. Consolidates port states into Discarding, Learning, Forwarding.|
|**Rapid PVST+**|Cisco|Fast (~1-2 sec)|Cisco's version of RSTP running per VLAN. **The default on modern Cisco switches.**|

Export to Sheets

---

### Essential Commands

|Command|Description|
|---|---|
|`show spanning-tree`|Shows the STP status for all VLANs (in PVST+).|
|`show spanning-tree vlan [vlan-id]`|Shows detailed STP information for a specific VLAN. Use this to find the Root Bridge and check port roles/states.|
|`spanning-tree vlan [vlan-id] priority [value]`|(In global config mode) Sets the bridge priority for a specific VLAN to influence Root Bridge election. The value must be in increments of 4096.|