#NDP #ipv6 

![[IMG_9775.jpeg]]

### A Guide to the Solicited-Node Multicast Address

**CCNA Exam Objective:** 2.1 (Configure and verify IPv6 addressing and prefix)

The **solicited-node multicast address** is a special, automatically generated multicast address that is used by the **Neighbor Discovery Protocol (NDP)** to perform Layer 2 MAC address resolution. In essence, it allows a device to ask, "Will the owner of this specific IPv6 address please tell me their MAC address?" without having to bother every other device on the local network.

***

### How It Works: Replacing Broadcast ARP 🎯

In IPv4, when a device needs to find the MAC address for a known IP address, it sends a broadcast **ARP request** to every single device on the local network. This is inefficient and creates unnecessary traffic.

IPv6 improves this with the solicited-node multicast address.
1.  For every unicast address it has, a device automatically creates and joins a corresponding solicited-node multicast group.
2.  When a device needs to find the MAC address for a neighbor, it sends a **Neighbor Solicitation** message not to a broadcast address, but to the specific solicited-node multicast address of that neighbor.
3.  Because of how Ethernet switches work, only the handful of devices that have joined that specific multicast group will even process the packet. In most cases, this is only the one target device.

This process is far more efficient as it doesn't interrupt the CPU of every host on the LAN.

---

### Creating the Address

A solicited-node multicast address is created by taking the **last 24 bits** of a device's unicast address and appending them to the prefix **`FF02::1:FF00:0/104`**.

**Example:**
Let's say a device has the unicast address `2001:DB8::1234:5678`.

1.  **Isolate the last 24 bits (6 hex digits):**
    `34:5678`

2.  **Append them to the prefix:**
    `FF02::1:FF` + `34:5678`

3.  **The resulting solicited-node address is:**
    `FF02::1:FF34:5678`

The device will now "listen" for any packets sent to this specific multicast address.
