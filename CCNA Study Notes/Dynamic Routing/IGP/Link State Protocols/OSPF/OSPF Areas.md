#OSPF #dynamicrouting #Cisco 

![[IMG_9701.jpeg]]

###  OSPF Areas

OSPF areas are a powerful tool for creating scalable and stable networks. By dividing a large domain into smaller, hierarchical areas connected to a central **backbone (Area 0)**, you dramatically reduce router resource usage and isolate the impact of network changes.

---

### Why Use Areas? The Scalability Solution

Without areas, every router in a large network gets overwhelmed.
* **High CPU/Memory Usage:** Every router must store the entire network map (LSDB) and constantly re-run the complex SPF algorithm for any change.
* **Network Instability:** A single flapping link forces every router to recalculate, causing widespread churn.

**Areas fix this.** Routers only need the detailed map for their own area and receive summarized information about others. A problem in one area no longer affects the stability of another.

---

### OSPF Area & Router Types Cheat Sheet

| Type | Role & Key Function |
| :--- | :--- |
| **Backbone Area (Area 0)** | The heart of the network. All other areas must connect to it. |
| **Area Border Router (ABR)** | Connects areas (e.g., Area 0 and Area 1). Its job is to **summarize** routes and **filter** LSAs. |
| **AS Boundary Router (ASBR)**| Connects OSPF to an external network (like the Internet or an EIGRP domain) and injects external routes. |
| **Stub Area** | A simplified area for spoke networks. **Blocks all external routes (Type 5 LSAs)** and replaces them with a single **default route** from the ABR. |
| **Totally Stubby Area** | A Cisco-proprietary, even simpler area. **Blocks external AND inter-area routes (Type 3, 4, & 5 LSAs)**, leaving only a single **default route**. Maximum efficiency. |
| **Not-So-Stubby Area (NSSA)** | A special stub area that is allowed to have an ASBR within it. It uses a special **Type 7 LSA** to pass its external routes to the ABR. |

---

### Essential CLI Commands

#### **Configuration**

The area type must be configured on **all routers within that area**. The `no-summary` command is only needed on the ABR.

* **Configure a Stub Area:**
    ```cisco
    router ospf 10
     area 1 stub
    ```
* **Configure a Totally Stubby Area:**
    ```cisco
    ! On the ABR for Area 1
    router ospf 10
     area 1 stub no-summary
    
    ! On all other routers inside Area 1
    router ospf 10
     area 1 stub
    ```

#### **Verification**

* `show ip route ospf`
    The best way to see the result. In a **stub area**, you'll see a default route marked `O*IA`. In a **totally stubby area**, all `O IA` routes will also be gone, leaving only the default route.

* `show ip ospf`
    Quickly confirms if a router is an ABR and lists the areas it's connected to.
