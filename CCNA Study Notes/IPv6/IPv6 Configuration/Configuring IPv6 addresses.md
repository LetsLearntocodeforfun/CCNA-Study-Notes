#ipv6 #CLI #examday 


![[IMG_9743.jpeg]]

![[IMG_9744 2.jpeg]]

### Guide to Basic IPv6 Configuration on Cisco IOS

**CCNA Exam Objective:** 2.1 (Configure and verify IPv6 addressing and prefix)

Configuring IPv6 on a Cisco router is a straightforward process that involves enabling the protocol globally and then configuring addresses on each interface. This guide covers the essential commands to get your router running on IPv6.

***

### The Ultimate IPv6 CLI Cheat Sheet ⚙️

| Command                         | Purpose                                                                                                                   |
| :------------------------------ | :------------------------------------------------------------------------------------------------------------------------ |
| `ipv6 unicast-routing`          | **Enables IPv6 routing globally.** This is the essential first step.                                                      |
| `ipv6 address [address/prefix]` | Manually assigns a full static IPv6 address to an interface.                                                              |
| `ipv6 address [prefix] eui-64`  | Assigns the network prefix and tells the router to generate the host portion using the EUI-64 method.                     |
| `ipv6 address autoconfig`       | Tells the router to learn its full address automatically using SLAAC from another router on the link.                     |
| `ipv6 enable`                   | Enables IPv6 processing on an interface and generates a link-local address, but does not assign a global unicast address. |
| `show ipv6 interface brief`     | **The primary verification command.** Displays a summary of all interfaces, their IPv6 addresses, and their status.       |
| `show ipv6 route`               | Displays the IPv6 routing table.                                                                                          |
| `ping ipv6 [address]`           | Tests connectivity to another IPv6 address.                                                                               |

***

### Command Breakdown & Explanations

#### `ipv6 unicast-routing`
This is the master switch that turns on IPv6 routing for the entire device. It is configured in **global configuration mode**. Without this command, the router will not forward any IPv6 packets between its interfaces, even if they have IPv6 addresses configured.

---

#### `ipv6 address [address/prefix-length]`
This is the most common method for statically assigning a specific IPv6 address to an interface. It gives you complete manual control.

* **Use Case:** Assigning a known, predictable address to a server-facing interface or a router-to-router link.
* **Example:**
    ```cisco
    interface GigabitEthernet0/1
     ipv6 address 2001:DB8:ACAD:1::1/64
    ```

---

#### `ipv6 address [prefix/prefix-length] eui-64`
This command is a semi-automated method. You define the 64-bit network portion of the address, and the router automatically calculates the 64-bit host portion based on the interface's MAC address using the **EUI-64** process.

* **Use Case:** Quickly deploying addresses on router interfaces without having to manually type out the full 128-bit address.
* **Example:**
    ```cisco
    interface GigabitEthernet0/1
     ipv6 address 2001:DB8:ACAD:1::/64 eui-64
    ```

---

#### `ipv6 address autoconfig`
This command configures an interface as a **SLAAC (Stateless Address Autoconfiguration)** client. The interface will listen for Router Advertisement (RA) messages from another router on the link, learn the network prefix from that RA, and then generate its own host portion to create a full global unicast address.

* **Use Case:** Configuring a "client" router in a lab or a small office that gets its addressing information from an upstream ISP router.
* **Example:**
    ```cisco
    interface GigabitEthernet0/1
     ipv6 address autoconfig
    ```
