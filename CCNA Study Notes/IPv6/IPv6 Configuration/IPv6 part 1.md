#ipv6 #examday #hexadecimal 

![[IMG_9740.jpeg]]

![[IMG_9741.jpeg]]
### Deconstructing the IPv6 Address

**CCNA Exam Objective:** 2.1 (Configure and verify IPv6 addressing and prefix)

An IPv6 address is a **128-bit** value, which is four times larger than an IPv4 address. To make it manageable, it's written as eight groups of four hexadecimal digits, separated by colons. Each of these 16-bit groups is called a **hextet**.

**Example Full Address:** `2001:0DB8:ACAD:0001:0000:0000:0000:0010`

---

### The Two Halves of an IPv6 Address
![[IMG_9742.jpeg]]

A standard unicast IPv6 address is logically split right down the middle into two 64-bit sections.

`[--------- Network Prefix (64 bits) --------] [--------- Interface ID (64 bits) ---------]`

#### **1. Network Prefix (The "Street")** 🛣️
The first 64 bits of the address identify the network. This part is used by routers to forward packets to the correct location. This prefix itself is often subdivided:
* **Global Routing Prefix (GRP):** Typically the first 48 bits (`/48`). This is the part of the address assigned to an organization by an Internet Registry or ISP. It's globally unique and routable on the internet. In our example, `2001:0DB8:ACAD` is the GRP.
* **Subnet ID:** The next 16 bits (`/48` to `/64`). This field is controlled by the network administrator to create internal subnets within the organization. In our example, `0001` is the Subnet ID.

This structure provides an organization with `65,536` subnets (`2^16`), which is a massive amount of internal network space.

#### **2. Interface ID (The "House Number")** 🏠
The last 64 bits of the address identify a specific host or device on a given subnet. This part is locally significant to a particular link and should be unique on that link. There are two common ways this ID is generated:

* **EUI-64 (Extended Unique Identifier):** A legacy method where a device's 48-bit MAC address is mathematically modified to create a 64-bit interface ID. While important to know about, it's less common now due to privacy concerns.
* **Random Generation:** Modern operating systems (Windows, macOS, Linux) typically generate a random 64-bit number for the interface ID. This is done for privacy reasons, so that a device's MAC address is not exposed in its IPv6 address.

This design gives every `/64` subnet `2^64` possible host addresses—a number so large it's practically inexhaustible.
