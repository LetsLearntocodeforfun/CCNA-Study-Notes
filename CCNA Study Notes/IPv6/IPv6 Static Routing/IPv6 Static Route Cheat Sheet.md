#ipv6 #staticroute #cheatsheet

### The Static Routing CLI Playbook 📖

This playbook covers the essential commands for crafting and validating IPv6 static routes.

| Command Type | Command | Purpose |
| :--- | :--- | :--- |
| **Configuration**| `ipv6 route [prefix/len] [exit-int \| next-hop]` | The fundamental command to create a static route. |
| **Configuration**| `ipv6 route ::/0 [exit-int \| next-hop]` | Creates the default route, or gateway of last resort. |
| **Configuration**| `ipv6 route [prefix/len] [next-hop] [AD]` | Creates a floating static route by setting a high Administrative Distance (AD). |
| **Verification** | `show ipv6 route` | Displays the entire IPv6 routing table. |
| **Verification** | `show ipv6 route static` | Displays **only** the static routes in the routing table. |
| **Verification** | `show ipv6 route [prefix]` | Shows the specific routing table entry for a given prefix. |
| **Verification** | `ping [ipv6-address]` | Tests basic Layer 3 reachability to a destination. |
| **Verification** | `traceroute [ipv6-address]` | Traces the hop-by-hop path taken to a destination. |

***

### The Craft of Static Routing: An In-Depth Look
![[IMG_9784.jpeg]]

#### 1. Recursive Static Route (The Standard)
This route type specifies only the IP address of the next-hop router. It's called "recursive" because the router must perform a second lookup in its routing table to find the interface needed to reach that next-hop IP.

* **Syntax:** `ipv6 route [prefix/len] [next-hop-ipv6-address]`
* **Example:**
    ```cisco
    ! To reach network 2, send packets to router 2001:DB8:12::2
    ipv6 route 2001:DB8:ACAD:2::/64 2001:DB8:12::2
    ```

#### 2. Fully Specified Static Route (The Best Practice)
This is the most efficient form of a static route because it provides both the exit interface and the next-hop IP. The router knows exactly where to send the packet without needing a second lookup.

* **Syntax:** `ipv6 route [prefix/len] [exit-interface] [next-hop-ipv6-address]`
* **Example:**
    ```cisco
    ! To reach network 2, send packets out Gi0/1 towards router 2001:DB8:12::2
    ipv6 route 2001:DB8:ACAD:2::/64 GigabitEthernet0/1 2001:DB8:12::2
    ```

#### 3. Floating Static Route (The Backup Plan)
A floating static route is a backup route with a manually increased Administrative Distance (AD). It remains inactive in the topology table as long as a primary route with a better (lower) AD is available. If the primary route fails, the floating static route is instantly installed in the routing table.

* **Syntax:** `ipv6 route [prefix/len] [next-hop] [AD]`
* **Example:** Assume OSPF (AD 110) is the primary routing source.
    ```cisco
    ! This static route has an AD of 111, making it less preferred than OSPF.
    ! It will only be used if the OSPF route to this destination is lost.
    ipv6 route 2001:DB8:ACAD:2::/64 2001:DB8:BACKUP::2 111