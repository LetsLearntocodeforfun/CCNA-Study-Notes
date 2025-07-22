#CLI #DNS #ipservices 

![[IMG_9852.jpeg]]

### An In-Depth Guide to DNS Configuration on Cisco IOS

**CCNA Exam Objective:** While not an explicit objective, configuring DNS is a fundamental skill for Topic 4.0, IP Services.

Configuring a Cisco router to use DNS allows it to resolve domain names (like `cisco.com`) into IP addresses. This is essential for management tasks, troubleshooting (e.g., `ping www.google.com`), and for using features that rely on hostnames.

***

### DNS Configuration CLI Playbook 📖
![[IMG_9854.jpeg]]

| Command | Mode | Purpose |
| :--- | :--- | :--- |
| **`ip domain lookup`** | Global Config | **Enables the DNS lookup feature.** This is the master switch and is on by default. |
| **`ip name-server [ip1] [ip2]...`**| Global Config | **Defines the DNS servers.** This is the most important command; it tells the router where to send its queries. |
| **`ip domain-name [name]`**| Global Config | Appends a default domain suffix to unqualified names. |
| **`show hosts`** | EXEC | Displays the router's local DNS cache. |
| **`ping [hostname]`** | EXEC | A great way to test if DNS resolution is working correctly. |

***

### The Configuration Process ⚙️

#### Step 1: Enable DNS Lookup
DNS lookups are enabled by default on a Cisco router. However, if they have been disabled, you must re-enable them.

* **Command:** `ip domain lookup`
* **To Disable:** `no ip domain lookup` (A common mistake is to mistype a command, causing the router to try and resolve it as a hostname. Some administrators disable lookups to prevent this.)

---

#### Step 2: Define the Name Servers
This is the most critical step. You must tell the router the IP address of at least one DNS server to which it can send queries. You can specify multiple servers for redundancy.

* **Command:** `ip name-server [server1-ip] [server2-ip]`
* **Example:**
    ```cisco
    ! Use Google's public DNS servers
    ip name-server 8.8.8.8 8.8.4.4
    ```

---

#### Step 3: Set a Default Domain Name (Optional)
![[IMG_9853.jpeg]]
This command provides a convenient shortcut. It tells the router to automatically append a specific domain name to any hostname you type that isn't fully qualified.

* **Command:** `ip domain-name [name]`
* **Example:**
    ```cisco
    ip domain-name mycompany.com
    ```
    Now, if you type `ping server1`, the router will automatically try to resolve `server1.mycompany.com`.

---

### Verification

The best way to verify your DNS configuration is to simply use it. Ping a well-known external domain name. If the router displays "Translating..." and then provides an IP address, your configuration is working.

**Example:**
```cisco
R1# ping [www.cisco.com](https://www.cisco.com)
Translating "[www.cisco.com](https://www.cisco.com)"...
[OK]
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 104.85.129.135, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 10/12/15 ms
