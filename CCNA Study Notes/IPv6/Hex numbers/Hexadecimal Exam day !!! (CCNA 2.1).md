#ipv6 #hexadecimal #examday 

### Converting Hex & Binary Without a Calculator

**CCNA Exam Objective:** 2.1 (Configure and verify IPv6 addressing and prefix)

The key to performing fast hex-to-binary conversions for the CCNA exam is to master the 4-bit binary place values. You don't need to memorize a large chart; you only need to know these four numbers: **8, 4, 2, 1**.

***

### The 8-4-2-1 Method

Every hexadecimal digit (0-F) can be represented by a 4-bit binary number. Each position in that 4-bit number has a set value. From left to right, they are **8, 4, 2, and 1**.

`[8] [4] [2] [1]`

To find the value of a 4-bit binary number, you just add up the place values that have a **`1`** in their position.

* `1000` = **8**
* `0100` = **4**
* `0010` = **2**
* `0001` = **1**

---

### Converting from Binary to Hexadecimal

This is the easiest conversion. Simply break the binary string into 4-bit chunks (nibbles) from right to left and convert each chunk.

**Example: Convert `11101011` to Hex**

1.  **Split the binary string** into 4-bit groups: `1110` and `1011`.
2.  **Convert the first group (`1110`):**
    * Place values: `[8] [4] [2] [1]`
    * Binary string: `1   1   1   0`
    * Add them up: `8 + 4 + 2 + 0 = 14`. In hex, 14 is **E**.
3.  **Convert the second group (`1011`):**
    * Place values: `[8] [4] [2] [1]`
    * Binary string: `1   0   1   1`
    * Add them up: `8 + 0 + 2 + 1 = 11`. In hex, 11 is **B**.
4.  **Combine the results:** `11101011` is **EB** in hex.

***

### Converting from Hexadecimal to Binary

This is the reverse process. For each hex digit, figure out which of the 8, 4, 2, and 1 values you need to "turn on" (set to `1`) to add up to its decimal value.

**Example: Convert `C9` to Binary**

1.  **Split the hex value** into individual digits: **C** and **9**.
2.  **Convert the first digit (`C`):**
    * `C` is 12 in decimal.
    * How do you make 12 using 8, 4, 2, and 1? You need `8 + 4`.
    * So, turn on the '8' and '4' bits: `1100`.
3.  **Convert the second digit (`9`):**
    * `9` is 9 in decimal.
    * How do you make 9 using 8, 4, 2, and 1? You need `8 + 1`.
    * So, turn on the '8' and '1' bits: `1001`.
4.  **Combine the results:** `C9` in hex is `11001001` in binary.
