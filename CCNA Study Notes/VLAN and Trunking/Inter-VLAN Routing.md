#VLAN #routeronastick #trunking 

For devices in different VLANs to communicate, a Layer 3 device (a router) is required. There are two primary methods to accomplish this.

### 1. Legacy Inter-VLAN Routing

This is the older method where you use a **separate physical router interface for each VLAN**.

- **Router Side:** Each physical interface is configured with an IP address from one of the VLAN's subnets. It acts as the default gateway for that VLAN.
    
    Cisco CLI
    
    ```
    R1(config)# interface GigabitEthernet0/0
    R1(config-if)# ip address 10.0.0.62 255.255.255.192
    R1(config-if)# no shutdown
    ```
    
- **Switch Side:** The corresponding switch port that connects to that router interface is configured as a simple **access port** in the same VLAN.
    
    Cisco CLI
    
    ```
    SW1(config)# interface GigabitEthernet0/1
    SW1(config-if)# switchport mode access
    SW1(config-if)# switchport access vlan 10
    ```
    

### 2. Router-on-a-Stick (ROAS)

This is the modern method that uses a **single physical router interface** connected to a **trunk port** on a switch. The router uses virtual **subinterfaces** to handle traffic for each VLAN.

- **Router Side:**
    
    1. Create a subinterface for each VLAN using a period (e.g., `.10` for VLAN 10).
        
    2. Set the encapsulation protocol to `dot1q` and specify the VLAN ID. **This must be done before assigning an IP address.**
        
    3. Assign the IP address (gateway) to the subinterface.
        
    4. Enable the main physical interface with `no shutdown`.
        
    
    Cisco CLI
    
    ```
    R1(config)# interface GigabitEthernet0/0.10
    R1(config-subif)# encapsulation dot1q 10
    R1(config-subif)# ip address 10.0.0.62 255.255.255.192
    R1(config-subif)# exit
    
    R1(config)# interface GigabitEthernet0/0
    R1(config-if)# no shutdown
    ```
    
- **Switch Side:** The single switch port that connects to the router must be configured as a **trunk port**.
    
    Cisco CLI
    
    ```
    SW1(config)# interface GigabitEthernet0/1
    SW1(config-if)# switchport mode trunk
    ```