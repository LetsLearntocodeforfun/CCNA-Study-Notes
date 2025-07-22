#ntp #CLI 

![[IMG_9826.jpeg]]

![[IMG_9837.jpeg]]

### An In-Depth Guide to NTP & Stratum Levels

**CCNA Exam Objective:** 4.7 (Configure and verify network time protocol (NTP))

The **Network Time Protocol (NTP)** is a client-server protocol designed to synchronize system clocks across a packet-switched network. It operates in a hierarchical fashion, using **stratum levels** to define the accuracy and distance from an authoritative time source. Accurate time across all devices is critical for log correlation, certificate validation, and authentication.

***

### NTP CLI Playbook 📖
![[IMG_9829.jpeg]]

![[IMG_9831.jpeg]]

| Command Type | Command | Purpose |
| :--- | :--- | :--- |
| **Client Config** | `ntp server [ip-address \| hostname]` | **The primary command.** Configures the device as a client to synchronize with a specified NTP server. |
| **Server Config**| `ntp master [stratum]` | Configures the device to act as an NTP server. You can optionally specify its stratum level. |
| **Authentication**| `ntp authenticate` | Globally enables NTP authentication. |
| **Authentication**| `ntp authentication-key [key#] md5 [key]`| Defines an authentication key and password. |
| **Authentication**| `ntp trusted-key [key#]` | Specifies which keys are trusted for authentication. |
| **Authentication**| `ntp server [ip] key [key#]`| Configures the client to use a specific key when communicating with a server. |
| **Verification** | `show ntp status` | **The best first command.** Shows if the clock is synchronized, the reference server, and the stratum level. |
| **Verification** | `show ntp associations [detail]`| Displays a detailed list of all NTP peers, their status, and statistics. |

***

### The NTP Hierarchy: Stratum Levels Explained

![[IMG_9828.jpeg]]
NTP's hierarchy prevents a single time source from being overwhelmed and ensures stability. The stratum number indicates how many hops a device is from a primary reference clock. **A lower stratum number is better.**

#### Stratum 0: The Source of Time ⚛️
This level is not on the network itself. It represents the high-precision reference clocks, such as **atomic clocks** or **GPS clocks**, that are the ultimate source of Coordinated Universal Time (UTC).

---

#### Stratum 1: The Primary Servers
A Stratum 1 server is a network device that is **directly connected** to a Stratum 0 time source. These servers are considered the most accurate and authoritative time sources available on the internet. Public NTP server pools (like `pool.ntp.org`) are composed of many Stratum 1 and 2 servers.

---

#### Stratum 2: Secondary Servers
A Stratum 2 device is a server or router that receives its time by synchronizing with a **Stratum 1 server**. A typical enterprise will configure its primary internal NTP server to sync with a public Stratum 1 or 2 server.

---

#### Stratum 3 and Beyond (Up to 15)
The hierarchy continues downwards. A device that syncs with a Stratum 2 server becomes a **Stratum 3** device. For example, all client devices and routers within an enterprise might point to their internal Stratum 2 server, making them Stratum 3 clients. The maximum valid stratum is **15**.

---

#### Stratum 16: Unsynchronized
A device with a stratum level of **16** is considered **unsynchronized** and cannot be used as a time source. If a device cannot reach any of its configured NTP servers, its stratum will become 16.
