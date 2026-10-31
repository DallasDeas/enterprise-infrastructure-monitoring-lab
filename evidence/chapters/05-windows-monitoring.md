# Bringing Windows into monitoring

[Walkthrough](../README.md) · [Full archive](../archive.md)

Once Zabbix could monitor itself, I installed Agent 2 on the Windows directory server. I checked the Windows service and listening port, added the passive agent interface in Zabbix, and inspected real collected values. I also captured unavailable and available states and a deliberate service-stop test. The file numbers alone are not a stopwatch for that test.

## 083 — Getting Windows Agent 2

I opened the official Zabbix download page for the Windows Agent 2 7.4.16 installer.

![Screenshot 083: Getting Windows Agent 2](../screenshots/archive/screenshot_083.png)

## 084 — Preparing to transfer the installer

I checked the downloaded MSI and the VM's Guest Service Interface. The integration service was initially disabled.

![Screenshot 084: Preparing to transfer the installer](../screenshots/archive/screenshot_084.png)

## 085 — Enabling guest file-transfer support

I enabled the Guest Service Interface and verified it changed from False to True. This prepared the guest integration feature used during installer transfer.

![Screenshot 085: Enabling guest file-transfer support](../screenshots/archive/screenshot_085.png)

## 086 — Confirming the installer reached Windows Server

I checked the installer directory inside the Windows Server 2022 evaluation guest and confirmed the Agent 2 MSI was present.

![Screenshot 086: Confirming the installer reached Windows Server](../screenshots/archive/screenshot_086.png)

## 087 — Launching Agent 2 setup

I opened the Windows Agent 2 installer inside the server VM.

![Screenshot 087: Launching Agent 2 setup](../screenshots/archive/screenshot_087.png)

## 088 — Reviewing Agent 2 components

I reviewed the installer components, including the agent daemon, sender, and get utility.

![Screenshot 088: Reviewing Agent 2 components](../screenshots/archive/screenshot_088.png)

## 089 — Configuring the agent connection

I entered the monitoring server as the allowed Zabbix server and used listen port 10050. The active-check field still showed loopback and PSK was unchecked; the monitoring demonstrated later uses the passive agent interface.

![Screenshot 089: Configuring the agent connection](../screenshots/archive/screenshot_089.png)

## 090 — Installing the Windows agent

I reached the final installation screen after reviewing the Agent 2 settings.

![Screenshot 090: Installing the Windows agent](../screenshots/archive/screenshot_090.png)

## 091 — Verifying the Windows service

I queried the service and confirmed Zabbix Agent 2 was Running with Automatic startup.

![Screenshot 091: Verifying the Windows service](../screenshots/archive/screenshot_091.png)

## 092 — Checking port 10050

I checked the service again and confirmed a listening TCP endpoint on port 10050. This separated local agent readiness from connectivity in the Zabbix frontend.

![Screenshot 092: Checking port 10050](../screenshots/archive/screenshot_092.png)

## 093 — Creating the monitored host

I added the Windows host to Zabbix with the Windows by Zabbix agent template and the Windows servers group.

![Screenshot 093: Creating the monitored host](../screenshots/archive/screenshot_093.png)

## 094 — Adding its agent interface

I configured the server's agent interface on port 10050 and described its directory-services and DNS role. The real address and machine name are redacted.

![Screenshot 094: Adding its agent interface](../screenshots/archive/screenshot_094.png)

## 095 — Seeing the host become available

The host list showed both the Windows agent and the Zabbix server enabled with green ZBX availability.

![Screenshot 095: Seeing the host become available](../screenshots/archive/screenshot_095.png)

## 096 — Inspecting collected data

I opened Latest data and reviewed CPU, disk, memory, and Windows service items. This checked that the host was producing data, beyond simply appearing in the inventory.

![Screenshot 096: Inspecting collected data](../screenshots/archive/screenshot_096.png)

## 097 — Checking DNS service states

I filtered the collected items for DNS. Both DNS Server and DNS Client showed Running (0) in the captured values.

![Screenshot 097: Checking DNS service states](../screenshots/archive/screenshot_097.png)

## 098 — Reviewing DNS triggers

I inspected the discovered DNS service triggers and their expressions. They were enabled and in an OK state at this point.

![Screenshot 098: Reviewing DNS triggers](../screenshots/archive/screenshot_098.png)

## 099 — Reviewing agent triggers

I compared the agent service-state trigger with the agent-availability trigger. These are different checks: one reads a service state, while the other reports whether Zabbix can reach the agent.

![Screenshot 099: Reviewing agent triggers](../screenshots/archive/screenshot_099.png)

## 100 — Capturing unavailable status

The Windows host's ZBX indicator was red in this capture. This records an unavailable monitoring interface, not proof that directory services or DNS had stopped.

![Screenshot 100: Capturing unavailable status](../screenshots/archive/screenshot_100.png)

## 101 — Capturing available status

Another host-list capture shows the Windows ZBX indicator green again. I retained both states without using file order alone as a precise event timeline.

![Screenshot 101: Capturing available status](../screenshots/archive/screenshot_101.png)

## 102 — Stopping the agent for a test

I deliberately stopped Zabbix Agent 2 and confirmed Stopped with Automatic startup still configured. This demonstrates a service-stop test, but the screenshot has no timestamp tying it to the later October 30 event.

![Screenshot 102: Stopping the agent for a test](../screenshots/archive/screenshot_102.png)

## 103 — Reviewing a restart warning

Zabbix showed a host-restarted warning based on uptime. I treated this as a separate signal from agent availability.

![Screenshot 103: Reviewing a restart warning](../screenshots/archive/screenshot_103.png)

## 104 — Reviewing host problem counts

I checked the host list, availability, and problem counts to see how the conditions appeared from the monitoring side.

![Screenshot 104: Reviewing host problem counts](../screenshots/archive/screenshot_104.png)
