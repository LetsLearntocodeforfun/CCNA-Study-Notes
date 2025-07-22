#ipv6 #hexadecimal 

![[IMG_9738.jpeg]]

![[IMG_9739.jpeg]]

### A Simple Guide to Hexadecimal Numbers

**CCNA Exam Objective:** 1.5 (Describe the need for private IPv4 addressing) & 2.1 (Configure and verify IPv6 addressing and prefix) - *Understanding hex is foundational for IPv6.*

**Hexadecimal** is just another way to count, but instead of using 10 digits (0-9) like we do in the decimal system, it uses **16 digits**. It's the numbering system used for IPv6 addresses because it's a much more compact and human-readable way to represent the long binary strings that make up an IPv6 address.

***

### The Core Concept: Counting Beyond 9 💡

In our normal decimal system, when we count past 9, we add another digit and start over (e.g., 10). Hexadecimal does the same thing, but it uses letters for the extra digits.

* **Decimal uses 10 digits:** `0, 1, 2, 3, 4, 5, 6, 7, 8, 9`
* **Hexadecimal uses 16 digits:** `0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F`

That's it! The letter 'A' simply represents the number 10, 'B' is 11, and so on, up to 'F' for 15.

---

### Hexadecimal to Decimal Conversion Chart

This is the most important chart to understand. Every other conversion is based on this.

| Decimal | Hexadecimal |
| :--- | :--- |
| 0 | 0 |
| 1 | 1 |
| 2 | 2 |
| 3 | 3 |
| 4 | 4 |
| 5 | 5 |
| 6 | 6 |
| 7 | 7 |
| 8 | 8 |
| 9 | 9 |
| **10** | **A** |
| **11** | **B** |
| **12** | **C** |
| **13** | **D** |
| **14** | **E** |
| **15** | **F** |

---

### The Link to Binary: The "Rule of Four" ✅

### A Clear Guide to Hexadecimal & Binary

**CCNA Exam Objective:** 2.1 (Configure and verify IPv6 addressing and prefix) - *Understanding hex is foundational for IPv6.*

**Hexadecimal** is a base-16 numbering system that uses 16 digits: `0-9` and `A-F`. It's essential for working with IPv6 because it provides a compact, human-readable way to represent the long binary strings that make up an IPv6 address.

***

### The "Rule of Four": The Link Between Hex & Binary ✅

The most critical concept to master is this: **Every one hexadecimal digit represents exactly four binary digits (a nibble).** This direct relationship is the entire reason hex is used for IPv6; it's a perfect shorthand for binary.

This conversion chart is your key to understanding that relationship.

| Decimal | Hexadecimal | 4-Bit Binary |
| :--- | :--- | :--- |
| 0 | 0 | `0000` |
| 1 | 1 | `0001` |
| 2 | 2 | `0010` |
| 3 | 3 | `0011` |
| 4 | 4 | `0100` |
| 5 | 5 | `0101` |
| 6 | 6 | `0110` |
| 7 | 7 | `0111` |
| 8 | 8 | `1000` |
| 9 | 9 | `1001` |
| 10 | **A** | `1010` |
| 11 | **B** | `1011` |
| 12 | **C** | `1100` |
| 13 | **D** | `1101` |
| 14 | **E** | `1110` |
| 15 | **F** | `1111` |

---

### Putting It Into Practice

Using the chart above, you can easily translate between hex and binary in groups of four.

**Example 1: Converting Hex to Binary**

Let's convert the hex value **`A8`**.

1.  Break it into individual hex digits: **A** and **8**.
2.  Find each digit on the chart and get its 4-bit binary value.
    * **A** = **`1010`**
    * **8** = **`1000`**
3.  Combine the two binary values.
    * `A8` in hex is `10101000` in binary.

**Example 2: Converting Binary to Hex**

Let's convert the binary value **`11000101`**.

1.  Break the binary string into groups of four, starting from the right: **`1100`** and **`0101`**.
2.  Find each 4-bit group on the chart and get its hex value.
    * **`1100`** = **`C`**
    * **`0101`** = **`5`**
3.  Combine the two hex digits.
    * `11000101` in binary is `C5` in hex.