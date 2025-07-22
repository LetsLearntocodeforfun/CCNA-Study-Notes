#security #ACLs #Cisco 

![[IMG_9786.jpeg]]

You can configure a standard Access Control List (ACL) to filter traffic based on its source IP address using the access-list command for numbered ACLs or ip access-list standard for named ACLs, and then applying it to an interface with the ip access-group command.
![[IMG_9787.jpeg]]
### A Guide to Standard IPv4 ACLs

**CCNA Exam Objective:** 4.4 (Configure and verify standard access control lists)

A **standard Access Control List (ACL)** is the most basic form of traffic filtering in networking. It acts as a set of rules that permits or denies traffic based on a single criterion: the **source IPv4 address**. Think of it as a simple guest list for your network; if a packet's source address is on the list, it's allowed in, otherwise, it's blocked.

***

### Standard ACL CLI Playbook 📖

| Command Type | Command | Purpose |
| :--- | :--- | :--- |
| **Configuration**| `access-list [1-99] [permit\|deny] [source] [wildcard]` | Creates a numbered standard ACL. |
| **Configuration**| `ip access-list standard [NAME]` | Creates a named standard ACL, which is more flexible. |
| **Application** | `ip access-group [number\|NAME] [in\|out]` | Applies the ACL to an interface to begin filtering traffic. |
| **Verification** | `show access-lists` | Displays all configured ACLs on the router. |
| **Verification** | `show ip interface [name]` | Shows which ACLs are applied to a specific interface. |

***

### Core Concepts of Standard ACLs

#### 1. Numbered vs. Named ACLs
* **Numbered ACLs (1-99):** The traditional method. They are simple to create but rigid; you cannot delete a single line from a numbered ACL. To make a change, you have to delete the entire list and re-create it.
* **Named ACLs:** The modern best practice. They allow you to use a descriptive name (e.g., `BLOCK_GUEST_WIFI`) and, most importantly, let you delete or insert individual lines using sequence numbers, making them far easier to manage.

#### 2. The `permit` and `deny` Logic
Every line in an ACL starts with either `permit` or `deny`. The router processes these rules from top to bottom. As soon as a packet's source IP matches a line, the action (`permit` or `deny`) is taken, and the router stops processing the list.

#### 3. The Implicit `deny any`
This is the most critical rule to remember: at the end of every ACL, there is an invisible, unwritten rule that says **`deny any`**. This means if a packet does not match any of the `permit` rules you've created, it will be dropped. Therefore, **every ACL must have at least one `permit` statement**, or it will block all traffic.

#### 4. Placement: Close to the Destination
Because standard ACLs can only filter by source address, you can't be too specific about what kind of traffic you are blocking. If you place a standard ACL too close to the source of the traffic, you might block more than you intend. The best practice is to place standard ACLs as **close to the destination** of the traffic as possible.

---

### Configuration Examples

**Scenario:** We want to permit traffic from the `192.168.10.0/24` network but deny traffic from a specific host, `192.168.20.5`, from reaching a server on the `10.1.1.0/24` network. The ACL will be applied outbound on the router's interface connected to the server network.

#### **Numbered ACL Example**

```cisco
! Enter global configuration mode
config t

! Deny the specific host first (most specific rules go at the top)
access-list 10 deny host 192.168.20.5

! Permit the desired network
access-list 10 permit 192.168.10.0 0.0.0.255

! Apply the ACL to the interface facing the destination server
interface GigabitEthernet0/1
 ip access-group 10 out

Named ACL Example (Best Practice)
! Enter global configuration mode
config t

! Create a named ACL and enter its configuration mode
ip access-list standard SERVER_ACCESS

! Deny the specific host (sequence number 10 is automatically assigned)
deny host 192.168.20.5

! Permit the desired network (sequence number 20 is automatically assigned)
permit 192.168.10.0 0.0.0.255

! Apply the ACL to the interface
interface GigabitEthernet0/1
 ip access-group SERVER_ACCESS out

The named ACL is superior because if you needed to add another rule, you could simply enter ip access-list standard SERVER_ACCESS and add a new line with a specific sequence number without having to delete the entire list.

