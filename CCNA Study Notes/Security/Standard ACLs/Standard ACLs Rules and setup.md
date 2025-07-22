#ACLs #security 

![[IMG_9788.jpeg]]

![[IMG_9790.jpeg]]

You can think of the order of operations for a standard ACL as a top-to-bottom checklist; the router checks a packet against each rule in sequence and stops as soon as it finds a match.
### ACL Logic: Order of Operations

**CCNA Exam Objective:** 4.4 (Configure and verify standard access control lists)

An Access Control List (ACL) is processed by a router in a strict, sequential order. The logic is simple but unforgiving, and understanding it is critical to creating effective and predictable traffic filters.

***

### Top-Down Processing

A router reads an ACL from the **top to the bottom**, one line at a time, just like you would read a list. In numbered ACLs, this is the order of the sequence numbers. In named ACLs, you can manually set the sequence numbers to control the order.

---

### First-Match Logic

The router applies a "first-match" logic. As soon as a packet's source IP address matches a line in the ACL, the router immediately executes the action (`permit` or `deny`) and **stops processing the list**. It will not look at any subsequent lines, even if another line would also match.

This is why the **order of your ACL statements is critically important**.

---

### Rule #1: Specific Rules Before General Rules

Because of the first-match logic, you must always place your most **specific** rules at the top of the list, followed by more **general** rules.

**Incorrect Order Example:**

access-list 10 permit 192.168.1.0 0.0.0.255
access-list 10 deny host 192.168.1.50
* **Problem:** A packet from `192.168.1.50` will match the first line (`permit 192.168.1.0/24`) and will be **permitted**. The router will never even get to the second line that is meant to deny it.

**Correct Order Example:**

access-list 10 deny host 192.168.1.50
access-list 10 permit 192.168.1.0 0.0.0.255
* **Result:** A packet from `192.168.1.50` matches the first `deny` rule and is immediately dropped. All other traffic from the `192.168.1.0/24` network matches the second `permit` rule and is allowed through.

---
![[IMG_9791.jpeg]]
### The Implicit `deny any`

Remember that at the very end of every ACL, there is an invisible rule: `deny ip any any`. If a packet makes it all the way through your list without matching any of your specific `permit` or `deny` rules, it will hit this final rule and be **dropped**. This makes ACLs a "default-deny" system.

