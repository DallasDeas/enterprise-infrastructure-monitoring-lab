# From a connected lab to a useful alert

[Project overview](../README.md) · [All 142 annotated screenshots](archive.md)

These are the parts of the project I would walk through in an interview. The complete archive includes the installation screens and repeated checks; this page follows the decisions and results that mattered most.

## First, I had to make the network checks agree

The workstation could ping the directory server, but the Hyper-V host could not. I found an APIPA address on the host's lab adapter, assigned it an address in the lab subnet, and checked again. The successful replies below came from the host after that change.

![Four successful host-to-server replies](screenshots/archive/screenshot_012.png)

That gave me a working foundation for the Ubuntu monitoring VM. I used one interface for internet access and a second static interface for the lab. [Network chapter](chapters/01-network-foundation.md) · [Ubuntu chapter](chapters/03-ubuntu-build.md).

## The database worked before the web page did

I created the Zabbix database, tested the application account, and imported the schema. The count of 207 tables gave me a useful checkpoint before I dealt with the frontend.

![Imported schema with 207 tables](screenshots/archive/screenshot_061.png)

Apache being active did not finish the job. I first got a 404, then a configuration error, then PHP source in the browser. I checked the alias, proxy modules, and PHP-FPM handler until the page reached the installer and then the dashboard. [Full web-stack troubleshooting](chapters/04-zabbix-stack.md).

![Working Zabbix dashboard](screenshots/archive/screenshot_082.png)

## I checked collected values, not just the host list

On Windows, I verified Agent 2 was Running / Automatic and listening on port 10050. In Zabbix, I added the Windows template and agent interface, then checked Latest data. The DNS service values below show actual collected service states.

![DNS Server and DNS Client values](screenshots/archive/screenshot_097.png)

I also reviewed the service-state and availability triggers and captured a deliberate agent-stop test. Those checks helped me distinguish a service value from loss of the monitoring connection. [Windows monitoring chapter](chapters/05-windows-monitoring.md).

## A time warning turned into its own troubleshooting exercise

Ubuntu's clock was about 221.5 seconds slow. I checked Chrony's sources, stepped the clock, and verified synchronization later. Then I found Chrony was denying the Windows client, corrected its access rule, and checked the time source Windows actually selected. A successful peer-configuration command had not been enough.

![Windows synchronized to the Linux NTP source](screenshots/archive/screenshot_121.png)

The final status showed Leap Indicator 0 and Stratum 4. The address is covered, and the exact intervening Windows provider change was not captured. [Time-sync chapter](chapters/06-time-synchronization.md).

## The alert needed a recovery message too

I tested Gmail media delivery, limited the action to the directory server's agent-unavailable trigger, and added recovery notification. The October 30 Action log showed both messages marked Sent.

![Problem and recovery Gmail sends](screenshots/archive/screenshot_139.png)

The problem lasted five minutes in Zabbix. I did not measure detection latency or mailbox delivery time. [Alert setup and results](../docs/alerting.md).

## I checked the Windows evidence before writing the incident story

I compared the last boot time, the agent's Running event, and the Zabbix problem/recovery times. The sequence supports a startup-associated monitoring interruption under the documented time-zone assumption. It does not prove an agent crash or a particular manual fix.

![Agent Running event in the queried window](screenshots/archive/screenshot_142.png)

The unexpected-shutdown message was the detail I had to read carefully: it was logged on October 30 but described October 28. I kept that finding in its own note. [October 30 investigation](../docs/incidents/2026-10-30-monitoring-interruption.md) · [October 28 finding](../docs/incidents/2026-10-28-unclean-shutdown.md).

## Browse the full record

The [full archive](archive.md) links every PNG with a first-person explanation. The seven chapters include the early Windows VM branch, Ubuntu installation, web-stack failures, monitoring checks, time-sync work, and alert investigation. Private fields are covered; the remaining screenshot content is original evidence.
