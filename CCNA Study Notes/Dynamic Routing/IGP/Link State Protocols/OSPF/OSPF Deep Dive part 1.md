#OSPF #linkstate #CLI #dynamicrouting 

![[IMG_9698.jpeg]]

![[IMG_9700.jpeg]]
### Open Shortest Path First (OSPF)

**OSPF (Open Shortest Path First)** is arguably the most widely deployed and important Interior Gateway Protocol (IGP) in modern enterprise and service provider networks. It's a robust, scalable, and standardized link-state protocol that builds a complete map of the network to make intelligent, loop-free routing decisions. Mastering OSPF is non-negotiable for any serious network engineer.

***

### A Brief History 📜

OSPF was developed by the **Internet Engineering Task Force (IETF)** in the late 1980s as a direct replacement for the limited, distance-vector protocol RIP. The goal was to create a non-proprietary, scalable, and efficient routing protocol for the growing internet.

* **1989:** The initial OSPFv1 specification was published in **RFC 1131**. This version was experimental and never widely deployed.
* **1991:** **OSPFv2** was introduced in **RFC 1247**, adding significant improvements.
* **1998:** The definitive version of OSPFv2 was standardized in **RFC 2328**, which remains the primary reference for IPv4 implementations today.
* **2008:** **OSPFv3**, defined in **RFC 5340**, was released to provide support for **IPv6**. It functions largely the same as OSPFv2 but is adapted for IPv6 addressing and transport.

OSPF's status as an **open standard** is a key reason for its popularity, as it ensures interoperability between equipment from different vendors like Cisco, Juniper, Arista, and more.

***

### **The Ultimate OSPF CLI Command Guide**

This guide covers the most critical commands for configuring, verifying, and troubleshooting OSPF on Cisco IOS.

#### **Core Configuration Commands**

These commands are the absolute essentials for getting an OSPF process running.

| Command                                  | Purpose                                                                                                                                                                                                |
| :--------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `router ospf [process-id]`               | Enters OSPF configuration mode. The process ID (1-65535) is locally significant; it **does not** need to match between routers.                                                                        |
| `router-id [x.x.x.x]`                    | Manually sets a stable, unique 32-bit ID for the router. **This is a critical best practice.** If not set, OSPF will choose the highest IP on a loopback, then the highest IP on a physical interface. |
| `network [address] [wildcard] area [id]` | Enables OSPF on matching interfaces and assigns them to a specific area. **The wildcard mask and area ID are mandatory.**                                                                              |
| `passive-interface [interface]`          | Prevents an interface from sending OSPF Hellos, used for security on LAN-facing ports.                                                                                                                 |
| `default-information originate`          | Generates and advertises a default route (`0.0.0.0/0`) to other routers.                                                                                                                               |

**Example Basic Configuration:**
```cisco
R1(config)# router ospf 10
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 10.1.1.1 0.0.0.0 area 0
R1(config-router)# network 10.2.2.0 0.0.0.255 area 0
R1(config-router)# passive-interface GigabitEthernet0/1







