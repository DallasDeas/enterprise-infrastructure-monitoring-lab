# Architecture and topology

## Component inventory

| Role | Components | Verification level |
|---|---|---|
| Virtualization | Hyper-V host and lab virtual networking | Hyper-V console visible in original Windows evidence; network isolation described in conversation |
| Directory server VM | Windows Server 2022, Active Directory Domain Services, DNS | Platform/roles described in conversation; agent state and Windows events directly visible |
| Monitoring VM | Ubuntu Server, Zabbix, MariaDB, Apache, Chrony | Stack described in conversation; Zabbix Action log directly visible |
| Notification destination | Gmail mailbox | Gmail media type and Sent statuses visible; receipt confirmed by owner |

## Logical paths

1. The monitored Windows VM and Zabbix have a monitoring relationship through the lab network.
2. Zabbix evaluates the agent-unavailability problem and its recovery.
3. An action uses the Gmail media type to send problem and recovery notifications.
4. Windows service and boot events provide independent investigation context.

The screenshots do not establish agent transport direction, configured ports, the virtual switch type, NAT arrangement, subnet, or the NTP hierarchy. No physical network topology is claimed. Chrony being installed does not establish that Windows uses it as its time source.

## Topology details still needed

Record OS/software versions, VM resources, switch type and attachment, sanitized interface roles, DNS dependencies, agent active/passive mode, firewall rules, and time sources. Use documentation aliases and synthetic addresses if examples are necessary. Keep real hostnames, domain names, MAC addresses, and IP addresses out of public artifacts.
