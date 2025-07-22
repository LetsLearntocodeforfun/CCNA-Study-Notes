#ACLs #security #CLI 

![[IMG_9800.jpeg]]


You can resequence a named or modern numbered ACL using the ip access-list resequence command to clean up the sequence numbers and create uniform spacing, which makes the ACL easier to manage.
### A Guide to Resequencing ACLs

**CCNA Exam Objective:** 4.4 (Configure and verify standard access control lists)

When you add and delete many individual lines in a named ACL, the sequence numbers can become messy and leave no room for future edits (e.g., you might have lines `10, 11, 12, 20`). The **resequence** command is a powerful tool that automatically renumbers all the entries in an ACL to be clean and evenly spaced.

***

### The Resequence Command

This command is run from **global configuration mode**, not from within the ACL sub-configuration mode.

`ip access-list resequence [acl-name | acl-number] [starting-sequence-number] [increment]`

* **`[acl-name | acl-number]`**: The name or number of the ACL you want to resequence.
* **`[starting-sequence-number]`**: The new sequence number for the very first entry in the list.
* **`[increment]`**: The value to add for each subsequent entry.

---

### How It Works: An Example

As shown in your slide, imagine an ACL where the sequence numbers are jumbled: `1, 3, 2, 4, 5`. This makes it impossible to insert a new rule between any of the existing ones.

**Original ACL:**

1 deny 192.168.1.1
3 deny 192.168.3.1
2 deny 192.168.2.1
4 deny 192.168.4.1
5 permit any
*(Note: The router processes them in numerical order, not the order you entered them.)*

**The Resequence Command:**
You want to renumber this list to start at 10 and increment by 10.
```cisco
ip access-list resequence 1 10 10

Resequenced ACL:
The router automatically reorders and renumbers the entries, resulting in a clean list with plenty of space for future additions.
10 deny 192.168.1.1
20 deny 192.168.2.1
30 deny 192.168.3.1
40 deny 192.168.4.1
50 permit any

This command is an essential tool for maintaining and managing complex ACLs over time.

