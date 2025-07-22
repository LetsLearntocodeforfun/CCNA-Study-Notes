#ntp #CLI 

![[IMG_9823.jpeg]]

![[IMG_9824.jpeg]]

You can configure time zones and daylight saving time on a Cisco router using the clock timezone and clock summer-time global configuration commands.
### A Guide to Time Zones & Daylight Saving Time

**CCNA Exam Objective:** 4.7 (Configure and verify network time protocol (NTP))

For log messages and time-sensitive operations to be meaningful, a router's clock must be configured for its correct local time zone and be able to automatically adjust for daylight saving time. This ensures that the time displayed to a network administrator is accurate and easy to understand.

***

### Time Configuration CLI Cheat Sheet ⚙️

| Command Type | Command | Purpose |
| :--- | :--- | :--- |
| **Time Zone** | `clock timezone [zone-name] [hours-offset]` | Sets the local time zone by specifying a name and its offset from Coordinated Universal Time (UTC). |
| **DST (Recurring)**| `clock summer-time [zone-name] recurring [params]` | **The best practice.** Configures DST to occur automatically on a recurring schedule (e.g., the second Sunday in March). |
| **DST (Specific Dates)**|`clock summer-time [zone-name] date [params]`| Configures DST to occur on specific dates, which requires manual updates each year. |
| **Verification**| `show clock [detail]`| Displays the current time and confirms if it is standard or daylight saving time. |

***

### Command Breakdown & Configuration

#### `clock timezone [zone-name] [hours-offset]`

This command sets the standard time zone for the device.

* **`[zone-name]`**: A 3-5 character name for your time zone (e.g., `CST`, `EST`, `PST`). This is the name that will be displayed in log messages.
* **`[hours-offset]`**: The number of hours your time zone is behind (negative) or ahead (positive) of UTC. For example, US Central Standard Time (CST) is 6 hours behind UTC, so the offset is `-6`.

---

#### `clock summer-time [zone-name] recurring`

This is the recommended method for configuring Daylight Saving Time (DST). It defines the rules for when the time change should automatically occur each year.

* **`[zone-name]`**: A 3-5 character name for your daylight saving time zone (e.g., `CDT`).
* **`recurring`**: This keyword tells the router to use a repeating schedule. The parameters that follow define the start and end of DST. In North America, this is typically the second Sunday in March to the first Sunday in November.

---

### Practical Example: Configuring for US Central Time

Let's configure a router for the US Central Time zone (CST/CDT).

* **Standard Time:** CST is UTC -6.
* **Daylight Saving Time:** CDT is UTC -5.
* **DST Schedule:** Starts on the second Sunday of March and ends on the first Sunday of November.

**Configuration:**
```cisco
! Set the standard time zone to CST, which is 6 hours behind UTC
clock timezone CST -6

! Configure the recurring daylight saving time schedule for CDT
clock summer-time CDT recurring 2 Sun Mar 2:00 1 Sun Nov 2:00

Verification:
After applying the configuration, run show clock detail. The output will now display the correct local time and indicate which time zone is currently active.
R1# show clock detail
06:23:35.123 CDT Mon Jul 21 2025
Time source is NTP

The CDT in the output confirms that the router understands it is currently in the daylight saving time period.

