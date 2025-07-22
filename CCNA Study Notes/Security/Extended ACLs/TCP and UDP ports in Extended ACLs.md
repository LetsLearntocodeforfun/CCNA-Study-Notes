#tcp #udp #ACLs #security #ports #examday 

![[IMG_9804 1.jpeg]]

Of course. Here is the complete guide formatted in a single block for easy export to Obsidian.
### A Guide to Matching Port Numbers in ACLs

**CCNA Exam Objective:** 4.5 (Configure and verify extended access control lists)

When you create an **extended ACL**, you can specify Layer 4 port numbers to filter traffic with incredible precision. This is done by adding an **operator** and a port number at the end of a `permit` or `deny` statement for TCP or UDP traffic.

***

### Port Number Operators

This chart breaks down the available operators and what they do.

| Operator | Meaning | Example | What It Matches |
| :--- | :--- | :--- | :--- |
| **`eq`** | **Equal to** | `eq 80` | Only port 80 (HTTP) |
| **`gt`** | **Greater than** | `gt 1023` | All ports from 1024 and up (all registered and dynamic ports) |
| **`lt`** | **Less than** | `lt 22` | All ports from 0 to 21 |
| **`neq`** | **Not equal to** | `neq 22` | Every single port except for port 22 (SSH) |
| **`range`**| **Inclusive range** | `range 20 21` | Port 20 (FTP data) and Port 21 (FTP control) |
### Practical Configuration Examples

Here are several scenarios demonstrating how these operators are used within a single configuration session.

```cisco
! Enter global configuration mode
config t

! --- Example 1: Permitting ONLY HTTPS Traffic ---
! Create a named ACL for our first policy
ip access-list extended ALLOW_WEB_ONLY

! Permit TCP traffic from any source to the server, only if the destination port equals 443.
permit tcp any host 10.1.1.10 eq 443
exit


! --- Example 2: Blocking a Range of Ports ---
! Create a second named ACL
ip access-list extended BLOCK_LEGACY_PORTS

! Deny any TCP traffic destined for ports 20 through 23 (FTP, SSH, Telnet)
deny tcp any any range 20 23

! Remember to permit other traffic if needed, otherwise it will be denied.
permit ip any any
exit


! --- Example 3: Enforcing HTTPS with 'neq' ---
! Create a third named ACL
ip access-list extended REQUIRE_HTTPS

! This rule denies any TCP traffic that is NOT destined for port 443.
! This is a creative way to enforce a "HTTPS only" policy.
deny tcp any any neq 443

! Permit all other IP traffic (like ICMP or UDP) if needed.
permit ip any any
exit
