# Evidence and verification notes

This edition documents all 142 PNGs supplied in the project archive. Every original was inspected, and every image has a numbered explanation in the [archive](../evidence/archive.md). The source ZIP, original PNGs, OCR output, local account paths, and private review materials are not repository content.

## What is directly supported

- Hyper-V switch membership, VM state and hardware checks, host/guest connectivity tests, and Ubuntu dual-interface configuration.
- Ubuntu 26.04.1 LTS, Zabbix 7.4.16, MariaDB 11.8.6, and PHP 8.5.4 as displayed during the build. These are captured versions, not recommendations about today's latest release.
- A populated Zabbix database, active services, completed frontend installation, Windows Agent 2 installation, a listening agent socket, and collected Windows items.
- Chrony offset, source and access checks; later synchronized Linux status and Windows selection of the Linux NTP source.
- Gmail media-test success, enabled trigger action, and problem/recovery sends marked Sent.
- Windows boot and service events used in the two incident notes.

## Where I kept the claims limited

| Topic | Limit |
|---|---|
| Initial AD build | The server and workstation predate this sequence. Domain-promotion and domain-join steps are not captured. |
| Early Windows monitoring VM | OS/network setup is shown. A completed PRTG deployment is not. |
| Screenshot ordering | File numbers follow the supplied export. They do not prove exact chronology between every capture. |
| October 30 root cause | Startup association is supported conditionally on the time basis; a specific initiating fault is not proven. |
| Manual recovery | The elevated screenshot shows Running before Start-Service. It cannot establish that command as the fix. |
| Agent crash | No crash event appears in the bounded query. This is not proof no crash ever occurred. |
| Five-minute event | Problem duration, not fault-to-detection latency, mailbox delay, or measured AD/DNS downtime. |
| Notification receipt | Owner-confirmed in the conversation; screenshot evidence records Sent, without mailbox-arrival timestamps. |
| October 30 time conversion | Earlier screenshots support the display settings, but no same-incident offset measurement was captured. |
| Final Windows time change | Final synchronized status is shown; the exact intervening provider change is not. |
| Reproducibility | This is a build record, not a complete configuration export or tested automated rebuild. |

## Image handling

The published PNGs use opaque solid-color redaction and, for one wide terminal capture, cropping. Pixels outside those edits are preserved from the originals. PNG metadata was removed. A [SHA-256 manifest](../evidence/sanitized-sha256.csv) identifies the sanitized files included in this edition; it does not expose original-file metadata.

The short walkthrough selects the strongest evidence. The seven chapter pages retain all 142 screenshots, including intermediate screens, mistakes, and repeated checks. The four earlier curated images remain as supporting legacy crops.
