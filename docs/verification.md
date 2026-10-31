# Evidence boundaries

## Directly inspected

Four original screenshots supplied with the source conversation were visually inspected. They show Windows agent service state/boot time, Event 7036, Events 41/6008, and Zabbix Action log entries. Curated derivatives are linked in the [evidence index](../evidence/README.md).

## Owner-confirmed

The lab owner explicitly stated that both emails were received. There is no mailbox screenshot or independent receipt timestamp in the retrieved attachments.

## Conversation-documented, not independently reverified

Windows Server 2022, AD DS/DNS, Ubuntu, MariaDB, Apache, Chrony, the virtual-network isolation design, performance/service monitoring setup, and prior time synchronization work are described in the source conversation. Windows-Pacific/Zabbix-Eastern display settings also come from that narrative rather than the four screenshots.

## Not established

Exact versions other than the conversation's Windows release, VM sizing, network topology details, agent mode, trigger expressions, polling intervals, SMTP authentication method, actual fault onset, exact root cause, application downtime, and a causal manual remediation action.

## Before public release

- Confirm architecture and setup details against the live lab, adding sanitized evidence where useful.
- Verify the timezones and retain a clock-offset check if using second-level correlations.
- Review all screenshot derivatives, prose, filenames, and full Git history for sensitive data.
- Keep the repository private until the owner explicitly approves public visibility.

The retrieved conversation was titled “Resume Project Ideas.” No raw conversation dump or unredacted attachment is included here.
