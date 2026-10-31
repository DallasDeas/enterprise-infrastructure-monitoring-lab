# Setup record

This is a reconstruction of the completed lab at the level supported by the available record. It is not an exact installation transcript or a tested one-command deployment. Commands and configuration values from earlier setup stages were not available in the retrieved conversation.

| Stage | Recorded implementation | Verification or missing detail |
|---|---|---|
| 1. Virtualize | Hyper-V VMs with lab networking | Confirm switch type, interface attachment, egress method, and resource allocation |
| 2. Configure directory services | Windows Server 2022 providing AD DS and DNS | Preserve redacted role and DNS validation output |
| 3. Deploy monitoring | Ubuntu with Zabbix, MariaDB, and Apache | Capture versions, package source, service status, and installation choices |
| 4. Configure agent | Zabbix Agent 2 on Windows | Screenshot verifies Running / Automatic; transport mode and settings remain unverified |
| 5. Configure monitoring | Windows performance and service monitoring reported | Preserve template names, item freshness, trigger expression, and recovery expression |
| 6. Align time | Chrony and Windows time troubleshooting reported | Record timezone, selected time source, offset, and synchronization status on both systems |
| 7. Configure alerts | Gmail media and automated problem/recovery action | Action log verifies two sends; owner confirms both receipts |
| 8. Investigate | Correlate Zabbix event with service and boot logs | See the two incident reports and screenshot evidence |

## Validation sequence for a rebuild

1. Confirm VM connectivity and intended isolation; test DNS before debugging application services.
2. Verify database, web, and Zabbix services on the monitoring VM using the actual installed service names.
3. Confirm Zabbix Agent 2 is running, permitted peers match the intended monitoring server, and the configured transport is reachable.
4. Verify fresh item timestamps and working service discovery in Zabbix.
5. Check clock synchronization separately from timezone display configuration.
6. Configure sender authentication privately, then verify recipient media, action conditions, problem operations, and recovery operations.
7. Use an isolated lab test with a written start time. Capture the problem, action status, mailbox receipt, recovery, and fresh data after recovery.

These are validation steps, not claims that every listed check is already preserved in this repository. Use installation instructions matching the versions actually deployed; no package versions are guessed here.
