#examday #portnumbers #tcp #udp 

![[IMG_9732.jpeg]]

### In-Depth Guide to Port Numbers

**CCNA Exam Objective:** 2.5 (Explain the role of port numbers)

Port numbers are the heart of Layer 4 communication, serving as the crucial addressing mechanism that directs data to the correct application on a host. While an IP address gets a packet to the right computer, the **port number** gets that packet to the right *program* running on that computer. Mastering common port numbers is essential for network configuration, security, and troubleshooting.

***

### The Analogy: IP Addresses & Port Numbers 📬

Think of a large apartment building.
* **The IP Address** is the street address of the building itself (e.g., 123 Main Street). It gets the mail truck to the correct location.
* **The Port Number** is the specific apartment or mailbox number inside that building (e.g., Apt #443). It ensures the mail for a specific resident (an application like a web server) gets put in the right box.

Without port numbers, the building's mail carrier (the operating system) would receive a stack of letters and have no idea which resident (application) to give them to. This process of managing multiple conversations using port numbers is called **session multiplexing**.

---

### Port Number Ranges

Port numbers are divided into three ranges, managed by the Internet Assigned Numbers Authority (IANA).

| Range | Name | Numbers | Purpose & Key Facts |
| :--- | :--- | :--- | :--- |
| **Well-Known Ports** | System Ports | **0 - 1023** | Reserved for common, standardized services and protocols (e.g., HTTP, FTP, SSH). Administrator privileges are typically required to run an application on these ports. |
| **Registered Ports** | User Ports | **1024 - 49151** | Assigned by IANA for specific applications and services (e.g., Microsoft SQL, Minecraft). Any user can run an application on these ports. |
| **Dynamic Ports** | Private / Ephemeral Ports | **49152 - 65535**| Used by client applications as their **temporary source port** when initiating a connection to a server. This port is randomly assigned for the duration of the session. |

---

### Essential Port Numbers for the CCNA

This chart expands on your slide, providing the must-know port numbers for the exam.

| Protocol | Port | Transport | Purpose |
| :--- | :--- | :--- | :--- |
| **FTP** | **20 & 21** | TCP | File Transfer Protocol (21 for control, 20 for data). |
| **SSH** | **22** | TCP | Secure Shell - Provides secure remote command-line access. |
| **Telnet** | **23** | TCP | Provides insecure remote command-line access. |
| **SMTP**| **25** | TCP | Simple Mail Transfer Protocol - Used for sending email. |
| **DNS** | **53** | **TCP & UDP**| Domain Name System - Resolves domain names to IP addresses. |
| **DHCP**| **67 & 68** | UDP | Dynamic Host Configuration Protocol - Assigns IP addresses to clients. |
| **TFTP**| **69** | UDP | Trivial File Transfer Protocol - A simple, connectionless file transfer protocol. |
| **HTTP**| **80** | TCP | Hypertext Transfer Protocol - The foundation of the World Wide Web. |
| **POP3**| **110** | TCP | Post Office Protocol v3 - Used for retrieving email. |
| **SNMP**| **161 & 162** | UDP | Simple Network Management Protocol - Used for network device monitoring. |
| **HTTPS**| **443** | TCP | HTTP Secure - The encrypted version of HTTP, using TLS/SSL. |
| **Syslog**| **514** | UDP | A standard for sending system log messages to a central server. |

#### **Why is DNS both TCP and UDP?**
* **UDP 53:** Used for standard, fast client queries (e.g., "What is the IP for google.com?").
* **TCP 53:** Used for **zone transfers** between DNS servers, where the complete reliability of TCP is required to transfer the entire database of records.
