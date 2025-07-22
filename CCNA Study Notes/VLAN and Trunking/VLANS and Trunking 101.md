# CCNA Notes: VLANs, Trunking, & Inter-VLAN Routing

## VLAN (Virtual LAN) Fundamentals

#examday #VLAN #trunking #routeronastick

A **VLAN** is a way to logically divide a single physical switch into multiple, separate virtual switches. This is done to improve security and manage broadcast traffic. Devices in one VLAN cannot communicate with devices in another VLAN without a router.

### VLAN Creation & Naming

This is done in global configuration mode on the switch.

|Task|Command|Example|
|---|---|---|
|Create a VLAN|`vlan [vlan-id]`|`SW1(config)# vlan 10`|
|Name a VLAN|`name [vlan-name]`|`SW1(config-vlan)# name Engineering`|
|Verify VLANs|`show vlan brief`|`SW1# show vlan brief`|

## Switch Port Configuration

There are two primary types of switch ports when dealing with VLANs.

### Access Ports

An **access port** belongs to and carries traffic for only **one VLAN**. These are the ports that end devices like PCs, printers, and servers plug into.

**Configuration Steps:**

1. Enter the interface configuration mode.
    
2. Set the port mode to `access`.
    
3. Assign the port to a VLAN.
    

Cisco CLI

```
SW1(config)# interface FastEthernet0/1
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10
```

### Trunk Ports

A **trunk port** is a point-to-point link that can carry traffic for **multiple VLANs** simultaneously. Trunks are used to connect switches to other switches, or switches to routers in a Router-on-a-Stick configuration.

- **Trunking Protocol:** The open-standard protocol for trunking is **802.1q** (also called Dot1q). It works by adding a "tag" to the Ethernet frame that identifies which VLAN the frame belongs to.
    

**Configuration Steps:**

1. Enter the interface configuration mode.
    
2. Set the port mode to `trunk`.
    
3. (Optional but recommended) Specify which VLANs are allowed on the trunk.
    

Cisco CLI

```
SW1(config)# interface GigabitEthernet0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 10,20,30
```