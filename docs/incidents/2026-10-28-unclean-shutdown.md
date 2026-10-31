# October 28: keeping an older shutdown separate

While checking the October 30 monitoring interruption, I found a Windows message saying that an earlier shutdown was unexpected. An unexpected shutdown means Windows did not record a normal, orderly shutdown; the message alone does not explain why.

The dates mattered:

| What the time refers to | Windows local time |
|---|---|
| The earlier shutdown described inside the message | October 28, 2026 at 18:45:59 |
| When Windows recorded that message, Event 6008 | October 30 at 20:06:22 |
| A nearby startup record, Event 41 | October 30 at 20:06:20 |

![The shutdown message and the time it was recorded](../../evidence/screenshots/archive/screenshot_141.png)

I had to read the message, not just its date in the event list. Otherwise, I could have incorrectly described an October 28 shutdown as a new October 30 event.

I could not tell from this screenshot whether the earlier shutdown came from a power loss, a forced stop of the virtual machine, a failure inside Windows, or something else. I also could not prove that it caused either the October 28 clock warning or the October 30 monitoring interruption.

My next step would be to compare the physical computer's Hyper-V records and the virtual machine's Windows records around the October 28 shutdown time, using the same time zone. Those additional records are not included in this archive.

[October 30 investigation](2026-10-30-monitoring-interruption.md) · [All screenshots](../../evidence/archive.md)
