#ntp #security 

![[IMG_9835.jpeg]]

You can configure NTP authentication to ensure a device only accepts time updates from trusted sources by defining a shared key and enabling authentication on both the client and server.
### A Guide to Configuring NTP Authentication

**CCNA Exam Objective:** 4.7 (Configure and verify network time protocol (NTP))

**NTP authentication** provides a layer of security for your time synchronization infrastructure. It uses pre-shared MD5 keys to ensure that a client router only accepts NTP updates from a server that it explicitly trusts. This prevents a malicious actor from injecting false time information into your network, which could disrupt logs, certificates, and other time-sensitive services.

***

### NTP Authentication CLI Playbook 📖
![[IMG_9836.jpeg]]

| Command | Mode | Purpose |
| :--- | :--- | :--- |
| `ntp authenticate` | Global Config | **Globally enables** the NTP authentication feature. |
| `ntp authentication-key [key#] md5 [key]`| Global Config | **Creates the key.** Defines a key number and its corresponding password (the key string). |
| `ntp trusted-key [key#]`| Global Config | **Specifies which keys are trusted.** The router will only accept updates from servers using one of these keys. |
| `ntp server [ip] key [key#]` | Global Config | On a client, this command tells the router to **use a specific key** when communicating with a specific server. |

***

### The 4-Step Configuration Process ⚙️

Configuring NTP authentication requires four steps that must be mirrored on both the client and the server device.

#### **Step 1: Enable Authentication Globally**
This command is the master switch for the feature.
`ntp authenticate`

#### **Step 2: Create the Authentication Key**
This defines the key number and the password that will be shared between the devices.
`ntp authentication-key 1 md5 MySecurePa55`

#### **Step 3: Define the Key as "Trusted"**
This tells the router which key numbers it should consider valid for incoming NTP packets.
`ntp trusted-key 1`

#### **Step 4: Associate the Key with the NTP Server**
This final step, done on the client, ties all the pieces together by telling the client which key to use for which server.
`ntp server 10.1.1.1 key 1`

---

### Practical Configuration Example

**Scenario:** We want to configure secure time synchronization between a client router and a server at `10.1.1.1`.

**Configuration on the NTP Server (R1):**
```cisco
! The server also needs to be configured to trust the key
ntp authenticate
ntp authentication-key 1 md5 MySecurePa55
ntp trusted-key 1

! Configure the server to act as a time source
ntp master 3

Configuration on the NTP Client (R2):
! The client needs all four commands
ntp authenticate
ntp authentication-key 1 md5 MySecurePa55
ntp trusted-key 1
ntp server 10.1.1.1 key 1

Now, R2 will only synchronize its clock with R1 if R1 provides the correct key (MySecurePa55), ensuring the time source is legitimate.

