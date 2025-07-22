#ntp #CLI 

![[IMG_9834.jpeg]]

You can configure NTP symmetric active mode using the ntp peer command to create a redundant, mutually synchronizing relationship between two time servers.
### A Guide to NTP Symmetric Active Mode

**CCNA Exam Objective:** 4.7 (Configure and verify network time protocol (NTP))

NTP **symmetric active mode** is a special configuration used between two or more NTP servers that you want to act as peers. Unlike a standard client/server relationship, in symmetric active mode, both devices can synchronize with each other. This creates a highly redundant and stable timekeeping backbone for your network.

***

### Why Use Symmetric Active Mode? 🤔

You use this mode to create a **mutually redundant pair of time servers**.

Imagine you have two core routers, R1 and R2, that both sync to public internet time sources. You want them to also use each other as a backup time source. If R1 loses its connection to the internet, it can then sync its clock to R2, and vice versa.

In a client/server model (`ntp server`), the client always trusts the server. In symmetric active mode (`ntp peer`), the routers will compare their stratum levels, and the router with the **lower (better) stratum** will become the time source for the other, ensuring the most accurate clock is always used.

---

### CLI Guide & Configuration ⚙️

| Command | Mode | Purpose |
| :--- | :--- | :--- |
| `ntp peer [ip-address]` | Global Config | **Configures symmetric active mode** between this router and the specified peer. |
| `show ntp associations`| EXEC | Verifies the peer relationship. The `~` symbol next to the peer indicates symmetric active mode. |

#### **Example Configuration**

Let's configure R1 and R2 to be NTP peers. Both are also configured to sync with a public server.

**Configuration on R1:**
```cisco
! Sync to a public server
ntp server 1.1.1.1

! Configure R2 as a peer
ntp peer 10.1.2.2

Configuration on R2:
! Sync to a different public server for redundancy
ntp server 2.2.2.2

! Configure R1 as a peer
ntp peer 10.1.2.1

Result:
Now, R1 and R2 will continuously exchange NTP messages. If, for example, R1 has a stratum of 3 and R2 has a stratum of 4, R1 will act as the source for R2. If R1 loses its public connection and its stratum becomes 16, R2 (with stratum 4) will automatically take over and become the time source for R1.

