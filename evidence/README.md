# My lab, from the first connection checks to email alerts

[Project overview](../README.md) · [All 142 screenshots](archive.md)

I built this lab to practice looking after computers after they have been installed. I wanted to check their health, notice problems, and use their records to explain what happened. This page follows the main steps without requiring experience with the tools.

## 1. I got the computers talking to each other

My virtual workstation could reach the Windows server, but my physical computer could not. I found that the physical computer's lab connection had given itself an address outside the lab's network range. I assigned a matching fixed address and tested again.

![Four replies after correcting the network address](screenshots/archive/screenshot_012.png)

The four replies show that the server answered the test requests. This did not test every possible network function, but it confirmed the basic connection after my change. [Network checks](chapters/01-network-foundation.md).

## 2. I gave Zabbix somewhere to store its information

Zabbix is the monitoring program. It uses a database to store settings and collected information. I created that database, checked that the application could sign in, and loaded the structure it needed. The result showed 207 tables, the containers that organize the stored information.

![Database check showing 207 tables](screenshots/archive/screenshot_061.png)

The database was ready, but the website still failed. First I got a missing-page error. Later the browser showed program code instead of the page. I worked through the web server settings and the PHP processing setup until the dashboard opened.

![The working Zabbix dashboard](screenshots/archive/screenshot_082.png)

This taught me to check each part separately. A running database or web server does not automatically mean the whole application works. [Zabbix installation and website repairs](chapters/04-zabbix-stack.md).

## 3. I checked that Windows was sending useful information

I installed Agent 2, a small background program on Windows that responds to Zabbix's requests. I checked that it was running, added the Windows server to Zabbix, and looked at the information being collected.

![Collected readings showing the DNS services running](screenshots/archive/screenshot_097.png)

Here I checked DNS, the service that helps computers find each other by name. The readings showed both DNS services running. I also looked at processor use, memory, and storage. These readings showed more than simply seeing the server listed on a dashboard. [Windows monitoring](chapters/05-windows-monitoring.md).

## 4. I worked through a clock disagreement

Ubuntu's clock was about three minutes and 42 seconds slow. That matters when comparing records: if two clocks disagree, an event can appear to happen before or after something it did not actually precede or follow.

I used Chrony, Ubuntu's timekeeping program, to check and correct the clock. I then corrected a rule that was preventing Windows from requesting time from Ubuntu. Windows initially kept using its previous time source, so I checked its status again rather than assuming the setting had taken effect.

![Windows reporting synchronization with the Linux time server](screenshots/archive/screenshot_121.png)

The final result showed Windows synchronized and using the Linux server. I did not capture every change between the first checks and this result, so the notes do not claim one unrecorded command was the fix. [Time troubleshooting](chapters/06-time-synchronization.md).

## 5. I added both a problem email and a recovery email

I tested Gmail from Zabbix, then created a rule for when Zabbix could not contact the Windows agent. I added a second message for when the problem cleared. I wanted to know both when attention was needed and when monitoring had recovered.

![Problem and recovery emails marked Sent](screenshots/archive/screenshot_139.png)

The October 30 records show both messages marked Sent. The problem was open for five minutes. That does not tell me how long Zabbix took to notice the original failure or how quickly the messages arrived in my inbox. [Email setup and results](../docs/alerting.md).

## 6. I read the records before deciding what happened

I compared Windows' startup time, the record of the agent starting, and Zabbix's problem and recovery times. After accounting for the displayed time zones, the interruption appeared close to a Windows startup. That is a useful clue, but it does not prove the original cause.

![Windows record showing the agent entering the Running state](screenshots/archive/screenshot_142.png)

I also found a message recorded on October 30 that described an unexpected shutdown on October 28. I kept that earlier shutdown separate. I did not have evidence to say the agent crashed, that a particular start command restored it, or that the Windows server's other services were unavailable for five minutes.

[October 30 investigation](../docs/incidents/2026-10-30-monitoring-interruption.md) · [October 28 finding](../docs/incidents/2026-10-28-unclean-shutdown.md)

The [full archive](archive.md) includes every screenshot, including installation screens, mistakes, and repeated checks. Each one has a first-person explanation of what I was doing and what the result meant.
