# How I built and checked the lab

[Project overview](../README.md) · [Full screenshot archive](../evidence/archive.md)

I built this in stages so I could check one part before relying on it for the next. These are my build notes, not a script that automatically recreates everything. The original Windows directory server and workstation already existed when the screenshot sequence began.

## 1. Connect the lab computers

I used Hyper-V, which lets one physical computer run several virtual computers. I started the existing machines and checked their virtual switches, the software connections between them. I compared their network addresses and tested whether they could answer one another.

My physical computer had a fallback address on its lab connection. I assigned an address in the lab's network range, tested again, and received four replies from the Windows server.

**My check:** the machines were running and the corrected connection answered requests. [Screenshots 001–013](../evidence/chapters/01-network-foundation.md).

## 2. Build the Linux monitoring computer

After the early Windows build, I created an Ubuntu machine with two virtual processors, 4 GB of starting memory, and a 60 GB virtual disk. I checked the installer, installed the operating system, and updated its software.

I set up SSH so I could manage Ubuntu from a Windows command window. I used two network connections: one with an automatically assigned address for internet access, and one with a fixed address for the lab. I edited the lab settings through Netplan, Ubuntu's network configuration system.

**My check:** I could log in remotely, see the fixed lab address, and successfully request a page from ubuntu.com. [Screenshots 027–053](../evidence/chapters/03-ubuntu-build.md).

## 3. Set up monitoring and its database

I installed Zabbix 7.4.16 from its official software source. I also set up MariaDB, the database where Zabbix stores its information. I created the database, gave the application account permission to use it, and tested that account's login.

I loaded Zabbix's required database structure and counted 207 tables afterward. I then checked both that Zabbix was running and that it was set to start automatically. Those are different checks: a program can be running now without being set to return after a reboot.

**My check:** the database had its structure, the application could access it, and the monitoring program was running with automatic startup enabled. [Screenshots 054–064](../evidence/chapters/04-zabbix-stack.md).

## 4. Repair the monitoring website

The dashboard depends on Apache, which serves the website, and PHP-FPM, which processes the code that builds its pages. At first the browser could not find the page. Then an Apache setting failed its check. Later the browser displayed raw program code instead of the dashboard.

I checked where the website address pointed, corrected the web server configuration, and checked its connection to PHP-FPM. Once the setup page loaded, I connected it to the database, chose the New York display time zone, and completed installation.

**My check:** the website opened and the dashboard showed Zabbix running. [Screenshots 065–082](../evidence/chapters/04-zabbix-stack.md).

## 5. Collect Windows health information

I installed Zabbix Agent 2 on Windows. This background program answers requests from the monitoring server. I checked that it was running automatically and accepting connections on port 10050, its numbered network entry point.

I added Windows to Zabbix and selected a template, a ready-made collection of Windows checks. I then reviewed actual readings for the machine and its services. I also captured a deliberate test where I stopped the agent.

**My check:** Zabbix could contact Windows and collect information, including the DNS services' running states. [Screenshots 083–104](../evidence/chapters/05-windows-monitoring.md).

## 6. Get the clocks synchronized

Zabbix warned that the computers' clocks disagreed. I checked Chrony, Ubuntu's timekeeping program, and found its clock was about 221.5 seconds slow. I corrected it and checked again until Ubuntu reported synchronization.

I then allowed Windows to request time from Chrony. Windows kept reporting its previous time source after an initial settings change, so I checked what it actually used. The final result showed the Linux server as the selected source. The screenshots do not show every intervening Windows change.

**My check:** both systems reported synchronization, and Windows identified the intended time server. [Screenshots 105–123](../evidence/chapters/06-time-synchronization.md).

## 7. Send alerts and investigate a real recorded interruption

I tested Gmail delivery, selected who should receive messages, and added a rule for losing contact with the Windows agent. I configured one email for the problem and another for recovery.

For the October 30 interruption, I compared these alerts with Windows' startup and service records. I also separated an older October 28 shutdown from the newer event.

**My check:** the email test succeeded, both notification records said Sent, and the agent was shown running after the interruption. [Email results](alerting.md) · [Investigation](incidents/2026-10-30-monitoring-interruption.md).
