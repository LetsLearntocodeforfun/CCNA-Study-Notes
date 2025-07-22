#ACLs #security 


![[IMG_9806.jpeg]]

### ACL Placement Strategy: Standard vs. Extended

**CCNA Exam Objective:** 4.4 & 4.5 (Configure and verify standard and extended access control lists)

Where you apply an Access Control List (ACL) is as important as the rules within it. Proper placement ensures the ACL works as intended without causing unintended side effects and that your network runs efficiently. The rule is simple and based entirely on the ACL type.

***

### The Golden Rules of ACL Placement

| ACL Type | Placement Rule | The Reason Why (The "So What?") |
| :--- | :--- | :--- |
| **Standard ACL** | As **close to the destination** as possible. | It can only filter by source IP. Placing it near the source might block that traffic from reaching other, legitimate destinations. |
| **Extended ACL**| As **close to the source** as possible. | It's specific enough to not cause collateral damage. This stops unwanted traffic early, saving network bandwidth and resources. |

---

#### Standard ACLs: Place Close to the Destination 📍

A **standard ACL** is a blunt instrument; it only sees the **source IP address**.

Imagine you want to stop `User A` from accessing `Server X`. If you place a standard ACL that denies `User A` at their own router (the source), you've not only blocked them from `Server X`, but you've also blocked them from accessing `Server Y`, `Server Z`, and the internet, which was not your intention.

By placing the ACL on the router interface directly connected to `Server X` (the destination), you ensure the rule only affects traffic trying to reach that specific server.

---

#### Extended ACLs: Place Close to the Source ➡️

An **extended ACL** is a surgical tool. It can filter on source IP, destination IP, protocol, and port numbers.

Because you can be incredibly specific (e.g., `deny tcp host UserA host ServerX eq 80`), there's no risk of collateral damage. You can place this rule on the very first router `User A`'s traffic hits.

The benefit of this is **efficiency**. You stop the unwanted traffic at the front door. It doesn't waste bandwidth crossing your entire network only to be dropped right at the end. This saves router processing power and link capacity across your infrastructure.
