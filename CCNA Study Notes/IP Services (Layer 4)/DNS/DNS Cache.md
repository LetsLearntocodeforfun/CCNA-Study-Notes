#DNS #ipservices 

![[IMG_9850.jpeg]]

### An In-Depth Guide to the DNS Cache

**CCNA Exam Objective:** While not an explicit objective, understanding DNS caching is fundamental to Topic 4.0, IP Services.

The **DNS cache** is a crucial performance-enhancing feature built into networking devices and operating systems. It functions as a temporary database of recently resolved domain names and their corresponding IP addresses. When a new DNS query is made, the device checks its local cache first. If a valid entry exists, the device can use the stored IP address immediately, bypassing the entire, multi-step DNS lookup process.

![[IMG_9851.jpeg]]
***

### The Role of Time to Live (TTL) ⏳

A cached DNS record isn't stored forever. Every DNS record is configured with a **Time to Live (TTL)** value by the domain's administrator.

* **What it is:** The TTL is a timer, measured in seconds, that dictates how long a DNS resolver is allowed to store (or cache) that record.
* **How it works:** When a record is cached, the device starts counting down the TTL. Once the TTL reaches zero, the record is considered "stale" and is purged from the cache. The next time that domain is requested, the device must perform a full, live DNS query to get the most up-to-date record.
* **Why it matters:** A short TTL (e.g., 5 minutes) means changes to DNS records propagate quickly, but it also means more frequent queries. A long TTL (e.g., 24 hours) reduces query traffic but means changes take longer to be seen by users.

---

### DNS Cache CLI Playbook for Cisco IOS ⚙️

| Command | Mode | Purpose |
| :--- | :--- | :--- |
| **`show hosts`** | EXEC | **The primary verification command.** Displays all entries currently in the router's DNS cache, including their type, age, and IP address. |
| **`clear host all`**| EXEC | **Clears the entire DNS cache.** This is an essential troubleshooting step if you suspect a stale or incorrect entry is causing connectivity issues. |
| **`no ip host-cache`**| Global Config | Disables the DNS caching feature on the router. |
