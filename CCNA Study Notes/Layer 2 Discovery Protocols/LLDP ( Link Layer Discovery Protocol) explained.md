#LLDP 

![[IMG_9815.jpeg]]

You can manage and verify the Link Layer Discovery Protocol (LLDP) using a set of lldp global and interface commands, with the show lldp neighbors detail command being the most important for viewing information about adjacent multi-vendor devices.
### An In-Depth Guide to LLDP Commands

**CCNA Exam Objective:** 3.4 (Configure and verify Layer 2 discovery protocols (Cisco Discovery Protocol and LLDP))

The **Link Layer Discovery Protocol (LLDP)** is the open-standard alternative to CDP. Its function is identical: to discover information about directly connected network devices. Because LLDP is vendor-neutral (IEEE 802.1AB), it's the essential discovery protocol for multi-vendor environments where, for example, a Cisco switch needs to identify a connected Juniper router.

***

### The LLDP CLI Playbook 📖
![[IMG_9817.jpeg]]
![[IMG_9816.jpeg]]

| Command Type | Command | Purpose |
| :--- | :--- | :--- |
| **Global Config**| `lldp run` | **Enables LLDP globally.** This is the mandatory first step, as LLDP is disabled by default. |
| **Global Config**| `no lldp run` | Disables LLDP entirely on the device. |
| **Interface Config**| `lldp transmit` | Enables the sending of LLDP advertisements on an interface. |
| **Interface Config**| `no lldp transmit`| **Disables sending** LLDP packets from a specific interface. |
| **Interface Config**| `lldp receive` | Enables the processing of received LLDP advertisements on an interface. |
| **Interface Config**| `no lldp receive`| **Disables receiving** LLDP packets on a specific interface. |
| **Verification** | `show lldp` | Displays global LLDP settings, such as timers. |
| **Verification** | `show lldp neighbors`| Displays a summary table of all directly connected neighbors. |
| **Verification** | `show lldp neighbors detail`| **The most useful command.** Provides an in-depth report for each neighbor. |
| **Verification** | `show lldp interface` | Shows the LLDP status for each interface on the device. |
| **Troubleshooting**| `clear lldp table` | Clears the table of LLDP neighbor information. |

***

### Key Configuration & Verification Steps

Unlike CDP, LLDP is **disabled by default** on Cisco devices. This means you must explicitly enable it.

#### **Step 1: Enable LLDP Globally**
This command is the master switch for LLDP. Without it, no other LLDP commands will have an effect.
```cisco
! Enter global configuration mode
config t

! Enable the LLDP process
lldp run

Step 2: Control Interface Behavior (Optional)
By default, once lldp run is configured, all interfaces will both transmit and receive LLDP packets. You can control this on a per-interface basis for security.
! Enter interface configuration mode
interface GigabitEthernet0/1

! To stop this interface from advertising itself
no lldp transmit

! To stop this interface from listening for neighbors
no lldp receive

Step 3: Verify Neighbors
The verification commands are nearly identical to their CDP counterparts. The detail keyword is the best way to get a full picture of your multi-vendor neighbors.
Example show lldp neighbors detail Output:
------------------------------------------------
Chassis id: 10.1.1.2
Port id: Gi0/0/0
Port Description: Link to R1
System Name: R2

System Description:
Cisco IOS Software, C1900 Software (C1900-UNIVERSALK9-M), Version 15.1(4)M4

Time remaining: 101 seconds
System Capabilities: B, R
Enabled Capabilities: R
Management Addresses:
    IP: 10.1.1.2

This output provides everything you need to identify the neighboring device, including its management IP address, hostname (System Name), and hardware/software details.

Step 2: Control Interface Behavior (Optional)
By default, once lldp run is configured, all interfaces will both transmit and receive LLDP packets. You can control this on a per-interface basis for security.
! Enter interface configuration mode
interface GigabitEthernet0/1

! To stop this interface from advertising itself
no lldp transmit

! To stop this interface from listening for neighbors
no lldp receive

Step 3: Verify Neighbors
The verification commands are nearly identical to their CDP counterparts. The detail keyword is the best way to get a full picture of your multi-vendor neighbors.
Example show lldp neighbors detail Output:
------------------------------------------------
Chassis id: 10.1.1.2
Port id: Gi0/0/0
Port Description: Link to R1
System Name: R2

System Description:
Cisco IOS Software, C1900 Software (C1900-UNIVERSALK9-M), Version 15.1(4)M4

Time remaining: 101 seconds
System Capabilities: B, R
Enabled Capabilities: R
Management Addresses:
    IP: 10.1.1.2

This output provides everything you need to identify the neighboring device, including its management IP address, hostname (System Name), and hardware/software details.