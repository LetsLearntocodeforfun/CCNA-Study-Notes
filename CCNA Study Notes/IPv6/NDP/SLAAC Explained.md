#SLAAC #ipv6 #NDP 

![[IMG_9780.jpeg]]

SLAAC (Stateless Address Autoconfiguration) is the default method in IPv6 for a device to automatically assign itself a global unicast address without needing a DHCP server.
### A Guide to SLAAC: IPv6 Plug-and-Play

**CCNA Exam Objective:** 2.3 (Describe the function of ICMPv6 NDP)

**Stateless Address Autoconfiguration (SLAAC)** is a core feature of the Neighbor Discovery Protocol (NDP) that allows an IPv6 device to join a network and automatically configure its own unique **Global Unicast Address (GUA)**. It's called "stateless" because no central server (like a DHCP server) needs to keep a record or "state" of which address has been assigned to which device.

***

### The SLAAC Process ⚙️

The entire process is managed through two ICMPv6 NDP messages: **Router Solicitation (RS)** and **Router Advertisement (RA)**.

1.  **Host Comes Online:** A device connects to a network and automatically generates its own **Link-Local Address (LLA)**.
2.  **Router Solicitation (RS):** The device sends an RS message from its LLA to the "all-routers" multicast address (`FF02::2`). This message is essentially the device asking, "Hey, can a router on this link please tell me about the network?"
3.  **Router Advertisement (RA):** The local router hears the RS and responds directly to the host with an RA message. This message contains crucial information, most importantly the **`/64` network prefix** for that specific link (e.g., `2001:DB8:ACAD:1::/64`).
4.  **Address Creation:** The host receives the RA and combines the two parts:
    * **The 64-bit Prefix from the RA:** `2001:DB8:ACAD:1::`
    * **A self-generated 64-bit Interface ID:** Created using EUI-64 or a random number.
5.  **Duplicate Address Detection (DAD):** Before using the address, the host performs DAD by sending a Neighbor Solicitation for the address it just created. If no one replies, the address is unique, and the host can now use its new GUA to communicate on the internet.

---

### SLAAC vs. DHCPv6

SLAAC is powerful, but it only provides a device with its IP address and prefix length. It does **not** provide other critical information like the address of a DNS server.

| Method | What it Provides | Use Case |
| :--- | :--- | :--- |
| **SLAAC** | IP Address, Prefix Length, Default Gateway | The default, simple method for host addressing. |
| **DHCPv6** | IP Address, DNS Server, Domain Name, etc. | A more "managed" approach needed when hosts require more than just an IP address. |

Modern networks often use a hybrid approach where a host gets its IP address via SLAAC but is told via a flag in the RA message to contact a DHCPv6 server to get DNS information.

