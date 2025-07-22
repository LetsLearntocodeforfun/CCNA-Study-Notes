#ACLs #security #CLI 

![[IMG_9801.jpeg]]

An extended Access Control List (ACL) filters network traffic based on several criteria, including source and destination IP address, protocol, and port numbers, giving you precise control over what is permitted or denied.

![[IMG_9802.jpeg]]
### An Expert's Guide to Extended ACLs

**CCNA Exam Objective:** 4.5 (Configure and verify extended access control lists)

An **extended ACL** is a surgical tool for traffic filtering. Unlike standard ACLs that only look at the source address, extended ACLs can inspect traffic based on a combination of Layer 3 and Layer 4 information. This allows you to create highly specific rules, such as "Allow PC1 to access the web server, but nothing else."

***

### Extended ACL CLI Playbook 📖

| Command Type | Command | Purpose |
| :--- | :--- | :--- |
| **Configuration**| `access-list [100-199] [rule]` | The legacy method for creating a numbered extended ACL. |
| **Configuration**| `ip access-list extended [NAME]`| **The best practice.** Creates a flexible named extended ACL. |
| **Application** | `ip access-group [number\|NAME] [in\|out]` | Applies the ACL to an interface to begin filtering. |
| **Verification** | `show access-lists` | Displays all configured ACLs and their hit counters. |
| **Verification** | `show ip interface [name]` | Shows which ACLs are applied to an interface and in which direction. |

---

### Deconstructing the Command Syntax

The power of an extended ACL comes from its detailed syntax.

`permit/deny [protocol] [source] [destination] [operator] [port]`

* **`[protocol]`**: Specifies the Layer 4 protocol (e.g., `tcp`, `udp`, `icmp`, or `ip` to match any).
* **`[source]`**: The source IP address and wildcard mask.
* **`[destination]`**: The destination IP address and wildcard mask.
* **`[operator]`**: A keyword used to specify the port number.
    * **`eq`**: equals (matches a single port)
    * **`gt`**: greater than
    * **`lt`**: less than
    * **`neq`**: not equal
    * **`range`**: matches a range of ports
* **`[port]`**: The Layer 4 port number (e.g., 80 for HTTP, 443 for HTTPS).

---

### Configuration Example: Allowing Web Access

**Scenario:** We want to allow users on the `192.168.10.0/24` network to access a web server at `10.1.1.100`, but deny all other traffic to that server. We will use a named ACL, as it is the best practice.

Because extended ACLs are so specific, we can place them as **close to the source** as possible. We will apply this ACL inbound on the router's interface connected to the user network.

**Configuration:**
```cisco
! Create a named extended ACL
ip access-list extended WEB_ACCESS_POLICY

! Rule 1: Permit HTTP traffic (TCP port 80)
! This line permits traffic FROM any host in 192.168.10.0/24
! TO the specific web server host 10.1.1.100, ONLY if the destination port is 80.
permit tcp 192.168.10.0 0.0.0.255 host 10.1.1.100 eq 80

! Rule 2: Permit HTTPS traffic (TCP port 443)
permit tcp 192.168.10.0 0.0.0.255 host 10.1.1.100 eq 443

! The implicit "deny any" at the end will block all other traffic.
! No more rules are needed.

! Apply the ACL to the interface connected to the users
interface GigabitEthernet0/1
 ip access-group WEB_ACCESS_POLICY in

This configuration surgically permits only web traffic to the server from the specified source, while all other traffic (like ICMP pings or FTP) will be dropped by the implicit deny rule.

