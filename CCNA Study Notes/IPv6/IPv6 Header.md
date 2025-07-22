#ipv6 #Header

![[IMG_9774.jpeg]]

### An Architect's Guide to the IPv6 Header

**CCNA Exam Objective:** 2.2 (Describe IPv6 header)

The IPv6 header was designed for efficiency and simplicity. Unlike the complex IPv4 header, the main IPv6 header has a **fixed size of 40 bytes** and contains only eight essential fields. Optional information is handled by separate **extension headers**, which prevents the main header from becoming bloated and allows for faster processing by routers.

***

### IPv6 Header Fields at a Glance 📊

| Field | Size (bits) | IPv4 Equivalent | Purpose |
| :--- | :--- | :--- | :--- |
| **Version** | 4 | Version | Identifies the IP version (always `6`). |
| **Traffic Class** | 8 | ToS / DSCP | Used for Quality of Service (QoS) to prioritize traffic. |
| **Flow Label** | 20 | *None* | A new field used to label sequences of packets that require the same handling. |
| **Payload Length**| 16 | Total Length | Specifies the size of the payload (the data portion) in bytes. |
| **Next Header** | 8 | Protocol | Identifies the type of header that immediately follows (e.g., TCP, UDP, or an extension header). |
| **Hop Limit** | 8 | Time to Live (TTL) | A counter that is decremented by each router; when it reaches 0, the packet is discarded to prevent routing loops. |
| **Source Address**| 128 | Source Address | The 128-bit IPv6 address of the originating device. |
| **Destination Address**| 128 | Destination Address | The 128-bit IPv6 address of the intended recipient. |

---

### Field-by-Field Breakdown

#### Version
* A 4-bit field that always contains the binary value `0110`, which is the number 6. This is the very first field a router looks at to identify the packet as IPv6.

#### Traffic Class
* An 8-bit field, identical in purpose to the Differentiated Services (DSCP) field in IPv4. It allows packets to be marked for **Quality of Service (QoS)**, enabling routers to prioritize delay-sensitive traffic like voice and video over less critical traffic like email.

#### Flow Label
* A 20-bit field that is new to IPv6. It can be used to label a sequence of packets belonging to the same conversation or "flow." A router can then be configured to handle all packets with the same flow label in the same way, without having to inspect each one deeply. This allows for more efficient load balancing and real-time traffic processing.

#### Payload Length
* A 16-bit field that specifies the size of the packet's payload (everything *after* the 40-byte header) in bytes.

#### Next Header
* An 8-bit field that cleverly replaces IPv4's Protocol field. It identifies the type of header that comes immediately after the main IPv6 header.
    * If the next header is **TCP**, this field will contain the value `6`.
    * If the next header is **UDP**, it will contain `17`.
    * If the packet is using optional features, this field will point to the first **Extension Header** (e.g., a value of `60` for a Destination Options header).

#### Hop Limit
* An 8-bit field that serves the exact same purpose as **Time to Live (TTL)** in IPv4. It's an integer that is decremented by 1 by every router that forwards the packet. If the Hop Limit reaches 0, the packet is discarded. This is a crucial mechanism to prevent packets from looping endlessly around the network.

#### Source & Destination Address
* Each of these fields is 128 bits long and contains the full IPv6 address of the sender and intended recipient, respectively.
