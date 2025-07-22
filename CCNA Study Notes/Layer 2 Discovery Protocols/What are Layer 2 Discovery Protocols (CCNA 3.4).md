#CDP #LLDP #layer2

![[IMG_9809.jpeg]]

You can use Layer 2 discovery protocols like CDP and LLDP to automatically identify directly connected network devices and learn details about them, such as their device type, IP address, and platform.
### A Guide to Layer 2 Discovery Protocols

**CCNA Exam Objective:** 3.4 (Configure and verify Layer 2 discovery protocols (Cisco Discovery Protocol and LLDP))

**Layer 2 discovery protocols** are special protocols that allow network devices to advertise their identity and learn about their immediate neighbors on the same physical link. They are incredibly useful for network mapping, documentation, and troubleshooting, as they provide an automated way to see exactly what is plugged into each port.

***

### The Two Main Protocols: CDP vs. LLDP

| Feature | CDP (Cisco Discovery Protocol) | LLDP (Link Layer Discovery Protocol) |
| :--- | :--- | :--- |
| **Standard** | **Cisco Proprietary** | **IETF Open Standard** (IEEE 802.1AB) |
| **Use Case** | Works only between Cisco devices. | Works between devices from different vendors (e.g., a Cisco switch connected to a Juniper router). |
| **Default Status** | **Enabled** by default on most Cisco devices. | **Disabled** by default on most Cisco devices. |

---

### What Information is Shared? ℹ️

Both CDP and LLDP share similar types of information with their neighbors:
* **Device ID:** The hostname of the neighboring device.
* **Port ID:** The specific interface on the neighbor that you are connected to (e.g., `GigabitEthernet0/1`).
* **Capabilities:** The type of device (e.g., Router, Switch).
* **Platform:** The hardware model of the device (e.g., `Cisco 2960`).
* **IP Address:** The management IP address of the neighbor.

**Security Note:** As your slide indicates, because these protocols openly share information about your network infrastructure, they are often considered a security risk and are commonly disabled on publicly accessible or untrusted interfaces.

---

### CLI Playbook: CDP & LLDP Commands ⚙️

#### **Cisco Discovery Protocol (CDP)**

* **`show cdp neighbors [detail]`**
    This is the primary command. It displays a summary table of all directly connected Cisco devices. Adding the `detail` keyword provides an in-depth look at each neighbor, including their IP address and IOS version.

* **`no cdp run`**
    (Global command) Disables CDP on the entire device.

* **`cdp run`**
    (Global command) Enables CDP on the entire device.

* **`no cdp enable`**
    (Interface command) Disables CDP on a specific interface, which is a common security practice.

#### **Link Layer Discovery Protocol (LLDP)**

LLDP commands mirror CDP commands but are disabled by default.

* **`lldp run`**
    (Global command) **You must run this first** to enable LLDP globally.

* **`show lldp neighbors [detail]`**
    The LLDP equivalent of the CDP verification command.

* **`no lldp transmit`**
    (Interface command) Prevents the interface from sending LLDP advertisements.

* **`no lldp receive`**
    (Interface command) Prevents the interface from processing incoming LLDP advertisements.

