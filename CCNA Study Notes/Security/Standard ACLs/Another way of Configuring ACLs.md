#ACLs #security 

![[IMG_9799.jpeg]]

### A Guide to Standard ACL Configuration Methods

**CCNA Exam Objective:** 4.4 (Configure and verify standard access control lists)

Cisco IOS provides multiple ways to configure a standard Access Control List (ACL). While they achieve the same result, they differ in flexibility and ease of management. Understanding all three methods is key to both passing the CCNA and working on modern networking equipment.

***

### Standard ACL Configuration Cheat Sheet ⚙️

| Method | Configuration Command | Key Characteristic |
| :--- | :--- | :--- |
| **1. Traditional Numbered**| `access-list [1-99] [rule]` | The legacy method. Simple but rigid; cannot edit individual lines. |
| **2. Named** | `ip access-list standard [NAME]` | **The best practice.** Allows for descriptive names and editing of individual lines via sequence numbers. |
| **3. Modern Numbered**| `ip access-list standard [1-99]` | Uses the flexible named ACL syntax to configure a numbered ACL. |

***

### Method Breakdown

#### 1. Traditional Numbered ACL
This is the original method for creating ACLs. You define each rule from global configuration mode, and all rules for the same list must use the same number (from 1-99 for standard ACLs).

* **Drawback:** Its primary limitation is the inability to edit. If you need to remove one line, you must delete the entire ACL (`no access-list [number]`) and re-enter all the rules in the correct order.
* **Example:**
    ```cisco
    access-list 1 deny host 192.168.1.1
    access-list 1 permit any
    ```

#### 2. Named ACL
This is the modern, recommended best practice. It allows you to use a descriptive name and, most importantly, enter a sub-configuration mode where you can add, delete, and insert rules using sequence numbers, making it incredibly flexible.

* **Example:**
    ```cisco
    ip access-list standard BLOCK_PC1
     deny host 192.168.1.1
     permit any
    ```

#### 3. Modern Numbered ACL
As your slide correctly points out, modern Cisco IOS allows you to use the superior named ACL syntax to configure a numbered ACL. You get the benefit of the flexible sub-configuration mode while still creating a standard numbered list.

* **How it Works:** You use the `ip access-list standard` command but provide a number from the standard range (1-99) instead of a name.
* **Important Note:** Even though you configure it this way, when you look at the `show running-config` output, the router will display the ACL as if you had configured it using the traditional numbered method.
* **Example:**
    ```cisco
    ip access-list standard 1
     deny host 192.168.1.1
     permit any
    ```
This method gives you the best of both worlds: the ease of editing of a named ACL with the simplicity of a numbered ACL.
