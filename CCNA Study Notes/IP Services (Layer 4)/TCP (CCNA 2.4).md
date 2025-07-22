#ipservices #tcp

![[IMG_9724.jpeg]]

![[IMG_9725.jpeg]]

### TCP In-Depth: Reliability & Control

**CCNA Exam Objective:** 2.4 (Compare and contrast TCP and UDP)

**Transmission Control Protocol (TCP)** is the workhorse of the internet, providing a **reliable, connection-oriented** data delivery service. It ensures that every byte of data sent from one host to another arrives intact and in the correct order, making it the foundation for applications where data integrity is paramount, such as web Browse, email, and file transfers.

***
![[IMG_9726.jpeg]]
### The TCP Three-Way Handshake 🤝

Before any data is sent, TCP establishes a formal connection using a three-step process called the **three-way handshake**. This ensures both devices are ready and able to communicate. The process uses special bits in the TCP header called **flags**.

1.  **SYN (Synchronize):** The client sends a TCP segment to the server with the **SYN** flag set. This is like saying, "I want to start a conversation."
2.  **SYN-ACK (Synchronize-Acknowledge):** The server receives the SYN and, if it's able to communicate, sends back a TCP segment with both the **SYN** and **ACK** (Acknowledge) flags set. This means, "I received your request and I also want to start a conversation."
3.  **ACK (Acknowledge):** The client receives the SYN-ACK and sends a final TCP segment with the **ACK** flag set. This confirms that the connection is established. "Great, connection open!"

At this point, the connection is open, and data transfer can begin.

***

### Core Features of TCP

TCP's reliability comes from several key mechanisms that are all managed via fields in the TCP header.

| Feature | How It Works |
| :--- | :--- |
| **Reliable Delivery** | Uses **sequence numbers** and **acknowledgements (ACKs)**. The sender numbers each segment, and the receiver sends ACKs to confirm which segments it has received. If the sender doesn't get an ACK for a segment, it re-transmits it. |
| **Ordered Data Reconstruction**| The sequence numbers allow the receiving host to reassemble the data segments into their original, correct order, even if they arrive out of order due to network conditions. |
| **Flow Control** | Uses a **window size** field. The receiver tells the sender how much data it can accept before it needs an acknowledgement. This prevents the sender from overwhelming the receiver, ensuring efficient data transfer without dropping segments. |
| **Connection-Oriented** | Establishes and terminates connections gracefully using the three-way handshake and a four-way termination process. |

---
![[IMG_9728.jpeg]]
### TCP Session Termination (Four-Way Handshake)

When the conversation is over, the connection is closed gracefully.

1.  **FIN (Finish):** The client that wants to close the session sends a TCP segment with the **FIN** flag set.
2.  **ACK:** The server acknowledges the FIN request.
3.  **FIN:** The server, when it's ready, sends its own **FIN** segment.
4.  **ACK:** The client acknowledges the server's FIN, and the connection is officially closed.

***

### The TCP Header

The TCP header contains all the fields needed to manage the features above.

* **Source & Destination Ports:** Identifies the sending and receiving applications.
* **Sequence Number:** Tracks the order of data segments.
* **Acknowledgement Number:** Confirms receipt of data.
* **Header Length:** Specifies the size of the TCP header.
* **Flags (Control Bits):** Six bits that control the state of the connection (e.g., SYN, ACK, FIN, RST).
* **Window Size:** Used for flow control.
* **Checksum:** Used to check for errors in the TCP header and data.

---

### CLI Verification

While you don't configure TCP itself, you can view active TCP connections on a device.

* **On a Cisco Router/Switch:**
    ```cisco
    show tcp brief
    show tcp tcb
    ```
* **On Windows Command Prompt:**
    ```powershell
    netstat -n
    ```
* **On Linux/macOS Terminal:**
    ```bash
    netstat -nt
    ```

![[IMG_9729.jpeg]]

### TCP Sequencing: A Detailed Look

**CCNA Exam Objective:** 2.4 (Compare and contrast TCP and UDP)

This slide demonstrates the core of TCP's reliability: its **sequencing and acknowledgment** mechanism. Think of it like a formal, registered mail conversation where every single message and receipt is numbered for perfect tracking.

***

### The Analogy: Registered Mail ✉️

* **Sequence Number (Seq):** This is the serial number on the letter you are sending.
* **Acknowledgment Number (Ack):** This is the serial number of the *next* letter you expect to receive, which you write on your return receipt.

***

### Deconstructing the Handshake & Data Flow

Here is a step-by-step breakdown of the conversation shown in the diagram.

#### Step 1: PC1 Initiates (SYN)
`PC1 -> PC2 | Seq: 10`
* **What's happening:** PC1 wants to start a conversation. It sends its first message (a SYN packet) and puts a random starting serial number on it: **10**.

---

#### Step 2: PC2 Responds (SYN-ACK)
`PC2 -> PC1 | Seq: 50, Ack: 11`
* **What's happening:** PC2 agrees to talk.
* **`Seq: 50`**: PC2 starts its own side of the conversation with its own random serial number: **50**.
* **`Ack: 11`**: This is the crucial part. PC2 sends a receipt confirming it received PC1's message #10. By saying **11**, it's using **forward acknowledgment**. It's not just saying "I got 10," it's saying "I got everything up to 10, and I am now expecting message #11 from you."

---

#### Step 3: PC1 Completes the Connection (ACK)
`PC1 -> PC2 | Seq: 11, Ack: 51`
* **What's happening:** PC1 receives PC2's confirmation and sends its own final receipt to establish the connection.
* **`Seq: 11`**: PC1 continues its sequence, now sending message #11.
* **`Ack: 51`**: PC1 confirms it received PC2's message #50 and is now expecting message #51.
* **The connection is now `ESTABLISHED`.**

***

### Data Transfer Phase

With the connection open, the same mechanism ensures reliable data flow.

---

#### Step 4: PC1 Sends Data
`PC1 -> PC2 | Seq: 11, Ack: 51`
*(The slide shows this as the first data packet after the handshake)*
* **What's happening:** PC1 sends its first chunk of actual data. It's still using `Seq: 11` and acknowledging that it's still waiting for `Seq: 51` from PC2. Let's assume this packet contains **1 byte** of data.

---

#### Step 5: PC2 Acknowledges Data
`PC2 -> PC1 | Seq: 51, Ack: 12`
* **What's happening:** PC2 received the data.
* **`Seq: 51`**: PC2 sends its own data (or just an empty ACK packet) using its next serial number.
* **`Ack: 12`**: PC2 confirms it received PC1's packet. Since the packet started at `Seq: 11` and contained 1 byte, the next byte PC2 expects is **12**.

This process continues, with each side carefully tracking the sequence number it's sending and the acknowledgment number it has received, ensuring no data is lost and everything can be reassembled in the correct order.
