# Architecture and topology

[Project overview](../README.md) · [Setup record](setup.md)

I used Hyper-V to keep the lab on one physical host. The screenshots establish an internal lab switch and the Default Switch. The lab network carried communication between the Windows directory server, workstation, host adapter, and Ubuntu monitoring server. Ubuntu also had a separate adapter on the Default Switch for internet access.

| Component | Observed role and configuration | Evidence |
|---|---|---|
| Hyper-V host | Internal lab switch, Default Switch, and a static /24 address on its lab adapter | Screenshots 003–012 |
| Windows Server 2022 evaluation VM | Directory-services/DNS role, FSMO roles, Agent 2, static lab address | 005, 086, 091–099, 108 |
| Windows 11 evaluation VM | Lab workstation with static addressing and directory-server DNS | 006–007 |
| Ubuntu 26.04.1 LTS VM | Generation 2, 2 vCPU, 4 GB startup RAM, 60 GB dynamic disk | 027–041 |
| Ubuntu eth0 | DHCP on the internet-facing Default Switch; default route | 047–052 |
| Ubuntu eth1 | Static /24 address on the internal lab switch; no separate default route shown | 050–052 |
| Monitoring stack | Zabbix 7.4.16, MariaDB 11.8.6, Apache, PHP 8.5 FPM | 054–082 |
| Time service | Ubuntu Chrony synchronized externally; Windows later selected Ubuntu as its NTP source | 110–121 |

The archive also contains an early Windows monitoring VM with 4 vCPU, 4 GB RAM, and a 100 GB dynamic disk. I document that branch separately. Its setup captures do not prove a working PRTG installation, and it is not part of the final Zabbix monitoring path.

## Traffic and management paths

```mermaid
flowchart TB
  A[Windows management session] -->|SSH: TCP 22| U[Ubuntu monitoring VM]
  A -->|Local browser: 127.0.0.1:8080| T[SSH local forward]
  T -->|Remote loopback: TCP 80| P[Apache and PHP-FPM]
  P --> M[Local MariaDB database]
  Z[Zabbix server on Ubuntu] -->|Passive checks: TCP 10050| D[Windows Agent 2]
  D -->|NTP requests: UDP 123| C[Chrony on Ubuntu]
  C --> N[External NTP source]
  Z -->|Gmail media and trigger action| E[Problem and recovery notifications]
```

The Windows template and agent interface demonstrate passive polling. The installer still showed loopback in its active-check field; I do not claim a working active-check configuration. The loopback web address in the tunnel screenshot is the local management endpoint, not the Ubuntu VM's lab address.

I removed real hostnames, domain names, private addressing, MAC addresses, and account details from the shared evidence. Generic service names, port numbers, loopback addresses, and public package-source domains remain where they help explain the build. See the [full archive](../evidence/archive.md) for the numbered evidence.
