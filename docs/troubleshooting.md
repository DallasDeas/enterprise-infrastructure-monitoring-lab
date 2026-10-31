# Troubleshooting runbook

This runbook is a future-use guide. It is not a claim that every procedure below was executed during the documented incident.

| Symptom | Investigate first | Evidence to preserve |
|---|---|---|
| Agent unavailable | Guest power state, service state, connectivity, allowed peers, firewall and configured active/passive mode | Service output, Zabbix item errors, timestamps |
| Service running but problem unresolved | Item freshness, server reachability, trigger and recovery expressions, evaluation windows | Item history and exact expression; do not infer availability from service state alone |
| Email not received | Action match, recipient media, enabled periods/severity, send status, mail filtering | Sanitized action status and mailbox timestamps |
| Mismatched times | Timezone display first, then synchronization source and offset | Timezone and time-service output from both systems |
| Unexpected shutdown | Event message's prior-shutdown time, boot context, host events, crash evidence | Full event details retained privately; sanitized excerpts in repository |

## Read-only Windows checks

Run in the Windows guest. Output can contain identifiers; review it before sharing.

```powershell
Get-Service -Name 'Zabbix Agent 2' |
    Select-Object Name, Status, StartType
Get-CimInstance Win32_OperatingSystem |
    Select-Object LastBootUpTime
Get-TimeZone | Select-Object Id, DisplayName
w32tm /query /status
```

For the historical service-event window (Windows local time as shown in the evidence):

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'System'
    Id = 7031,7034,7036
    StartTime = [datetime]'2026-10-30T20:00:00'
    EndTime = [datetime]'2026-10-30T20:20:00'
} | Where-Object { $_.Message -match 'Zabbix Agent 2' } |
    Select-Object TimeCreated, Id, Message
```

If logs have aged out, the command cannot reconstruct them. No matching result can also mean unavailable evidence; distinguish that from a successful query finding no relevant events. Avoid suppressing errors in a new investigation.

## Read-only Ubuntu time checks

```sh
timedatectl status
chronyc tracking
chronyc sources -v
```

These are diagnostic examples for systems using systemd and Chrony. Selected sources may reveal addresses; redact them. For service diagnostics, use the actual installed unit names rather than assuming a package layout.
