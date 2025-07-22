#ipservices #tcp #udp #examday #ports 

![[IMG_9721.jpeg]]

![[IMG_9722.jpeg]]

![[IMG_9723.jpeg]]

### Understanding the Transport Layer (Layer 4)

**CCNA Exam Objective:** 2.4 (Compare and contrast TCP and UDP) & 2.5 (Explain the role of port numbers)

The **Transport Layer (Layer 4)** of the OSI model is the crucial link between the network itself and the applications running on end devices. Its primary job is to manage the communication sessions between hosts and to provide a method for identifying which application should receive the data, a process known as **session multiplexing**.

***

### Core Functions of Layer 4

As shown in your slides, the Transport Layer has several key responsibilities:

* **Session Multiplexing:** Layer 4 uses **port numbers** to manage multiple conversations over the network at the same time from a single device. As your first image illustrates, PC1 can simultaneously browse a web page on SRV1 (destination port 80), check another web page (destination port 80), and use FTP on SRV2 (destination port 21), all at once. The source ports (50000, 55000, 60000) are randomly chosen by PC1 to keep track of each individual session.
* **Service Delivery:** It provides services like **reliable data transfer**, **error recovery**, and **flow control**. However, not all Layer 4 protocols offer these services.
* **Layer 4 Addressing:** It uses port numbers as its addressing scheme to identify specific applications. It's important to remember these are **logical ports**, not the physical interfaces on a router or switch.

***

### TCP vs. UDP: The Two Main Protocols

Layer 4 has two primary protocols, TCP and UDP, which offer different levels of service.

| Feature | TCP (Transmission Control Protocol) | UDP (User Datagram Protocol) |
| :--- | :--- | :--- |
| **Reliability** | **Reliable** - Guarantees data delivery. | **Unreliable** - "Best effort" delivery; no guarantee. |
| **Connection** | **Connection-Oriented** - Establishes a formal session (the "three-way handshake") before sending data. | **Connectionless** - No session is established; data is just sent. |
| **Sequencing** | **Yes** - Data segments are numbered and reassembled in the correct order. | **No** - No sequencing. |
| **Error Recovery**| **Yes** - Uses acknowledgements (ACKs) to confirm receipt. If an ACK isn't received, the data is re-sent. | **No** - No acknowledgements or retransmissions. |
| **Flow Control** | **Yes** - Uses a "sliding window" mechanism to ensure the sender doesn't overwhelm the receiver. | **No** - No flow control. |
| **Header Size** | Larger (20 bytes) | Smaller (8 bytes) |
| **Use Case** | Web Browse (HTTP/S), email (SMTP), file transfers (FTP). **Any application where data integrity is critical.** | DNS, DHCP, voice/video streaming (VoIP), online gaming. **Any application where speed is more important than perfect reliability.** |

---

### Understanding Session Multiplexing

Your diagrams perfectly illustrate this concept. Let's break down one flow from your second image:

`(TCP) Src: 50000 Dst: 80`

* **`Dst: 80` (Destination Port):** This is the **well-known port** for HTTP. When SRV1 receives this packet, it knows the data is intended for its web server application. This identifies the Application Layer protocol.
* **`Src: 50000` (Source Port):** This is a **randomly selected, high-numbered port** that PC1 chose for this specific session. When SRV1 replies, it will send the data back to PC1 with a destination port of 50000. This is how PC1 knows which of its open browser tabs the returning data belongs to.

This use of source and destination ports allows a single IP address (PC1) to have multiple, independent conversations with one or more servers simultaneously.

