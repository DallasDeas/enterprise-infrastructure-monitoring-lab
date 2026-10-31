# How I built and checked it

[Project overview](../README.md) · [Full screenshot archive](../evidence/archive.md)

This is a record of my build, including the checks that failed. It is not an unattended installer or a complete export of every setting. The directory server and workstation already existed when this screenshot sequence began; the archive does not include their original domain-promotion or domain-join steps.

## 1. Establish the network foundation

I opened an elevated PowerShell session, checked VM state and switch membership, and started the existing lab machines. I compared the directory server and workstation addressing before testing connectivity. The host's lab adapter had an APIPA address, so I assigned it a static address in the lab's /24 subnet and confirmed the server then answered four probes.

**Checkpoint:** VM Running state, expected switch membership, correct adapter address, and a successful test from each relevant source. [Screenshots 001–013](../evidence/chapters/01-network-foundation.md).

## 2. Build the Ubuntu guest

After an early Windows VM branch, I prepared a Generation 2 Ubuntu VM with two virtual processors, 4 GB startup memory, and a 60 GB dynamic disk. I checked the mounted ISO and Secure Boot template, installed Ubuntu 26.04.1 LTS using LVM, and completed package updates. I confirmed OpenSSH was listening and connected from Windows.

I used two adapters: DHCP on the Default Switch for internet access and a static address on the internal lab switch. I wrote a separate Netplan file for the lab interface, set its permissions to 600, generated the configuration, and checked the resulting addresses and routes. A later login showed the lab address remained in place. An HTTPS header request to ubuntu.com returned HTTP/2 200.

**Checkpoint:** SSH access, static lab address, internet default route, and successful outbound request. [Screenshots 027–053](../evidence/chapters/03-ubuntu-build.md).

## 3. Prepare the database and Zabbix server

I used the official Zabbix 7.4 repository and checked the 7.4.16 package candidate. I verified MariaDB was active and enabled, created the Zabbix database with utf8mb4/utf8mb4_bin, granted the local application account database access, and tested its login. After installing the Zabbix components, I imported server.sql.gz and counted 207 tables.

I checked the Zabbix service, its startup log, and whether it was enabled. The first service capture showed active but disabled; a later check showed enabled. These are separate checks because a running service can still fail to start on the next boot.

**Checkpoint:** database access, populated schema, active server, and enabled startup. [Screenshots 054–064](../evidence/chapters/04-zabbix-stack.md).

## 4. Get the frontend working

I worked through a 404 response, an Apache ProxyTimeout configuration error, and PHP source displaying in the browser. I inspected the Zabbix alias, enabled the required proxy/FPM configuration, checked the PHP handler, and tested locally with curl. The frontend then reached the setup wizard.

I reviewed the PHP prerequisites, configured the local database connection, selected America/New_York for the frontend display, and completed installation. The dashboard showed the Zabbix server running and monitoring itself.

**Checkpoint:** Syntax OK, PHP-FPM active, HTML response, completed wizard, and working dashboard. [Screenshots 065–082](../evidence/chapters/04-zabbix-stack.md).

## 5. Add Windows monitoring

I obtained Agent 2 7.4.16, enabled Hyper-V's Guest Service Interface during transfer preparation, and confirmed the MSI was inside the Windows Server guest. I installed the agent, configured the monitoring server and listen port 10050, and verified Running / Automatic plus the listening socket.

In Zabbix I added the Windows host, its agent interface, the Windows servers group, and the Windows by Zabbix agent template. I checked green availability and actual collected items, including DNS Server and DNS Client service states. I also captured a deliberate agent stop and the frontend's unavailable/available states.

**Checkpoint:** service, listener, available agent interface, and recent data. [Screenshots 083–104](../evidence/chapters/05-windows-monitoring.md).

## 6. Correct time synchronization

I compared Windows and Linux time status after Zabbix raised a clock warning. Chrony reported a roughly 221.5-second slow clock. I checked its source, stepped the clock, and verified synchronization afterward. I then corrected Chrony's access rules for the Windows NTP client and checked UDP 123.

Windows initially continued using the VM integration provider after a successful manual-peer configuration command. The final capture showed the Linux server as its source, Stratum 4, and Leap Indicator 0. The exact intervening provider change is not in the screenshots, so this record does not prescribe an unverified fix.

**Checkpoint:** actual selected source and synchronized state on both systems. [Screenshots 105–123](../evidence/chapters/06-time-synchronization.md).

## 7. Add notifications and investigate recovery

I tested Gmail media delivery, enabled the recipient, and created a host-and-trigger-specific action with problem and recovery operations. The later Action log recorded both sends. I compared the October 30 monitoring event with Windows boot and service records and documented the October 28 shutdown message separately.

**Checkpoint:** successful media test, enabled action, problem send, recovery send, and service-event evidence. [Alerting demonstration](alerting.md) and [incident report](incidents/2026-10-30-monitoring-interruption.md).
