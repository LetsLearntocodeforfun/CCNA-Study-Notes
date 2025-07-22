#ipv6 #staticroute #CLI #Cisco 

![[IMG_9782.jpeg]]


### A Guide to Configuring IPv6 Static Routes

**CCNA Exam Objective:** 2.1 (Configure and verify IPv6 addressing and prefix)

An **IPv6 static route** is a manually configured path that tells a router how to reach a destination network that is not directly connected. Static routes are the simplest form of routing and are essential for defining paths in smaller networks or for specifying a gateway of last resort.

***

### IPv6 Static Route CLI Cheat Sheet ⚙️

| Route Type | Command Syntax |
| :--- | :--- |
| **Directly Attached**| `ipv6 route [prefix/len] [exit-interface]` |
| **Recursive** | `ipv6 route [prefix/len] [next-hop-ipv6-address]` |
| **Fully Specified** | `ipv6 route [prefix/len] [exit-interface] [next-hop-ipv6-address]` |
| **Default Route** | `ipv6 route ::/0 [exit-int \| next-hop-ipv6]` |

***

### Command Breakdown & Examples
![[IMG_9783.jpeg]]

#### 1. Directly Attached Static Route

This type defines the route using only the local router's **exit interface**. The router assumes the destination is directly connected to this interface and will perform neighbor discovery to find the next hop.

* **Use Case:** Best for point-to-point serial links or other broadcast-disabled media where there is only one possible next-hop device.
* **Example:**
    To route traffic for the `2001:DB8:ACAD:2::/64` network out the `Serial0/0/0` interface:
    ```cisco
    ipv6 route 2001:DB8:ACAD:2::/64 Serial0/0/0
    ```

---

#### 2. Recursive Static Route

This is the most common type of static route. It defines the route using the **IPv6 address of the next-hop router**. The router must perform a "recursive lookup" in its routing table to first find the path to the next-hop address.

* **Use Case:** The standard method for static routing on multi-access networks like Ethernet.
* **Example:**
    To route traffic for `2001:DB8:ACAD:2::/64` to a neighboring router with the IP `2001:DB8:12::2`:
    ```cisco
    ipv6 route 2001:DB8:ACAD:2::/64 2001:DB8:12::2
    ```

---

#### 3. Fully Specified Static Route

This type is considered a best practice as it specifies **both the exit interface and the next-hop address**. This eliminates the need for a recursive lookup, making it the most efficient type. The router knows exactly where and how to send the packet in a single step.

* **Use Case:** The recommended method for all static routes on multi-access networks.
* **Example:**
    To route traffic for `2001:DB8:ACAD:2::/64` out interface `GigabitEthernet0/1` towards next-hop `2001:DB8:12::2`:
    ```cisco
    ipv6 route 2001:DB8:ACAD:2::/64 GigabitEthernet0/1 2001:DB8:12::2
    ```

---

#### 4. Default Static Route (`::/0`)

A default route, also known as the **gateway of last resort**, tells the router where to send traffic for any destination that is not specifically listed in its routing table. It uses the prefix `::/0`, which means "all networks."

* **Use Case:** Essential for edge routers to direct all internet-bound traffic to the ISP.
* **Example:**
    To create a default route pointing to an ISP at `2001:DB8:100::1`:
    ```cisco
    ipv6 route ::/0 2001:DB8:100::1
    ```
