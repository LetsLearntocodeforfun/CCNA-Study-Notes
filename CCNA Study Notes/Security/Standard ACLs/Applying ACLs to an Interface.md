#ACLs #security 

![[IMG_9796.jpeg]]

### A Guide to Applying Standard ACLs

**CCNA Exam Objective:** 4.4 (Configure and verify standard access control lists)

Creating an Access Control List (ACL) is only the first step; an ACL has **no effect** until it is applied to an interface. Applying the ACL activates it, telling the router to begin inspecting traffic that passes through that interface against the rules you've defined.

***

### The `ip access-group` Command

The command to apply an ACL is always done from **interface configuration mode**.

`ip access-group [access-list-number | name] [in | out]`

* **`[access-list-number | name]`**: The identifier for the ACL you want to apply. This is either the number (e.g., `50`) or the name (e.g., `BLOCK_GUESTS`) you used when creating the list.
* **`[in | out]`**: This specifies the **direction** of traffic flow you want to filter. This is the most critical decision in applying an ACL.

---

### Direction: `in` vs. `out` 🚦

The direction is always from the perspective of the **router's interface**.

#### `in` (Inbound)
* Inspects packets as they **enter** the interface from the network segment it's connected to.
* The filtering decision is made **before** the router makes a routing decision. If the packet is denied, it's dropped immediately and never processed further.

#### `out` (Outbound)
* Inspects packets just before they **leave** the interface to go out to the connected network segment.
* The filtering decision is made **after** the router has already made a routing decision and determined that this is the correct exit interface.

---

### Placement Strategy & Example

The best practice for **standard ACLs** is to place them as **close to the destination** as possible. This is because a standard ACL can only filter by source IP and lacks the intelligence to know what service or destination the traffic is for. Placing it close to the destination prevents you from accidentally blocking legitimate traffic.

**Scenario:**
* You want to prevent the `10.1.1.0/24` network from reaching a server on the `192.168.2.0/24` network.
* Your router has two interfaces: `Gi0/0` connected to the source network and `Gi0/1` connected to the destination server network.

![ACL Placement Diagram](https://i.imgur.com/k6lP09V.png)

1.  **Create the ACL:**
    ```cisco
    access-list 10 deny 10.1.1.0 0.0.0.255
    access-list 10 permit any
    ```

2.  **Determine Placement:** The destination is the server network. The router interface closest to that destination is `Gi0/1`.

3.  **Determine Direction:** To filter traffic going *towards* the server, we need to inspect packets as they are **leaving** the router via `Gi0/1`. Therefore, the direction is **`out`**.

4.  **Apply the ACL:**
    ```cisco
    interface GigabitEthernet0/1
     ip access-group 10 out
    ```
Now, any traffic from `10.1.1.0/24` that is routed towards `Gi0/1` will be inspected and dropped just before it can leave the interface.
