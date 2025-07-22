#CDP #Cisco  #layer2

![[IMG_9810.jpeg]]

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

### CLI Playbook: CDP Commands ⚙️

![[IMG_9814.jpeg]]
![[IMG_9811.jpeg]]
#### **Cisco Discovery Protocol (CDP)**

* **`show cdp neighbors [detail]`**
    This is the primary command. It displays a summary table of all directly connected Cisco devices. Adding the `detail` keyword provides an in-depth look at each neighbor, including their IP address and IOS version.

* **`no cdp run`**
    (Global command) Disables CDP on the entire device.

* **`cdp run`**
    (Global command) Enables CDP on the entire device.

* **`no cdp enable`**
    (Interface command) Disables CDP on a specific interface, which is a common security practice.

### An In-Depth Guide to CDP Commands

**CCNA Exam Objective:** 3.4 (Configure and verify Layer 2 discovery protocols (Cisco Discovery Protocol and LLDP))

The **Cisco Discovery Protocol (CDP)** is an essential tool for network administrators working in a Cisco environment. It provides a simple way to map out a network and verify physical connections by gathering information about directly connected Cisco devices. Mastering the CDP command set is crucial for efficient troubleshooting and documentation.

***

![[IMG_9812.jpeg]]
### The CDP CLI Playbook 📖

| Command Type | Command | Purpose |
| :--- | :--- | :--- |
| **Global Config**| `cdp run` | Enables CDP globally on the device (it is on by default). |
| **Global Config**| `no cdp run` | **Disables CDP entirely** on the device. |
| **Global Config**| `cdp timer [seconds]` | Changes how often CDP advertisements are sent (default is 60 seconds). |
| **Global Config**| `cdp holdtime [seconds]`| Changes how long a device will hold neighbor info before discarding it (default is 180 seconds). |
| **Interface Config**| `cdp enable` | Enables CDP on a specific interface if it was disabled. |
| **Interface Config**| `no cdp enable`| **Disables CDP on a single interface.** This is a common security practice. |
| **Verification** | `show cdp` | Displays global CDP settings, including the timer and holdtime values. |
| **Verification** | `show cdp neighbors`| Displays a summary table of directly connected Cisco devices. |
| **Verification** | `show cdp neighbors detail`| **The most useful command.** Provides an in-depth report for each neighbor. |
| **Verification** | `show cdp interface` | Shows the CDP status for each interface on the device. |
| **Verification** | `show cdp traffic` / `entry` | Displays CDP packet statistics or a specific neighbor entry. |
| **Troubleshooting**| `clear cdp table` | Clears the table of CDP neighbor information. |
| **Troubleshooting**| `debug cdp [packets \| events]`| Provides real-time debugging output for CDP. |

***
