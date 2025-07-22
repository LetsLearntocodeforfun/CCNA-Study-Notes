#ipservices #DNS 

![[IMG_9846.jpeg]]


![[IMG_9847.jpeg]]

You can configure a Cisco router to use the Domain Name System (DNS) to resolve hostnames to IP addresses using the ip name-server and ip domain lookup commands.
### An In-Depth Guide to DNS

**CCNA Exam Objective:** 4.8 (Explain the forwarding per-hop behavior (PHB) for QoS such as classification, marking, queuing, congestion, policing, shaping) - *While not a direct objective, understanding DNS is a fundamental prerequisite for network connectivity.*

The **Domain Name System (DNS)** is the phonebook of the internet. Humans access information online through domain names like `google.com` or `cisco.com`. DNS is the hierarchical, distributed system that translates those human-friendly names into the machine-readable IP addresses (e.g., `142.250.72.14`) that computers use to connect to each other.

***

### DNS CLI Playbook 📖

| Command Type | Command | Purpose |
| :--- | :--- | :--- |
| **Configuration**| `ip name-server [server-ip-1] [server-ip-2]` | **The primary command.** Defines the IP addresses of the DNS servers the router should use. |
| **Configuration**| `ip domain lookup` | **Enables the DNS lookup feature.** This is on by default. |
| **Configuration**| `no ip domain lookup` | Disables the DNS lookup feature on the router. |
| **Configuration**| `ip domain-name [name]` | Appends a default domain name to unqualified hostnames. |
| **Verification** | `show hosts` | Displays the router's local cache of recently resolved domain names. |
| **Troubleshooting**| `ping [hostname]` | Tests DNS resolution and connectivity by pinging a domain name. |

***

### How DNS Works: The Lookup Process

When you type a domain name into your browser, your computer (or router) initiates a DNS query.

1.  **Client Query:** Your device sends a query to its configured DNS server (often called a **recursive resolver**), asking, "What is the IP address for `www.cisco.com`?"
2.  **Recursive Lookup:** If the recursive resolver doesn't have the answer cached, it begins a lookup process on your behalf.
    * It first asks a **Root Server** (`.`). The root server doesn't know the full answer but replies, "I don't know, but here's the IP for the `.com` server."
    * The resolver then asks the **Top-Level Domain (TLD) Server** for `.com`. The TLD server replies, "I don't know the full answer, but here's the IP for the `cisco.com` server."
    * Finally, the resolver asks the **Authoritative Name Server** for `cisco.com`. This server is the ultimate source of truth for that domain and provides the final IP address for `www.cisco.com`.
3.  **Response:** The recursive resolver sends the final IP address back to your device and caches the result for future queries.

---

### DNS Record Types

DNS doesn't just store IP addresses. It has various record types for different purposes.

| Record Type | Name | Purpose |
| :--- | :--- | :--- |
| **A** | Address | Maps a hostname to an **IPv4 address**. |
| **AAAA** | Quad A | Maps a hostname to an **IPv6 address**. |
| **CNAME**| Canonical Name | An alias that points one domain name to another (e.g., `ftp.cisco.com` might point to `www.cisco.com`). |
| **MX** | Mail Exchanger | Specifies the mail server responsible for accepting email for a domain. |

---

### Practical Router Configuration

Let's configure a router to use Google's public DNS servers (`8.8.8.8` and `8.8.4.4`) and to automatically append `.cisco.com` to any hostname that isn't fully qualified.

```cisco
! Enter global configuration mode
config t

! Define the primary and backup DNS servers
ip name-server 8.8.8.8 8.8.4.4

! Set the default domain name
ip domain-name cisco.com

! Ensure DNS lookup is enabled (it is by default)
ip domain lookup

With this configuration, if you type ping R1, the router will automatically try to resolve R1.cisco.com. If you type ping www.google.com, it will forward the request to 8.8.8.8 to get the IP address.

