#ACL #security 

![[IMG_9797 1.jpeg]]

You can configure and edit a named standard ACL using its special sub-configuration mode, which allows you to add, insert, and delete individual lines using sequence numbers.
### A Guide to Named Standard ACLs

**CCNA Exam Objective:** 4.4 (Configure and verify standard access control lists)

A **named ACL** is the modern and recommended method for creating access control lists. Unlike legacy numbered ACLs, named ACLs allow you to use a descriptive name (e.g., `BLOCK_GUESTS`, `PERMIT_MGMT`) and, most importantly, provide the ability to **edit the ACL by deleting and inserting individual lines** without having to delete the entire list.

***

### Named ACL CLI Playbook 📖

| Command Type | Command | Purpose |
| :--- | :--- | :--- |
| **Configuration**| `ip access-list standard [NAME]` | Creates a named standard ACL and enters its sub-configuration mode. |
| **Configuration**| `[permit\|deny] [source] [wildcard]` | (Inside ACL config mode) Adds a permit or deny statement. |
| **Editing** | `[sequence#] [permit\|deny] ...` | (Inside ACL config mode) Inserts a new line at a specific sequence number. |
| **Editing** | `no [sequence#]` | (Inside ACL config mode) Deletes a specific line by its sequence number. |
| **Application** | `ip access-group [NAME] [in\|out]` | Applies the named ACL to an interface. |
| **Verification** | `show access-lists [NAME]` | Displays the rules for a specific named ACL, including sequence numbers. |

***

### The Power of Editing: Sequence Numbers

When you create a named ACL, the router automatically assigns a **sequence number** to each line, typically starting at 10 and incrementing by 10 (10, 20, 30, etc.). This provides gaps, allowing you to insert new rules in between existing ones later on. This is the key advantage over numbered ACLs.

---

### Configuration & Editing Example

**Scenario:** We want to create an ACL named `VTY_ACCESS` to control SSH/Telnet access to our router. We will perform several modifications to show the flexibility of named ACLs.

**Full Configuration Sequence:**
```cisco
! Enter global configuration mode
config t

! --- Step 1: Initial Configuration ---
! Create the named ACL and enter its configuration mode
ip access-list standard VTY_ACCESS
! Permit the initial management host (this gets sequence number 10)
permit host 10.1.0.100
! Exit ACL config mode
exit

! Apply the ACL to the VTY lines to activate it
line vty 0 4
 access-class VTY_ACCESS in
exit

! --- Step 2: Adding a New Rule ---
! Re-enter the ACL configuration mode
ip access-list standard VTY_ACCESS
! Insert a new rule for a new network at sequence number 20
20 permit 10.2.0.0 0.0.255
exit

! --- Step 3: Deleting an Existing Rule ---
! Re-enter the ACL configuration mode one more time
ip access-list standard VTY_ACCESS
! Delete the original rule at sequence number 10
no 10
exit

Final Result:
After this full sequence, if you run show access-lists VTY_ACCESS, the only remaining rule will be 20 permit 10.2.0.0 0.0.255. This entire process was done without ever having to delete and re-create the entire ACL, showcasing the power of named ACLs.

