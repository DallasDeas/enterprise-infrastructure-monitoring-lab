# Working through the time-sync warning

[Walkthrough](../README.md) · [Full archive](../archive.md)

I investigated a clock-skew warning because inconsistent timestamps weaken event correlation. I compared Windows and Ubuntu time status, checked Chrony's reference sources and access rules, and verified the source Windows actually selected. I documented the final synchronized state while keeping the uncaptured intermediate Windows changes explicit.

## 105 — Finding a time warning

The October 28 problem view showed clock skew, a restart warning, and a resolved two-minute agent-unavailable event. This is an earlier event, separate from the five-minute October 30 interruption.

![Screenshot 105: Finding a time warning](../screenshots/archive/screenshot_105.png)

## 106 — Checking Windows time

I compared the Windows timestamp and time-service status. Windows showed a UTC-07:00 offset, the VM integration time provider, and a not-synchronized status.

![Screenshot 106: Checking Windows time](../screenshots/archive/screenshot_106.png)

## 107 — Checking Linux time

I checked UTC time and timedatectl on Ubuntu. NTP was active, but the system clock was not yet synchronized.

![Screenshot 107: Checking Linux time](../screenshots/archive/screenshot_107.png)

## 108 — Checking domain time-service context

I queried the FSMO role holders and confirmed that the directory server held the PDC role along with the other domain roles. This established context for the Windows time investigation. The nearby UTC command also required a syntax correction, which I captured separately.

![Screenshot 108: Checking domain time-service context](../screenshots/archive/screenshot_108.png)

## 109 — Capturing Windows UTC time

I corrected the UTC command and recorded a Windows timestamp with a Z suffix for comparison across systems.

![Screenshot 109: Capturing Windows UTC time](../screenshots/archive/screenshot_109.png)

## 110 — Measuring clock skew

Chrony tracking showed the Ubuntu system clock about 221.5 seconds behind its reference. I treated this measured offset as a finding behind the clock-skew warning, rather than relying only on whether the NTP service was active.

![Screenshot 110: Measuring clock skew](../screenshots/archive/screenshot_110.png)

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

## 121 — Validating the selected Windows time source

The final Windows status showed Leap Indicator 0, Stratum 4, and the Linux server as the selected time source. This supported a synchronized state using the intended source. The intervening provider change was not captured, so I did not attribute the result to an unverified command.

![Screenshot 121: Validating the selected Windows time source](../screenshots/archive/screenshot_121.png)

## 122 — Checking the current problem view

The filtered Zabbix view showed no current problems. I followed that with a history check rather than treating an empty filtered screen as proof of every host's health.

![Screenshot 122: Checking the current problem view](../screenshots/archive/screenshot_122.png)

## 123 — Checking resolved history

The history view showed the earlier clock, restart, and agent-availability problems resolved. The clock warning lasted 17m 3s; that is a recorded event duration, not a promised response time.

![Screenshot 123: Checking resolved history](../screenshots/archive/screenshot_123.png)
