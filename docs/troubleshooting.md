# What went wrong and how I checked it

[Project overview](../README.md) · [Full screenshot archive](../evidence/archive.md)

I kept the failed attempts because they show how I worked through the problems. My approach was to check one part, make a change where the result pointed, and then test again. A command finishing without an error was not enough by itself.

| What I noticed | What I did next | What I learned |
|---|---|---|
| I could not list my virtual machines | I reopened PowerShell with administrator permissions | I could then see the machines (001–002). |
| The workstation could reach the server, but my physical computer could not | I checked and corrected the physical computer's lab address | The server answered all four test requests afterward (007–012). |
| A virtual network connection was on, but requests still failed | I tested from different computers and compared addresses, routes, and name lookups | Some paths worked while another failed; I did not prove one specific cause for all failures (021–026). |
| The Ubuntu preparation checks showed the wrong installation media and a command error | I checked the installation files and startup settings separately | I could confirm the Ubuntu installer and appropriate startup setting were present (028–030). |
| My remote-login command failed | I corrected how I wrote the SSH command | I could log into Ubuntu from Windows (045–046). |
| Ubuntu's second connection had no fixed lab address | I added its network settings and checked the results | It had a fixed lab address while internet traffic still used the other connection (048–052). |
| Zabbix was running but not set to start automatically | I checked its startup setting separately | A later check showed automatic startup enabled (062–064). |
| The browser could not find the Zabbix page | I checked how Apache connected the web address to the files on disk | This led to the next website configuration checks (065–066). |
| Apache rejected one of its settings | I investigated the web-server configuration and ran its check again | The settings check passed (067–070). |
| The browser showed program code instead of a webpage | I checked PHP-FPM and how Apache sent page requests to it | I eventually received webpage content and reached the setup screen (069–074). |
| The Windows agent was installed, but I still needed proof of monitoring | I checked the background program, its connection, and the collected readings | Zabbix could reach Windows and display its data (091–097). |
| Zabbix warned that the clocks disagreed | I checked each computer's time and time source | Ubuntu was about three minutes and 42 seconds slow (105–110). |
| Ubuntu's time service was on, but its clock was not synchronized | I checked the reference source, corrected the clock, and checked again | Ubuntu later reported synchronization (111–113). |
| Windows was not allowed to request time from Ubuntu | I checked Chrony's access rules and changed the relevant permission | The access test then allowed Windows (114–116). |
| Windows accepted a new time setting but still used its old source | I checked the source it actually selected | The final record showed Linux as the source, although not every intervening change was captured (118–121). |
| A command to start the Windows agent failed | I checked permissions and the current service state in an administrator window | The next capture already showed it running before the start command, so that command was not a proven repair (135–136). |

## What I would check during another interruption

1. I would record the time and time zone, then check that both virtual machines are running.
2. I would check the Windows agent locally. If it is running, I would still test whether the monitoring server can reach it.
3. I would look at the warning rule in Zabbix and when it last collected fresh information.
4. I would compare Windows' startup and service records with the alert times, using the same time zone.
5. I would check the email send record, then the inbox. A message marked Sent and a message received are separate checks.

For October 30, I could document the interruption and recovery, but I did not have enough evidence to prove the original cause or a particular command that fixed it. [Full investigation](incidents/2026-10-30-monitoring-interruption.md).
