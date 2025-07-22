#DNS #ipservices #dnsrecord

![[IMG_9848.jpeg]]

You need to know several key DNS record types for the CCNA, including A and AAAA for IP addresses, MX for mail servers, CNAME for aliases, and NS for authoritative servers.
### A Guide to Essential DNS Record Types

**CCNA Exam Objective:** While not an explicit objective, understanding DNS records is fundamental to Topic 4.0, IP Services.

The Domain Name System (DNS) uses different types of records to provide different kinds of information. While there are many record types, there are five that are essential to know for the CCNA.

![[IMG_9849.jpeg]]
***

#### A Record (Address Record)
* **Purpose:** The most common record type. It maps a hostname directly to an **IPv4 address**.
* **Analogy:** This is the primary phone book entry that links a person's name to their phone number.
* **Example:**
    * **Hostname:** `www.cisco.com`
    * **A Record:** `104.85.129.135`

---

#### AAAA Record (Quad A Record)
* **Purpose:** The IPv6 equivalent of an A record. It maps a hostname to an **IPv6 address**.
* **Analogy:** This is the entry for a person's international phone number.
* **Example:**
    * **Hostname:** `www.google.com`
    * **AAAA Record:** `2607:f8b0:4009:812::2004`

---

#### CNAME Record (Canonical Name)
* **Purpose:** An alias record. It maps one hostname to another hostname. This is used when you want multiple names to point to the same server.
* **Analogy:** This is a "see also" reference in a phone book. If you look up "John Smith," it might say, "See 'Jonathan Smith Construction'." You then have to do a second lookup for 'Jonathan Smith Construction' to get the actual phone number.
* **Example:**
    * **Alias:** `ftp.example.com`
    * **CNAME Record:** Points to `server1.example.com`
    * *(You would then look up the A record for `server1.example.com` to get the final IP address.)*

---

#### MX Record (Mail Exchanger)
* **Purpose:** Specifies which server is responsible for receiving email on behalf of a domain. It points to the hostname of a mail server.
* **Analogy:** This tells the post office which specific building or P.O. box receives all the mail for a large company.
* **Example:**
    * **Domain:** `gmail.com`
    * **MX Record:** Points to `alt1.aspmx.l.google.com`

---

#### NS Record (Name Server)
* **Purpose:** An NS record declares which DNS servers are the **authoritative** (official) sources of information for a specific domain.
* **Analogy:** This is a reference that says, "For the official list of all phone numbers for the 'Smith' family, you must consult Mr. or Mrs. Smith directly."
* **Example:**
    * **Domain:** `cisco.com`
    .
    * **NS Record:** Points to `ns1.cisco.com`

