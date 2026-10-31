# Bringing Windows into monitoring

[Walkthrough](../README.md) · [Full archive](../archive.md)

I installed a small background program called Zabbix Agent 2 on Windows. This agent lets the monitoring server collect information such as memory use and whether a service is running. I checked both that the agent worked on Windows and that Zabbix could actually collect its information over the network.

## 083 — Getting Windows Agent 2

I opened the official download page for Zabbix Agent 2 for Windows. The agent is a small background program that lets Zabbix collect information from the Windows server.

![Screenshot 083: Getting Windows Agent 2](../screenshots/archive/screenshot_083.png)

## 084 — Preparing to transfer the installer

I checked that I had the Windows installer and inspected Hyper-V's file-transfer support for the virtual machine. That support was disabled at first, so I checked it before trying to move the installer.

![Screenshot 084: Preparing to transfer the installer](../screenshots/archive/screenshot_084.png)

## 085 — Enabling guest file-transfer support

I enabled the virtual machine's Guest Service Interface and confirmed it changed to enabled. This feature supports file transfers between the physical computer and the virtual machine.

![Screenshot 085: Enabling guest file-transfer support](../screenshots/archive/screenshot_085.png)

## 086 — Confirming the installer reached Windows Server

I checked inside Windows Server and confirmed the Agent 2 installer file was there. This verified the file had reached the machine where I needed to install it.

![Screenshot 086: Confirming the installer reached Windows Server](../screenshots/archive/screenshot_086.png)

## 087 — Launching Agent 2 setup

I opened the Agent 2 installer on the Windows server. This began installing the program that would supply Windows health information to Zabbix.

![Screenshot 087: Launching Agent 2 setup](../screenshots/archive/screenshot_087.png)

## 088 — Reviewing the monitoring software components

I reviewed the installer components. These included the background monitoring program and tools for sending or requesting monitoring data.

![Screenshot 088: Reviewing the monitoring software components](../screenshots/archive/screenshot_088.png)

## 089 — Configuring the agent connection

I entered the monitoring server's address and used port 10050 for incoming checks. In this setup, Zabbix asks the Windows agent for information. The screenshot does not demonstrate the alternative mode where the agent initiates those checks, and encryption was not selected here.

![Screenshot 089: Configuring the agent connection](../screenshots/archive/screenshot_089.png)

## 090 — Installing the Windows agent

I reached the final installation screen after reviewing the agent settings. I still needed to check the installed program afterward rather than treating this screen alone as proof that monitoring worked.

![Screenshot 090: Installing the Windows agent](../screenshots/archive/screenshot_090.png)

## 091 — Verifying the Windows service

I checked the Windows service and found Zabbix Agent 2 running. A service is a program that works in the background. Its Automatic setting meant it was configured to start when Windows started.

![Screenshot 091: Verifying the Windows service](../screenshots/archive/screenshot_091.png)

## 092 — Checking port 10050

I checked that the agent was accepting connections on port 10050. This showed that it was ready locally, but I still needed to confirm that the separate Zabbix server could reach it.

![Screenshot 092: Checking port 10050](../screenshots/archive/screenshot_092.png)

## 093 — Creating the monitored host

I added the Windows server to Zabbix and selected its Windows monitoring template. A template supplies a ready-made set of checks, so I did not have to create every processor, memory, and service check from scratch.

![Screenshot 093: Creating the monitored host](../screenshots/archive/screenshot_093.png)

## 094 — Telling Zabbix how to reach Windows

I told Zabbix which address and port to use when contacting the Windows agent. I also described the server's role: directory services manage the lab's domain, and DNS helps computers find one another by name.

![Screenshot 094: Telling Zabbix how to reach Windows](../screenshots/archive/screenshot_094.png)

## 095 — Seeing the host become available

The Windows server's ZBX indicator turned green, showing that Zabbix could reach its agent. The Zabbix server itself was also enabled and available.

![Screenshot 095: Seeing the host become available](../screenshots/archive/screenshot_095.png)

## 096 — Inspecting collected data

I opened Latest data to check that information was actually being collected. I could see checks for processor use, storage, memory, and Windows background services. This was stronger evidence than simply seeing the computer listed.

![Screenshot 096: Inspecting collected data](../screenshots/archive/screenshot_096.png)

## 097 — Checking DNS service states

I looked specifically at the DNS checks. Both DNS Server and DNS Client were reported as running. I used the collected values to verify what Zabbix was seeing on Windows.

![Screenshot 097: Checking DNS service states](../screenshots/archive/screenshot_097.png)

## 098 — Reviewing the DNS warning rules

I reviewed the rules that watch the DNS services and raise a problem if their conditions are met. Zabbix calls these rules triggers. The rules shown were enabled and currently reported OK.

![Screenshot 098: Reviewing the DNS warning rules](../screenshots/archive/screenshot_098.png)

## 099 — Comparing two kinds of monitoring warning

I compared two different warning rules: one checks the agent program's reported state, and the other checks whether Zabbix can contact it. Losing contact with a machine is not the same as receiving a report that one of its programs stopped.

![Screenshot 099: Comparing two kinds of monitoring warning](../screenshots/archive/screenshot_099.png)

## 100 — Capturing unavailable status

The Windows server's ZBX indicator was red, meaning the monitoring connection was unavailable. That did not by itself prove that the server's directory or DNS services had stopped working.

![Screenshot 100: Capturing unavailable status](../screenshots/archive/screenshot_100.png)

## 101 — Capturing available status

Another screenshot showed the Windows monitoring connection available again, with a green indicator. I kept both records, but their file numbers alone do not tell me exactly how much time passed between them.

![Screenshot 101: Capturing available status](../screenshots/archive/screenshot_101.png)

## 102 — Stopping the agent for a test

I deliberately stopped the Windows monitoring agent and checked that its state changed to Stopped. This was a test of losing monitoring. The screenshot has no timestamp connecting that test to the separate October 30 interruption.

![Screenshot 102: Stopping the agent for a test](../screenshots/archive/screenshot_102.png)

## 103 — Reviewing a restart warning

Zabbix displayed a warning that the Windows machine had recently restarted. It used the machine's uptime—how long it had been running—to identify that condition. I treated this separately from a lost monitoring connection.

![Screenshot 103: Reviewing a restart warning](../screenshots/archive/screenshot_103.png)

## 104 — Reviewing host problem counts

I reviewed the list of monitored computers, their connection indicators, and their problem counts. This showed how Zabbix summarized the conditions for someone checking the dashboard.

![Screenshot 104: Reviewing host problem counts](../screenshots/archive/screenshot_104.png)
