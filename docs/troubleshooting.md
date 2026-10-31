# Troubleshooting notes from the build

[Project overview](../README.md) · [Full evidence archive](../evidence/archive.md)

I kept the failed checks because they explain how I narrowed the problems down. A successful command was useful only when the next check showed the behavior I needed.

| Symptom | What I checked or changed | What the next evidence showed |
|---|---|---|
| Get-VM permission error | Used an elevated shell | VM inventory became available (001–002) |
| Workstation reached the server; host could not | Checked the host's lab adapter and corrected its APIPA addressing | Four host-to-server replies (007–012) |
| Guest adapter Up, but host path failing | Compared ping, neighbor state, routes, and DNS from different sources | Some paths worked while the monitoring-VM host path still failed; no single cause proven (021–026) |
| Wrong media and malformed command during VM preparation | Inspected DVD drives and firmware separately | Ubuntu ISO present and Secure Boot template confirmed (028–030) |
| SSH command rejected by PowerShell | Corrected the command syntax | Ubuntu remote login succeeded (045–046) |
| Second Ubuntu NIC had no lab IPv4 address | Added static Netplan configuration and checked addresses/routes | Static lab address alongside the internet default route (048–052) |
| Zabbix service active but disabled | Checked enabled state separately | Later output showed enabled (062–064) |
| /zabbix returned 404 | Inspected the Apache configuration link and alias | Reached later web-configuration checks (065–066) |
| Apache rejected ProxyTimeout | Investigated proxy configuration and reran configtest | Syntax OK (067–070) |
| Browser displayed PHP source | Checked PHP-FPM, proxy_fcgi, enabled FPM configuration, and socket handler | Local request returned HTML and browser reached setup (069–074) |
| Agent installed but collection needed verification | Checked service, listener, host interface, template, and Latest data | Running / Automatic, TCP 10050, green availability, collected values (091–097) |
| Clock-skew warning | Compared actual time status and Chrony tracking | Linux clock was about 221.5 seconds slow (105–110) |
| NTP active but Linux clock unsynchronized | Checked sources, ran makestep, and rechecked later | timedatectl eventually showed synchronized (111–113) |
| Windows NTP client denied | Used chronyc accheck; inspected UFW and Chrony access configuration | Access allowed after configuration change (114–116) |
| Windows peer command succeeded but source stayed VMIC | Queried configuration and actual source repeatedly | Final status selected Linux NTP; exact intervening change not captured (118–121) |
| Start-Service could not open service | Checked elevation and queried current state | Elevated capture already showed Running before the displayed start command (135–136) |

## The checks I would repeat during another interruption

1. Record the time with its offset, then check whether the VM is running and whether the monitoring server is reachable.
2. On Windows, check Agent 2 state and its listening port. A local Running result does not prove the remote monitoring path is healthy.
3. In Zabbix, inspect interface availability, the problem's trigger, and the most recent item timestamps.
4. Compare Windows boot and service events with the problem and recovery times using an explicit time basis.
5. Check the action log separately from the problem state. Then verify mailbox receipt separately from Sent.

For the October 30 case, I did not find enough evidence to name a proven root cause or a proven manual recovery command. That limit belongs in the incident report rather than being replaced with a cleaner story.
