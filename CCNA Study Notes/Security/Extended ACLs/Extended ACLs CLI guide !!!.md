#ACLs #security #CLI #examday 

![[IMG_9803.jpeg]]

### An Expert's Guide to Extended ACL Commands

**CCNA Exam Objective:** 4.5 (Configure and verify extended access control lists)

Here are the essential commands you need to know to configure and verify an extended ACL, ordered by the configuration mode you use them in and their importance.

***

#### Global Configuration Mode (`config`)

* **`ip access-list extended [NAME]`**
    This is the **most important** command and the modern best practice. It creates a named extended ACL and enters you into the special `config-ext-nacl` mode where you define the permit and deny rules.

* **`access-list [100-199] [rule]`**
    This is the legacy method for creating a **numbered** extended ACL. While still functional, it's less flexible as you cannot edit individual lines.

---

#### Named ACL Configuration Mode (`config-ext-nacl`)

This is the sub-mode you enter after using the `ip access-list extended` command.

* **`[sequence#] [permit|deny] [protocol] [source] [destination] [operator] [port]`**
    This is the command used to write the actual filter rules. For example: `10 permit tcp any host 10.1.1.1 eq 443`.

* **`no [sequence#]`**
    This command allows you to **delete a single line** from the ACL without having to delete the entire list, which is the primary advantage of using named ACLs.

---

#### Interface Configuration Mode (`config-if`)

* **`ip access-group [NAME | number] [in|out]`**
    This command **activates the ACL** by applying it to an interface. You must specify the ACL's name or number and the direction (`in` or `out`) in which to filter traffic. An ACL does nothing until it's applied.

---

#### Privileged EXEC Mode (`#`)

* **`show access-lists`**
    This is the primary **verification command**. It displays all configured ACLs, their individual rules with sequence numbers, and a "hit counter" showing how many packets have matched each rule.

* **`show ip interface [name]`**
    This command is used to verify which ACLs are applied to a specific interface and in which direction.

