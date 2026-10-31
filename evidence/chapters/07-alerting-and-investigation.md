# Closing the loop with alerts and investigation

[Walkthrough](../README.md) · [Full archive](../archive.md)

I configured Gmail notification actions and investigated the October 30 agent-availability interruption. My analysis separates the alert condition, local service state, recovery notification, and historical shutdown evidence. I used Windows events and Zabbix records to build a conditional timeline without claiming a root cause that the captures do not prove.

## 124 — Opening email media settings

I opened the email media form, which initially contained generic SMTP examples. These are not the final Gmail settings.

![Screenshot 124: Opening email media settings](../screenshots/archive/screenshot_124.png)

## 125 — Preparing app-password access

I reached Google's App passwords creation screen. The screenshot does not contain a generated app password, and none is included in this repository.

![Screenshot 125: Preparing app-password access](../screenshots/archive/screenshot_125.png)

## 126 — Opening an SSH tunnel

I opened an SSH local forward from port 8080 to the monitoring server's loopback web service on port 80. Private connection details are covered.

![Screenshot 126: Opening an SSH tunnel](../screenshots/archive/screenshot_126.png)

## 127 — Using the tunneled login page

I reached the Zabbix login page through 127.0.0.1:8080 on my local machine.

![Screenshot 127: Using the tunneled login page](../screenshots/archive/screenshot_127.png)

## 128 — Selecting Gmail as the provider

I selected Gmail and username/password authentication in Zabbix. The credential fields are blank in this capture; it documents the configuration step, not the secret values.

![Screenshot 128: Selecting Gmail as the provider](../screenshots/archive/screenshot_128.png)

## 129 — Testing the Gmail media type

The media test reported success. This validated the send path before I relied on a trigger action.

![Screenshot 129: Testing the Gmail media type](../screenshots/archive/screenshot_129.png)

## 130 — Enabling the recipient media

I checked the user's Gmail media entry, with all severities and a continuous schedule enabled. The recipient address is removed.

![Screenshot 130: Enabling the recipient media](../screenshots/archive/screenshot_130.png)

## 131 — Scoping the alert action

I set the action conditions to the directory server and its agent-unavailable trigger, combined with AND. This kept the demonstration focused on a specific condition.

![Screenshot 131: Scoping the alert action](../screenshots/archive/screenshot_131.png)

## 132 — Adding the problem notification

I configured the problem operation to send through Gmail to the designated Zabbix user immediately when the action ran.

![Screenshot 132: Adding the problem notification](../screenshots/archive/screenshot_132.png)

## 133 — Adding recovery notification

I added Notify all involved as the recovery operation. I wanted the same alert chain to close the loop when the problem cleared.

![Screenshot 133: Adding recovery notification](../screenshots/archive/screenshot_133.png)

## 134 — Confirming the action was enabled

Zabbix reported the action was added, and the action list showed it enabled.

![Screenshot 134: Confirming the action was enabled](../screenshots/archive/screenshot_134.png)

## 135 — Running into service permissions

A non-elevated service-start attempt failed because it could not open the service. The service query in this capture showed Stopped. This command error alone does not explain the later incident's cause.

![Screenshot 135: Running into service permissions](../screenshots/archive/screenshot_135.png)

## 136 — Assessing the claimed recovery action

The elevated PowerShell capture showed Agent 2 already Running before the displayed Start-Service command. I therefore could not identify that command as the recovery action. I kept the observed state separate from assumptions about what restored it.

![Screenshot 136: Assessing the claimed recovery action](../screenshots/archive/screenshot_136.png)

## 137 — Checking availability during the interruption

The frontend showed the Windows agent unavailable. I used the subsequent problem and action logs to establish the recorded event times.

![Screenshot 137: Checking availability during the interruption](../screenshots/archive/screenshot_137.png)

## 138 — Recording the five-minute problem

The October 30 problem entry ran from 23:07:16 to 23:12:16 and was resolved. A separate recent-restart warning was still active in this capture.

![Screenshot 138: Recording the five-minute problem](../screenshots/archive/screenshot_138.png)

## 139 — Validating problem and recovery notifications

The October 30 Action log recorded both the problem and recovery notifications as Sent through Gmail. The five-minute interval describes the recorded problem duration. It does not measure fault-to-detection latency or mailbox delivery delay.

![Screenshot 139: Validating problem and recovery notifications](../screenshots/archive/screenshot_139.png)

## 140 — Checking service and boot context

I confirmed the agent was Running with Automatic startup and read the last boot time. Startup-related events were recorded around the same boot window.

![Screenshot 140: Checking service and boot context](../screenshots/archive/screenshot_140.png)

## 141 — Reading the shutdown message carefully

Event 6008 was recorded on October 30, but its message named an unexpected shutdown on October 28 at 18:45:59 Windows local time. I documented that as a separate historical finding.

![Screenshot 141: Reading the shutdown message carefully](../screenshots/archive/screenshot_141.png)

## 142 — Reviewing service-state evidence

I queried service events within the 20:00–20:20 Windows-local window. The result showed Event 7036 at 20:08:36 for Agent 2 entering Running. No crash event appeared in that bounded result; I did not treat the absence as proof that no crash could have occurred outside the search scope.

![Screenshot 142: Reviewing service-state evidence](../screenshots/archive/screenshot_142.png)
