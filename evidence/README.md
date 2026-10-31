# Monitoring and recovery: annotated screenshot walkthrough

This walkthrough follows the investigation in a practical order: notification result, Windows state, service-event timing, and the separate historical shutdown finding. Each image explains both what is visible and how it supports the investigation.

## 1. Verify the automated notification workflow

![Zabbix Gmail problem and recovery notification records](screenshots/04-gmail-action-log.png)

**What to look at:** The two rows show the Gmail media type and green `Sent` statuses. The problem notification is recorded at 23:07:16 on October 30, 2026; recovery is recorded at 23:12:16. The recovery subject states `Resolved in 5m 0s`.

**What this demonstrates:** Zabbix executed both sides of the notification workflow: reporting a problem and reporting recovery. The owner separately confirmed receipt of both messages, completing the available end-to-end validation.

**Why it matters operationally:** Recovery notification helps an operator distinguish an active problem from one that has cleared. Checking the action log also separates notification execution from the underlying monitoring condition.

**Boundary:** `Sent` is the Zabbix send result, not an independently timestamped mailbox receipt. Five minutes is event duration, not measured detection latency. The `for 3m` text in the problem name is not a substitute for inspecting the trigger expression.

## 2. Establish the Windows server's current state and boot context

![PowerShell service state and Windows last boot time](screenshots/03-service-and-boot.png)

**What to look at:** `Get-Service` shows Zabbix Agent 2 as `Running` with startup type `Automatic`. `LastBootUpTime` is October 30 at 20:06:29 Windows local time. The displayed event query also returns startup/shutdown-related records at 20:06:22.

**What this demonstrates:** The investigation checked both service state and recent boot context instead of assuming that an unavailable agent had crashed. The current state confirms that the service was running when checked; boot context provides a reason to inspect the startup sequence.

**Why it matters operationally:** A service check answers whether the process is currently available locally. Boot and event history help explain how it reached that state. These are distinct diagnostic questions.

**Boundary:** A running Windows service does not by itself establish that Zabbix is receiving fresh data or that AD/DNS was continuously available.

## 3. Correlate service startup with monitoring recovery

![Filtered System event query showing Zabbix Agent 2 entering the running state](screenshots/01-agent-running-event.png)

**What to look at:** The query targets System Events `7031`, `7034`, and `7036` in the 20:00–20:20 Windows-local window and filters for Zabbix Agent 2. The displayed result is Event `7036` at 20:08:36: the service entered the running state.

**What this demonstrates:** The investigation moved from a current-state check to a timestamped service event. Using the conversation-documented Pacific/Eastern display difference, this event corresponds to 23:08:36 Eastern—after the Zabbix problem and before its resolution.

**Why it matters operationally:** Correlating independent records provides a stronger explanation than attributing recovery to an intended troubleshooting action. Here, startup association is supported; a manual service restart is not established as the fix.

**Boundary:** No unexpected-termination event appears in the displayed query result. That statement applies to this query and interval only. The screenshot does not itself verify timezone settings or clock synchronization.

## 4. Separate the previous shutdown from the current incident

![Windows events identifying a previous October 28 unexpected shutdown](screenshots/02-previous-unclean-shutdown.png)

**What to look at:** Event `6008` was recorded on October 30, but its message explicitly names the previous shutdown at 18:45:59 on October 28. Event `41` also appears in the October 30 startup window.

**What this demonstrates:** The investigation distinguished the time an event was recorded from the historical event described inside its message. The October 28 shutdown is documented separately rather than presented as a newly proven October 30 crash.

**Why it matters operationally:** Accurate incident timelines depend on reading event details, not just event IDs and list timestamps. This distinction prevents an unsupported root-cause conclusion.

**Boundary:** The screenshot does not identify whether the earlier shutdown resulted from a VM power-off, host interruption, or guest crash. Its cause remains undetermined.

## Read the full investigation

- [October 30 timeline and recovery analysis](../docs/incidents/2026-10-30-monitoring-interruption.md)
- [Separate October 28 shutdown finding](../docs/incidents/2026-10-28-unclean-shutdown.md)
- [Diagnostic runbook](../docs/troubleshooting.md)

## Evidence handling and index

These are cropped/redacted derivatives of original lab screenshots, not recreated interfaces. Cropping removes title bars, unrelated desktop UI, and blank space. Solid rectangles remove identifiers. Remaining screenshot pixels are preserved; PNG metadata is stripped. Originals are not committed.

| ID | Screenshot | What it establishes |
|---|---|---|
| E01 | [Agent running event](screenshots/01-agent-running-event.png) | Query window and Event 7036 at 20:08:36 on October 30 |
| E02 | [Previous unclean shutdown](screenshots/02-previous-unclean-shutdown.png) | Events recorded October 30; Event 6008 names October 28 18:45:59 |
| E03 | [Service state and boot](screenshots/03-service-and-boot.png) | Running / Automatic, last boot, and startup event output |
| E04 | [Gmail action log](screenshots/04-gmail-action-log.png) | Two Gmail sends, event times, recovery duration and Sent statuses |

E04 masks the action name, recipient fields, and message-body lines containing host identifiers. The subject lines, notification times, media type, and send statuses remain visible. The browser/title/sidebar area is cropped out. Windows evidence is cropped below the machine-identifying VM title bar. The screenshot evidence does not itself establish mailbox receipt or timezone configuration.
