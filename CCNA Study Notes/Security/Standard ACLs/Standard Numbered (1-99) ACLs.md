#ACLs #security 

![[IMG_9793.jpeg]]

![[IMG_9795.jpeg]]

You can configure a standard numbered Access Control List (ACL) using the access-list global configuration command, specifying a number between 1-99, and then applying it to an interface using the ip access-group command.
### A Guide to Standard Numbered ACLs

**CCNA Exam Objective:** 4.4 (Configure and verify standard access control lists)

A **standard numbered ACL** is the traditional method for creating a basic, source-IP-based traffic filter. It uses a number from **1 to 99** to identify the list of rules. While effective, numbered ACLs are considered legacy because they are difficult to edit; you cannot delete a single rule and must delete and re-create the entire list to make changes.

***

### Numbered ACL CLI Playbook 📖

| Command Type | Command |
| :--- | :--- |
| **Configuration**| `access-list [1-99] [permit\|deny] [source] [wildcard]` |
| **Application** | `ip access-group [number] [in\|out]` |
| **Verification** | `show access-lists` |
| **Verification** | `show running-config \| section access-list` |
| **Removal** | `no access-list [number]` |

***

### The Configuration Process ⚙️

Configuring a numbered ACL is a two-step process:
1.  **Create the ACL:** In global configuration mode, you define the list of `permit` and `deny` statements.
2.  **Apply the ACL:** You apply the list to a specific interface to tell the router to start filtering traffic.

---

### Command Breakdown & Examples

#### `access-list [number] [permit|deny] [source] [wildcard]`
This is the command used to build the ACL.

* **`[number]`**: A number from 1 to 99. All lines that are part of the same ACL must use the same number.
* **`[permit|deny]`**: The action to take if the source IP matches.
* **`[source]`**: The source IP address you want to match. This can be a single host or an entire network.
* **`[wildcard]`**: The wildcard mask that specifies which parts of the source address must match. It's the inverse of a subnet mask (`0` means match, `1` means ignore).

**Keyword Shortcuts:**
* To match a single host, you can use the `host` keyword: `deny host 10.1.1.1`
* To match all addresses, you can use the `any` keyword: `permit any`

#### **Example Scenario**
We want to block a specific user at `192.168.10.100` but allow everyone else from the `192.168.10.0/24` network to access a server.

**Configuration:**
```cisco
! Remember to put the most specific rule first
access-list 50 deny host 192.168.10.100

! Now, permit the rest of the subnet
access-list 50 permit 192.168.10.0 0.0.0.255

(Remember the invisible deny any is at the end, so any traffic not from the 192.168.10.0/24 network will be dropped.)
ip access-group [number] [in|out]
An ACL does nothing until it is applied to an interface.
 * [in|out]: This specifies the direction of traffic to inspect.
   * in: Inspects traffic as it enters the interface, before it is routed.
   * out: Inspects traffic as it exits the interface, after it has been routed.
Application:
Let's apply ACL 50 to the interface GigabitEthernet0/1 to filter traffic leaving the router towards the server.
interface GigabitEthernet0/1
 ip access-group 50 out

Now, the router will inspect all packets exiting this interface against the rules in access-list 50.

