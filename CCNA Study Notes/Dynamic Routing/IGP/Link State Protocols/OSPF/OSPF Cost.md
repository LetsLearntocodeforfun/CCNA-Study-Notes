
#OSPF #cost #Cisco #CLI #IGP 

![[IMG_9704.jpeg]]

###  OSPF Cost

OSPF uses a metric called **cost** to determine the best path to a destination. The path with the lowest total cost is always preferred. By default, this cost is automatically calculated based on the bandwidth of an interface—the higher the bandwidth, the lower the cost.

---

### How Cost is Calculated 💰

The cost of sending traffic out an interface is determined by a simple formula:

`Cost = Reference Bandwidth / Interface Bandwidth`

* **Reference Bandwidth:** A global value on the router that defaults to **100 Mbps**.
* **Interface Bandwidth:** The configured bandwidth of the specific interface.

**The Problem with the Default:**
With a 100 Mbps reference, any interface faster than 100 Mbps will have a calculated cost of 1, because OSPF only uses whole numbers.

* **FastEthernet (100 Mbps):** `100 / 100 = 1`
* **GigabitEthernet (1,000 Mbps):** `100 / 1000 = 0.1` → **Cost of 1**
* **10-GigabitEthernet (10,000 Mbps):** `100 / 10000 = 0.01` → **Cost of 1**

This makes it impossible for OSPF to differentiate between modern high-speed links. The solution is to change the reference bandwidth to a value higher than your fastest link.

---

### Essential CLI Commands

You have two main ways to control OSPF cost: globally by adjusting the reference bandwidth, or manually on a per-interface basis.

#### **1. Adjusting the Reference Bandwidth (Global)**

This is the recommended method for ensuring accurate path selection across your entire network. This command must be configured **on all routers** in your OSPF domain to ensure consistent calculations.

* **Command:**
    ```cisco
    router ospf 1
     auto-cost reference-bandwidth [Mbps]
    ```

* **Example (For a 10 Gbps Network):**
    Set the reference bandwidth to 10,000 Mbps (or higher).
    ```cisco
    router ospf 1
     auto-cost reference-bandwidth 10000
    ```
    Now, a 1 Gbps link will have a cost of 10 (`10000/1000`), and a 10 Gbps link will have a cost of 1, allowing OSPF to make an intelligent choice.

#### **2. Setting Cost Manually (Per-Interface)**

If you need to influence a specific path for traffic engineering purposes, you can manually override the calculated cost directly on an interface. This value takes precedence over the automatic calculation.

* **Command:**
    ```cisco
    interface [interface_name]
     ip ospf cost [value]
    ```

* **Example:**
    To make OSPF strongly prefer a specific Gigabit link, you can manually set its cost to be very low.
    ```cisco
    interface GigabitEthernet0/1
     ip ospf cost 5
    ```

#### **Verification**

Use the following command to quickly see the calculated cost on any OSPF-enabled interface.

* **Command:**
    ```cisco
    show ip ospf interface [interface_name]
    ```
    *(Look for the "Cost:" line in the output.)*
