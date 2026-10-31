# What my screenshots show—and what they do not

I included all 142 screenshots from the supplied project archive. Each image has an explanation in the [full archive](../evidence/archive.md). I kept unsuccessful attempts and repeated checks because they help explain how I reached the working setup.

## What I could check directly

- I checked virtual-machine settings and tested connections between the computers.
- I checked installed software versions, the database structure, and the running monitoring programs.
- I reached the Zabbix dashboard and collected Windows health readings.
- I checked the Linux and Windows clocks and their selected time sources.
- I tested Gmail and found both problem and recovery messages marked Sent.
- I read Windows startup, shutdown, and agent service records during the investigation.

The screenshots show Ubuntu 26.04.1 LTS, Zabbix 7.4.16, MariaDB 11.8.6, and PHP 8.5.4. These are the versions captured during my build, not a claim about which versions someone should install today.

## Where the record has limits

| Topic | What I can honestly say |
|---|---|
| Original Windows domain setup | The server and workstation already existed. I did not capture the original steps for creating the domain or joining the workstation. |
| Early Windows monitoring machine | I captured its operating-system and network setup, but not a finished PRTG monitoring installation. |
| Screenshot order | The numbers follow the exported files. They are not precise proof of when every action happened. |
| Cause of the October 30 problem | The timing suggests a connection with startup, but I did not prove the original cause. |
| A manual repair | The agent was already running before the displayed administrator start command. I cannot claim that command restored it. |
| An agent crash | My limited log search returned no crash event. That is not proof that a crash never happened. |
| The five-minute value | It is the recorded problem duration, not a measurement of detection speed, email delay, or downtime for every server function. |
| Email receipt | I confirmed receiving the messages during the lab. The screenshots show Sent, not separate inbox arrival times. |
| Comparing incident times | I used time-zone settings shown earlier. I did not record a new comparison of both clocks during the incident. |
| Final Windows time correction | The final status showed synchronization, but not every change leading to it was captured. |
| Rebuilding the lab | These are build notes, not a complete settings export or a tested automatic installer. |

## How I prepared the images

I covered private information with solid rectangles and cropped one wide terminal image. The remaining pixels were preserved from the originals. I removed embedded PNG metadata, the extra information an image file can carry beyond its visible picture.

The [file-check manifest](../evidence/sanitized-sha256.csv) lists a SHA-256 value for each sanitized evidence image. This is a file fingerprint that can be used to check whether an image file has changed. It does not reveal the hidden original content.

The seven chapters contain all 142 screenshots. The shorter walkthrough selects the main results, and four earlier cropped evidence images remain in the repository. Original images, private account details, and working review files are not included.
