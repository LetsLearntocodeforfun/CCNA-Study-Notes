#ntp #CLI 

![[IMG_9832.jpeg]]

![[IMG_9833.jpeg]]
### A Guide to the `ntp master` Command

**CCNA Exam Objective:** 4.7 (Configure and verify network time protocol (NTP))

The **`ntp master`** command configures a Cisco router to function as an **NTP server**, allowing it to provide time synchronization services to other devices on the network. When this command is enabled, the router becomes an authoritative source of time for its clients.

***

### When to Use `ntp master`

You would typically use this command in one of two scenarios:

1.  **Disconnected Networks:** In a secure, air-gapped network with no internet access, you cannot reach public NTP servers. In this case, you would manually set the time on one router and then configure it as an `ntp master`. All other devices on that private network would then point to this router as their time source.
2.  **Internal Time Hierarchy:** In a large enterprise, you don't want every single device trying to reach public internet NTP servers. The best practice is to have a few core routers synchronize with external servers, and then configure those core routers as internal NTP masters. All other client devices on the network then synchronize with these internal masters, which reduces traffic to the internet and creates a stable, local time source.

---

### CLI Guide & Configuration ⚙️

| Command | Mode | Purpose |
| :--- | :--- | :--- |
| `ntp master [stratum]` | Global Config | **Enables NTP server mode.** The router will start serving time to clients. |
| `show ntp status` | EXEC | Verifies the router's own clock status, including its stratum level. |
| `show ntp associations`| EXEC | Shows the status of NTP clients that are synchronized to this router. |

#### **How it Works**

By default, when you issue the `ntp master` command, the router assigns itself a stratum level of **8**. You can optionally specify a lower (better) stratum number. For example, if your router is synchronized to a public Stratum 2 server, its own stratum would become 3. You could then configure it as a master with a stratum of 3 for internal clients.

**Example: Creating a Disconnected Time Source**
```cisco
! First, manually set the clock since there is no internet source
clock set 06:50:00 21 Jul 2025

! Configure the router to be the authoritative time source for the network
! We'll set the stratum to 3 to be more authoritative than the default of 8.
ntp master 3
