# Automated Gmail alert demonstration

## Observed result

The Zabbix Action log shows two automated Gmail notifications for the same agent-unavailability problem. The problem title includes `Zabbix agent is not available (for 3m)` and both records show `Sent`.

| Record | Displayed time, October 30, 2026 | Evidence |
|---|---|---|
| Problem notification | 23:07:16 | Gmail media type; Sent status |
| Recovery notification | 23:12:16 | Gmail media type; Sent status; subject reports 5m 0s |
| Mailbox receipt | Exact times not captured | Owner explicitly confirmed receiving both emails in the source conversation |

![Redacted Zabbix action log](../evidence/screenshots/04-gmail-action-log.png)

The log's send status and the owner's receipt confirmation support an end-to-end demonstration for this pair of messages. They do not establish a general delivery success rate or delivery latency. The `for 3m` text is part of the problem title; the full trigger expression and polling history were not captured, so it is not a measured detection time.

## Configuration record

Observed: a Gmail media type and an action for agent outage notifications. Sender settings, authentication method, recipient media settings, action conditions, escalation steps, and recovery-operation configuration are not visible in the available screenshot.

For a reconstruction, verify the email media type, configure credentials only in the live application, associate the intended recipient, and check both problem and recovery operations. Zabbix documents email media configuration and notes that media-type exports can contain a clear-text password; do not commit raw exports. [Official Zabbix email documentation](https://www.zabbix.com/documentation/current/en/manual/config/notifications/media/email).

## Presentation finding

The action-log message bodies visibly contain HTML tags. This establishes how the log displays those bodies, not how Gmail rendered them. Check the received messages before claiming polished HTML formatting. Plain text templates are a reasonable future simplification if tags appear literally in the mailbox.
