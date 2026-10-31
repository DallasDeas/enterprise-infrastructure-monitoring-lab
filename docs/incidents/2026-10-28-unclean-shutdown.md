# October 28: a separate unclean-shutdown finding

While investigating the October 30 monitoring interruption, I found an Event 6008 whose message said the previous shutdown on **October 28, 2026 at 18:45:59 Windows local time** was unexpected. The event itself was recorded on **October 30 at 20:06:22 Windows local time**. Event 41 was recorded at 20:06:20 in the same startup window.

![Shutdown-event messages and recording times](../../evidence/screenshots/archive/screenshot_141.png)

The recording date and the date inside the message describe different things. I kept the October 28 shutdown as its own finding instead of treating the October 30 log entry as proof of a new unclean shutdown that evening.

The screenshot does not establish whether the earlier shutdown came from a host power loss, forced VM stop, guest failure, or another cause. It also does not prove the shutdown caused the separate October 28 clock warning or the October 30 monitoring problem. Those connections would need more evidence.

My next check would be the relevant Hyper-V host and guest records around the October 28 shutdown time, using an explicit time-zone basis. I did not capture that complete causal chain in this archive.

[October 30 investigation](2026-10-30-monitoring-interruption.md) · [Full screenshot archive](../../evidence/archive.md)
