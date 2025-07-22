#ntp #CLI 

![[IMG_9821.jpeg]]

![[IMG_9822.jpeg]]

### A Guide to Router Clocks: Software vs. Hardware

**CCNA Exam Objective:** 4.7 (Configure and verify network time protocol (NTP))

A Cisco router maintains two separate clocks to ensure accurate timekeeping. The **software clock** is the primary, high-precision clock used by the operating system for all time-sensitive functions like logging and authentication. The **hardware clock** is a battery-backed physical component that simply maintains the date and time when the router is powered off.

***

### Clock Comparison

| Clock Type | Role & Key Function | How It's Set |
| :--- | :--- | :--- |
| **Software Clock**| The main system clock used by Cisco IOS. It is the **authoritative** time for all router operations. | Synchronized by an **NTP server** or set manually with the `clock set` command. |
| **Hardware Clock**| A battery-powered clock on the motherboard. Its only job is to **maintain time during a power outage** so it can update the software clock on reboot. | Updated from the software clock using a synchronization command. |

---

### The Relationship Between the Clocks ⚙️

The software clock is the master timekeeper. When a router is running, it relies exclusively on the software clock. The goal is to keep the software clock as accurate as possible, which is why we use **NTP** to synchronize it with an external source.

The hardware clock is just a backup. It is not as precise as the software clock and can drift over time. Therefore, it's a best practice to periodically update the hardware clock from the more accurate, NTP-synchronized software clock.

* **On Boot-up:** The software clock is inaccurate. The router copies the time from the hardware clock into the software clock to get a starting point.
* **During Operation:** The software clock is synchronized and updated by NTP.
* **Periodically:** The administrator configures the router to copy the accurate time from the software clock *back* to the hardware clock, ensuring it's correct for the next reboot.

---

### Essential Clock CLI Commands
![[IMG_9825.jpeg]]

| Command | Mode | Purpose |
| :--- | :--- | :--- |
| `show clock [detail]`| EXEC | **Displays the current software clock time.** The `detail` keyword shows the time source. |
| `show calendar` | EXEC | **Displays the current hardware clock time.** |
| `clock set [hh:mm:ss] [day] [month] [year]`| EXEC | Manually sets the software clock. |
| `ntp update-calendar` | Global Config | **Configures the router to automatically update the hardware clock** from the software clock whenever NTP makes a significant update. This is the recommended best practice. |
| `clock update-calendar` | EXEC | Manually forces a one-time update of the hardware clock from the current software clock. |
