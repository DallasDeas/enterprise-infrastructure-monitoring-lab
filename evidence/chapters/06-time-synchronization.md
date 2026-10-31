# Working through the time-sync warning

[Walkthrough](../README.md) · [Full archive](../archive.md)

A time-sync warning made me compare the actual clock sources on both systems. Ubuntu's NTP service was active without the clock being synchronized, and Windows kept reporting the VM integration provider after I configured a manual peer. I followed the status output until both systems showed the intended result. Some intermediate changes were not captured, and I leave those gaps visible.

## 105 — Finding a time warning

The October 28 problem view showed clock skew, a restart warning, and a resolved two-minute agent-unavailable event. This is an earlier event, separate from the five-minute October 30 interruption.

![Screenshot 105: Finding a time warning](../screenshots/archive/screenshot_105.png)

## 106 — Checking Windows time

I compared the Windows timestamp and time-service status. Windows showed a UTC-07:00 offset, the VM integration time provider, and a not-synchronized status.

![Screenshot 106: Checking Windows time](../screenshots/archive/screenshot_106.png)

## 107 — Checking Linux time

I checked UTC time and timedatectl on Ubuntu. NTP was active, but the system clock was not yet synchronized.

![Screenshot 107: Checking Linux time](../screenshots/archive/screenshot_107.png)

## 108 — Checking domain time context

After correcting a PowerShell syntax mistake in a later capture, I also queried the FSMO role holders here. The output placed the PDC role and the other FSMO roles on the directory server.

![Screenshot 108: Checking domain time context](../screenshots/archive/screenshot_108.png)

## 109 — Capturing Windows UTC time

I corrected the UTC command and recorded a Windows timestamp with a Z suffix for comparison across systems.

![Screenshot 109: Capturing Windows UTC time](../screenshots/archive/screenshot_109.png)

## 110 — Measuring the Linux clock offset

Chrony tracking showed the system clock about 221.5 seconds slow relative to its reference. That was a measurable problem behind the time warning.

![Screenshot 110: Measuring the Linux clock offset](../screenshots/archive/screenshot_110.png)

## 111 — Inspecting Chrony's sources

I checked the NTP source list and the selected external source before forcing a correction.

![Screenshot 111: Inspecting Chrony's sources](../screenshots/archive/screenshot_111.png)

## 112 — Stepping the Linux clock

I ran chronyc makestep and received 200 OK. Tracking then showed zero system-time offset, while timedatectl had not yet changed its synchronized flag in that immediate capture.

![Screenshot 112: Stepping the Linux clock](../screenshots/archive/screenshot_112.png)

## 113 — Verifying Linux synchronization

A later timedatectl check reported the system clock synchronized. I also inspected the Chrony configuration, including rtcsync and the makestep setting.

![Screenshot 113: Verifying Linux synchronization](../screenshots/archive/screenshot_113.png)

## 114 — Finding denied NTP access

Chrony's access check denied the Windows server, while UFW was inactive. I had to check Chrony's own client-access rules rather than assuming this was a host firewall block.

![Screenshot 114: Finding denied NTP access](../screenshots/archive/screenshot_114.png)

## 115 — Allowing the Windows NTP client

I edited the Chrony configuration, restarted the service, and repeated the access check. It returned Access allowed for the Windows server.

![Screenshot 115: Allowing the Windows NTP client](../screenshots/archive/screenshot_115.png)

## 116 — Checking the NTP listener

I confirmed a UDP 123 listener and checked Chrony tracking again. The displayed system-time offset was roughly half a millisecond.

![Screenshot 116: Checking the NTP listener](../screenshots/archive/screenshot_116.png)

## 117 — Testing time from Windows

I ran a five-sample w32tm stripchart against the Linux time source. The responses showed an offset of about 3.7 seconds at that stage.

![Screenshot 117: Testing time from Windows](../screenshots/archive/screenshot_117.png)

## 118 — Configuring the Windows time peer

I configured the manual NTP peer and requested a resync. The commands completed, but the status still reported the VM integration provider and no synchronization, so I kept checking.

![Screenshot 118: Configuring the Windows time peer](../screenshots/archive/screenshot_118.png)

## 119 — Inspecting time-service configuration

I reviewed the configured NTP peer and provider settings. The VM integration time provider was still enabled in this capture.

![Screenshot 119: Inspecting time-service configuration](../screenshots/archive/screenshot_119.png)

## 120 — Checking the actual source again

The source still reported the VM integration provider. This reinforced that a successful configuration command did not prove Windows had selected the intended source.

![Screenshot 120: Checking the actual source again](../screenshots/archive/screenshot_120.png)

## 121 — Confirming Windows synchronization

The later status showed Leap Indicator 0, Stratum 4, and the Linux server as the time source. The exact intervening change is not captured, so I do not claim a specific provider-disable command fixed it.

![Screenshot 121: Confirming Windows synchronization](../screenshots/archive/screenshot_121.png)

## 122 — Checking the current problem view

The filtered Zabbix view showed no current problems. I followed that with a history check rather than treating an empty filtered screen as proof of every host's health.

![Screenshot 122: Checking the current problem view](../screenshots/archive/screenshot_122.png)

## 123 — Checking resolved history

The history view showed the earlier clock, restart, and agent-availability problems resolved. The clock warning lasted 17m 3s; that is a recorded event duration, not a promised response time.

![Screenshot 123: Checking resolved history](../screenshots/archive/screenshot_123.png)
