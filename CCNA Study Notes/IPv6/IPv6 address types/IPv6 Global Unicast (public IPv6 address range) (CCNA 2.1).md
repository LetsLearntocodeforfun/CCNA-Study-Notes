#ipv6 #globalunicast


![[IMG_9763.jpeg]]

### An In-Depth Look at Global Unicast Addresses

**CCNA Exam Objective:** 2.1 (Configure and verify IPv6 addressing and prefix)

A **Global Unicast Address (GUA)** is the standard IPv6 address used for normal host-to-host communication across the internet. A device with a GUA is reachable globally, much like a device with a public IPv4 address. All currently assigned GUAs fall within the **`2000::/3`** range, meaning their first three bits are always `001`.

***

### The Structure of a Global Unicast Address

A typical `/64` GUA is logically divided into three hierarchical parts, providing a clear structure for routing and subnetting.

| GUA Component | Bit Range | Description |
| :--- | :--- | :--- |
| **Global Routing Prefix** | Bits 1-48 (`/48`) | This is the public prefix assigned to an entire organization by an Internet Registry or ISP. Routers on the internet use this portion to forward packets to the correct organization. |
| **Subnet ID** | Bits 49-64 (`/64`)| This 16-bit field is controlled by the local network administrator to create internal subnets. It allows for **65,536** individual subnets within an organization. |
| **Interface ID** | Bits 65-128 | This is the host portion of the address, uniquely identifying a device on a specific subnet. It can be auto-generated via SLAAC/EUI-64 or assigned by DHCPv6. |

**Visual Breakdown:**

`[-- Global Routing Prefix --] [-- Subnet ID --] [---------- Interface ID ----------]`
`2001:0DB8:ACAD` : `0001` : `1234:5678:9ABC:DEF0`

---

### How Devices Get a GUA

Unlike in IPv4 where DHCP is standard, IPv6 hosts have multiple ways to acquire a GUA.

* **SLAAC (Stateless Address Autoconfiguration):** This is the most common method. A host listens for **Router Advertisement (RA)** messages on its link. These RAs, sent by the local router, contain the `/64` network prefix. The host then takes this prefix and generates its own unique Interface ID to create a full GUA.
* **DHCPv6 (Stateful):** Similar to DHCP for IPv4, a DHCPv6 server can provide a host with its full IP address, default gateway, and DNS server information. This method is considered "stateful" because the server keeps a record of which address it has assigned to which device.
* **Static Configuration:** An administrator can manually assign a full GUA to a device, which is common for servers, printers, and network infrastructure like routers.
