# How I set up problem and recovery emails

[Project overview](../README.md) · [Screenshots](../evidence/chapters/07-alerting-and-investigation.md)

I wanted to hear about a monitoring problem without keeping the dashboard open. I also wanted a second email when the problem cleared, so the first message would not leave me wondering whether it was still happening.

## How the pieces work together

Zabbix calls a warning rule a **trigger**. It calls the response to that rule an **action**. The email connection is a **media type**. In plain language, I set up: **when Zabbix cannot contact this Windows agent, send me an email; when the problem clears, send another.**

| What I did | Why I did it | Screenshot |
|---|---|---|
| Selected Gmail and prepared app-password access | Give Zabbix a way to send email through the account | 125, 128 |
| Ran an email test | Check the email connection before relying on an automatic rule | 129 |
| Enabled the recipient at all times and severity levels | Allow the demonstration's messages to reach the selected user | 130 |
| Required both the chosen server and its unavailable-agent warning | Keep the action focused on this particular condition | 131 |
| Added the problem message | Notify the user when the action runs | 132 |
| Added the recovery message | Notify the same alert chain when the problem clears | 133 |
| Checked that the action was enabled | Confirm that the saved rule was switched on | 134 |
| Checked both send records | Verify that the automatic notification steps actually ran | 139 |

![Email connection test reporting success](../evidence/screenshots/archive/screenshot_129.png)

A successful test email only checks the email path. It does not prove the automatic problem rule works, which is why I also checked the later action log.

![Recovery notification configuration](../evidence/screenshots/archive/screenshot_133.png)

## What happened on October 30

The action log records the problem notification at **23:07:16** and the recovery notification at **23:12:16**. Both show **Sent** through Gmail. I confirmed receiving both emails during the lab, but I did not save separate screenshots showing when they arrived in the inbox.

![Both problem and recovery messages marked Sent](../evidence/screenshots/archive/screenshot_139.png)

The five-minute gap is how long Zabbix recorded the problem. It does not measure the time between the original failure and Zabbix noticing it, or the time taken for email to arrive. I would need the exact failure time and inbox arrival times to measure those delays.

The setup's **Immediately** option means I did not add another waiting period to the email step. It does not mean detection or delivery is instant.

The screenshots show parts of the email setup, not a complete copy of every final mail-server setting. Passwords are not included, and I have not guessed the missing settings. [October 30 investigation](incidents/2026-10-30-monitoring-interruption.md).
