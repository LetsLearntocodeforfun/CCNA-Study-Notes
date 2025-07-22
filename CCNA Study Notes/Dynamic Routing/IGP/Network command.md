#CLI #rip #eigrp #Cisco #IGP #OSPF #OSPF  #network

![[IMG_9687 1.jpeg]]

Of course. Here is the complete note about the network command, consolidated into a single markdown block for easy copying.
### The `network` Command in IGPs

**TLDR:** The `network` command has two main jobs: **1)** It tells a router's interfaces to start "speaking" a specific routing protocol, and **2)** It tells the router which connected networks to advertise to its neighbors. Its syntax and behavior change slightly depending on the protocol.

***

### **CLI Command Cheat Sheet**

| Protocol | Example Command Syntax | Notes |
| :--- | :--- | :--- |
| **RIP** | `network 10.0.0.0` | Always uses a classful network address. No subnet mask or wildcard mask. |
| **EIGRP** | `network 10.1.1.0 0.0.0.255` | Wildcard mask is optional but recommended for precision. Can be classful if mask is omitted. |
| **OSPF** | `network 10.1.1.0 0.0.0.255 area 0` | **Wildcard mask and area ID are mandatory.** This is the most specific syntax. |

***

### **Core Concept: What Does `network` Actually Do?**

The `network` command is one of the most fundamental commands in router configuration, but its function is often misunderstood. It does **not** simply "advertise a network." Instead, it functions as a matching tool.

When you enter a `network` command, the router performs the following logic:

1.  **Interface Matching:** The router looks at the list of all its configured interfaces (e.g., GigabitEthernet0/0, Serial0/0/1).
2.  **Find a Match:** It compares the IP address of each interface against the network address and mask in your `network` command.
3.  **Activate & Advertise:** If an interface's IP address falls within the range specified by the `network` command, two things happen:
    * **Activate Protocol:** The router enables the IGP (RIP, EIGRP, OSPF) on that specific interface. This means it will start sending and listening for that protocol's hello/update packets on the interface.
    * **Advertise Network:** The router takes the network prefix assigned to that matched interface (e.g., `10.1.1.0/24`) and prepares to advertise it to any neighbors it discovers.

***

### **Protocol-Specific Examples & Syntax**

The precise behavior of the command changes slightly between protocols. Let's assume a router has an interface configured with the IP address `172.16.10.1` and a subnet mask of `255.255.255.0`.

#### **RIP Configuration**

RIP is **classful**. This means you must enter the major class A, B, or C network address. It does not use wildcard masks.

```cisco
router rip
 version 2
 ! Even though the interface is 172.16.10.1, you must use the class B address.
 network 172.16.0.0

Result: The router sees that interface 172.16.10.1 is part of the 172.16.0.0 class B network. It enables RIP on that interface and advertises the 172.16.10.0/24 network.
EIGRP Configuration
EIGRP is classless and more flexible. You can use a classful address, but it's better practice to be specific using a wildcard mask.
 * Method 1 (Classful - Lazy):
   router eigrp 100
 network 172.16.0.0

   This works, but it would accidentally enable EIGRP on all interfaces within the 172.16.0.0 range.
 * Method 2 (Specific Subnet - Better):
   router eigrp 100
 ! The wildcard mask 0.0.0.255 matches any address in the 172.16.10.0/24 subnet.
 network 172.16.10.0 0.0.0.255

 * Method 3 (Specific Interface - Best Practice):
   router eigrp 100
 ! The wildcard mask 0.0.0.0 matches only the exact IP address 172.16.10.1.
 network 172.16.10.1 0.0.0.0

OSPF Configuration
OSPF is the strictest. The network command must include a wildcard mask and an area ID. There is no classful shortcut.
router ospf 1
 ! You must specify the wildcard and the area this interface belongs to.
 network 172.16.10.0 0.0.0.255 area 0

Result: The router enables OSPF on any interface within the 172.16.10.0/24 range and places it into area 0. The network 172.16.10.0/24 is then advertised within Area 0. The interface-specific method (network 172.16.10.1 0.0.0.0 area 0) is also very common and considered a best practice for OSPF.


