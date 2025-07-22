#ACLs #security 

![[IMG_9792.jpeg]]

You can use two main types of Access Control Lists (ACLs) to filter IPv4 traffic: standard ACLs, which filter traffic based only on the source IP address, and extended ACLs, which can filter traffic based on source and destination IP addresses, protocols, and port numbers.
### A Guide to Standard vs. Extended ACLs

**CCNA Exam Objective:** 4.4 & 4.5 (Configure and verify standard and extended access control lists)

In Cisco networking, you have two primary tools for filtering IPv4 traffic: standard and extended Access Control Lists (ACLs). The fundamental difference between them is the level of granularity they provide. Standard ACLs offer simple, source-based filtering, while extended ACLs provide a much more detailed, surgical approach to traffic control.

***

### Comparison at a Glance ⚖️

| Feature | Standard ACL | Extended ACL |
| :--- | :--- | :--- |
| **Filtering Criteria**| **Source IP Address ONLY** | **Source & Destination IP**, Protocol, Source & Destination Port |
| **Complexity** | Simple | Complex and granular |
| **Placement** | As **close to the destination** as possible | As **close to the source** as possible |
| **Numbered Range**| `1-99` & `1300-1999` | `100-199` & `2000-2699` |

---

### Standard ACLs

A standard ACL is the most basic filter. It operates at Layer 3 and makes its `permit` or `deny` decision based on only one piece of information: the source IP address of the packet.

* **Analogy:** Think of a bouncer at a VIP event who only has a list of approved last names. They don't care who you are trying to see or what you're doing; if your last name is on the list, you're in.
* **Use Case:**
    * **Simple access control:** Allowing an entire subnet to access a resource while blocking all others.
    * **Controlling VTY access:** Permitting only specific IP addresses (like from a management network) to be able to Telnet or SSH into the router.

---

### Extended ACLs

An extended ACL is a much more powerful and intelligent filter. It can inspect a packet's Layer 3 and Layer 4 headers to make a decision based on multiple criteria simultaneously.

* **Filtering criteria include:**
    * Source IP Address
    * Destination IP Address
    * Protocol Type (e.g., TCP, UDP, ICMP)
    * Source Port Number (e.g., a random client port)
    * Destination Port Number (e.g., port 80 for HTTP)

* **Analogy:** Think of a security guard at a high-security facility. They check your ID (source IP), ask who you are visiting (destination IP), what your business is (protocol), and which specific office you are going to (destination port).

* **Use Case:**
    * **Precise traffic control:** Allowing users from the `10.1.1.0/24` network to access a web server at `172.16.1.10` on port 443 (HTTPS), while denying all other traffic from that same source network. This is impossible with a standard ACL.
    * **Blocking specific applications:** Denying all FTP traffic (TCP port 21) from entering your network, regardless of its source or destination.

