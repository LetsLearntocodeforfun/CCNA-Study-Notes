#examday #VLAN #trunking #braindump
### VLAN & Trunking Command Reference

| Command                                            | Description                                                                        |
| -------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **VLAN Management (Switch)**                       |                                                                                    |
| `vlan [vlan-id]`                                   | Enters VLAN configuration mode to create or modify a VLAN.                         |
| `name [vlan-name]`                                 | (Inside vlan-config mode) Assigns a descriptive name to the VLAN.                  |
| `show vlan brief`                                  | Displays a summary of all created VLANs and their port assignments.                |
| **Access Port Configuration (Switch)**             |                                                                                    |
| `switchport mode access`                           | Sets the interface to be a permanent access port (belongs to one VLAN).            |
| `switchport access vlan [vlan-id]`                 | Assigns the access port to a specific VLAN.                                        |
| **Trunk Port Configuration (Switch)**              |                                                                                    |
| `switchport mode trunk`                            | Sets the interface to be a permanent trunk port (carries multiple VLANs).          |
| `switchport trunk allowed vlan [vlan-list]`        | Specifies which VLANs are permitted to cross the trunk (e.g., `10,20,30`).         |
| `switchport trunk native vlan [vlan-id]`           | Defines the untagged VLAN for the trunk link. Must match on both sides.            |
| `show interfaces trunk`                            | Displays detailed information about all trunking interfaces.                       |
| **Router-on-a-Stick (ROAS) Configuration**         |                                                                                    |
| `interface [physical-interface].[subinterface-id]` | Creates a virtual subinterface (e.g., `interface GigabitEthernet0/0.10`).          |
| `encapsulation dot1q [vlan-id]`                    | **(Required)** Sets the trunking protocol and links the subinterface to a VLAN ID. |
| `ip address [ip-address] [subnet-mask]`            | Assigns the gateway IP address to the subinterface.                                |
| `no shutdown`                                      | **(On the physical interface)** Activates the main port and all its subinterfaces. |

![[VLAN Cheat.pdf]]
