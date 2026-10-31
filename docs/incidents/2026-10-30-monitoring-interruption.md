# October 30: checking why monitoring lost contact with Windows

Zabbix recorded that it could not contact the Windows monitoring agent, then marked the problem resolved five minutes later. I checked Windows' own records to understand what happened around that time. The agent was later running, and both the problem and recovery emails were marked Sent.

The key distinction is that **losing monitoring does not automatically mean the whole server stopped working**. The agent is the background program that reports health information. I needed separate evidence to say whether the directory or DNS services were also unavailable.

## What I checked first

I checked that the Windows agent was running and set to start automatically. I then looked at when Windows had started and when its log said the agent entered the Running state. A log is a computer's record of events, such as programs starting or stopping.

![Windows agent status and startup records](../../evidence/screenshots/archive/screenshot_140.png)

I also read the full shutdown messages. One record written on October 30 described a shutdown on October 28. I kept that as a [separate finding](2026-10-28-unclean-shutdown.md), rather than assuming it described a new shutdown on October 30.

## Why I had to account for time zones

Earlier screenshots showed Windows using a time zone seven hours behind UTC and Zabbix displaying New York time, four hours behind UTC. That creates a three-hour difference on the screens even when they refer to the same moment. UTC is a common reference time used to compare systems.

I used those earlier settings to compare the incident times. I did not capture both time-zone settings again during this interruption, so the table assumes they had not changed. The converted times are calculations, not extra measurements.

| What happened | Windows time / Pacific | Zabbix time / Eastern | Calculated UTC |
|---|---|---|---|
| Windows reported its last startup | Oct 30 20:06:29 | Oct 30 23:06:29, converted | Oct 31 03:06:29Z |
| Zabbix recorded the problem and problem email | Oct 30 20:07:16, converted | Oct 30 23:07:16 | Oct 31 03:07:16Z |
| Windows recorded the agent starting | Oct 30 20:08:36 | Oct 30 23:08:36, converted | Oct 31 03:08:36Z |
| Zabbix cleared the problem and recorded the recovery email | Oct 30 20:12:16, converted | Oct 30 23:12:16 | Oct 31 03:12:16Z |

## What the Windows record showed

I searched for agent service events between 20:00 and 20:20 in Windows' displayed time. The result showed Event 7036 at 20:08:36, saying the agent had entered the Running state. An event number identifies a type of log message; 7036 records a service state change.

![Windows record showing the agent entering Running](../../evidence/screenshots/archive/screenshot_142.png)

My search included crash-related events 7031 and 7034, but none appeared in that result. That means I found no crash record in this limited search. It does not prove the agent had never crashed or that every possible record had been checked.

## What the sequence tells me

Using the time-zone assumption above, Windows started 47 seconds before Zabbix recorded the problem. The agent then started before Zabbix cleared it. Zabbix also showed a recent-restart warning. These details suggest that the interruption was associated with startup, but they do not prove what originally caused it.

There were three minutes and 40 seconds between the agent's Running event and Zabbix clearing the problem. Zabbix checks information periodically and uses rules to clear warnings, so those settings could affect that gap. I did not capture enough of the settings and reading history to prove that explanation.

I also checked whether my start command had restored the agent. One screenshot shows a failed attempt without administrator permissions. The next administrator-window screenshot already shows the agent running before the displayed start command. I therefore cannot credit that command as the fix. The earlier deliberate stop test also has no timestamp tying it to this event.

## What five minutes means here

| Question | What I can say |
|---|---|
| How long was the recorded Zabbix problem open? | Five minutes, from 23:07:16 to 23:12:16. |
| How quickly did Zabbix notice the original failure? | I did not measure that; the exact failure start was not captured. |
| How long did the emails take to reach the inbox? | I did not capture independent arrival times. |
| Were directory services or DNS down for five minutes? | This monitoring record does not establish that. |

![Resolved monitoring problem with a separate restart warning still visible](../../evidence/screenshots/archive/screenshot_138.png)

## How I confirmed recovery

The Windows agent was running, Zabbix marked the problem resolved, and the two Gmail records said Sent. I also confirmed receiving both emails during the lab, although the screenshots show the send records rather than inbox arrival times. The separate restart warning was still visible, so I did not describe every warning as cleared at that moment.

For a repeat test, I would record the exact time I stopped the agent, the time Zabbix noticed it, fresh readings after recovery, and when each email arrived. Those checks would let me measure the delays and explain the sequence more confidently.

[Email demonstration](../alerting.md) · [Full investigation screenshots](../../evidence/chapters/07-alerting-and-investigation.md)
