# Infrastructure Monitoring and Incident Investigation Lab

**Hyper-V · Windows Server · Ubuntu · Zabbix · PowerShell · Linux administration**

I built this lab to practice infrastructure monitoring and the investigation work that follows an availability alert. I configured Windows and Linux virtual machines, deployed Zabbix, collected Windows service and performance data, and automated Gmail problem and recovery notifications.

The main case study is an October 30 monitoring interruption. I correlated Zabbix alerts with Windows startup and service events, accounted for time-zone differences, and evaluated what the available records could establish about recovery. I also identified an older shutdown event that needed to remain separate from the current investigation.

This is a home lab focused on availability monitoring and incident analysis. It demonstrates troubleshooting and evidence interpretation; it does not establish a security compromise or production incident-response experience.

## Environment and scope

![Hyper-V lab topology with Windows and Ubuntu servers](docs/images/lab-topology.png)

I used Hyper-V to host the lab. The Windows Server VM provided Active Directory Domain Services (AD DS), DNS, and Zabbix Agent 2. Ubuntu hosted Zabbix, MariaDB, Apache, PHP-FPM, and Chrony for time synchronization. Ubuntu had separate internet-facing and internal lab interfaces.

Zabbix used passive agent checks on TCP 10050 to collect Windows telemetry—service states and system measurements. Windows later selected Ubuntu as its NTP time source on UDP 123. [Architecture and traffic paths](docs/architecture.md).

## What I implemented and validated

| Area | Work completed | Supporting evidence |
|---|---|---|
| Network configuration | Checked virtual switches, static addressing, routing, DNS resolution, and connectivity from multiple systems | [Network foundation](evidence/chapters/01-network-foundation.md) |
| Monitoring platform | Deployed Zabbix 7.4.16 with MariaDB and repaired the Apache/PHP-FPM configuration | [Platform deployment](evidence/chapters/04-zabbix-stack.md) |
| Windows telemetry | Installed Agent 2, applied the Windows template, and inspected collected service and performance values | [Windows monitoring](evidence/chapters/05-windows-monitoring.md) |
| Time synchronization | Investigated clock skew, corrected Chrony access rules, and verified the selected Windows time source | [Time investigation](evidence/chapters/06-time-synchronization.md) |
| Alerting | Tested Gmail delivery and configured both problem and recovery operations | [Alert configuration and results](docs/alerting.md) |
| Incident analysis | Correlated alert, boot, and service records; separated confirmed findings from possible explanations | [October 30 report](docs/incidents/2026-10-30-monitoring-interruption.md) |

## Incident case study: monitoring interruption

![Zabbix problem and recovery notifications marked Sent](evidence/screenshots/archive/screenshot_139.png)

On October 30, Zabbix recorded the Windows agent as unavailable from **23:07:16 to 23:12:16**. The Action log recorded both the problem and recovery notifications as **Sent** through Gmail. I confirmed receiving both during the lab, but did not capture independent mailbox-arrival timestamps.

I investigated three separate questions: whether the agent was running locally, whether Zabbix could reach it, and whether the notification action executed. These checks represent different parts of the recovery process.

Windows records placed a startup and the agent's transition to Running near the monitoring event, subject to the documented time-zone assumption. That supports a possible startup association, but does not prove the initiating cause. An unexpected-shutdown message recorded on October 30 referred to October 28, so I documented it separately.

**The five-minute value is the recorded problem duration, not detection latency.** I also did not establish five minutes of AD DS or DNS downtime. The monitoring connection's status alone cannot support that claim.

## What the investigation taught me

I learned to verify the result of a change rather than rely on a successful command. Windows initially continued using its virtual-machine time provider after I configured an NTP peer. Similarly, Apache was active while the Zabbix page still failed. In both cases, I needed to validate the actual behavior at the next layer.

The incident review reinforced the same approach: a local Running state does not automatically prove remote monitoring has recovered, and a Sent record does not establish mailbox delivery time. I kept these distinctions in the report instead of combining them into an unsupported recovery claim.

## Documentation

- [Investigation walkthrough](evidence/README.md): selected evidence and the reasoning behind my checks.
- [All 142 annotated screenshots](evidence/archive.md): the complete build and troubleshooting record in seven chapters.
- [Setup record](docs/setup.md) and [troubleshooting notes](docs/troubleshooting.md).
- [October 30 incident report](docs/incidents/2026-10-30-monitoring-interruption.md) and [October 28 shutdown finding](docs/incidents/2026-10-28-unclean-shutdown.md).
- [Evidence scope and limitations](docs/verification.md).

## Next validation steps

For a repeat test, I would capture an exact fault-injection timestamp, the effective trigger settings, fresh telemetry after recovery, and mailbox-arrival times. This would allow me to measure detection and notification delays separately. I would also harden the web and agent connections before treating this lab as a model for a production deployment.

The repository remains private pending my review for public release.
