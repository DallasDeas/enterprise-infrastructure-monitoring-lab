# Getting the lab network talking

[Walkthrough](../README.md) · [Full archive](../archive.md)

I began by establishing the network baseline for the existing Windows directory server and workstation. I checked VM state, virtual-switch membership, addressing, and reachability from both the guests and the Hyper-V host. Comparing those paths helped me isolate the host adapter issue without assuming a successful guest test validated the entire network.

## 001 — Checking administrative permissions

I first ran Get-VM without the required permissions and received an access error. I reopened PowerShell with administrator privileges before continuing. Establishing the correct execution context was necessary before interpreting any VM inventory results.

![Screenshot 001: Checking administrative permissions](../screenshots/archive/screenshot_001.png)

## 002 — Establishing VM state

In an elevated PowerShell session, I identified the existing directory server and workstation. Both were in a saved state rather than Running. I checked this baseline before beginning connectivity tests.

![Screenshot 002: Establishing VM state](../screenshots/archive/screenshot_002.png)

## 003 — Checking the virtual switches

I inspected the internal lab switch, the Default Switch, and the VM adapter connections before changing the network.

![Screenshot 003: Checking the virtual switches](../screenshots/archive/screenshot_003.png)

## 004 — Starting the lab machines

I started the two existing VMs and checked that their state changed to Running. This established the starting point for the network checks.

![Screenshot 004: Starting the lab machines](../screenshots/archive/screenshot_004.png)

## 005 — Checking directory-server addressing

I reviewed the server's static IPv4 configuration, subnet mask, and loopback DNS settings. The actual addresses and domain name are removed from this copy.

![Screenshot 005: Checking directory-server addressing](../screenshots/archive/screenshot_005.png)

## 006 — Checking the workstation

I checked the Windows 11 workstation's static address and its DNS setting, which pointed at the directory server. This gave me both ends of the client-to-server path to compare.

![Screenshot 006: Checking the workstation](../screenshots/archive/screenshot_006.png)

## 007 — Fixing a ping command

I initially pasted an angle-bracket placeholder into PowerShell and got a parser error. After using the actual address, all four ping requests received replies.

![Screenshot 007: Fixing a ping command](../screenshots/archive/screenshot_007.png)

## 008 — Testing from the Hyper-V host

The workstation could reach the server, but Test-Connection from the host failed with a resource-related error. I needed to inspect the host's own lab adapter instead of assuming every path worked.

![Screenshot 008: Testing from the Hyper-V host](../screenshots/archive/screenshot_008.png)

## 009 — Identifying an APIPA address

I found an APIPA address on the host's lab adapter. APIPA is fallback addressing used when an interface does not have its intended configuration; here, it placed the host outside the lab's configured IPv4 subnet. This gave me a specific addressing issue to correct.

![Screenshot 009: Identifying an APIPA address](../screenshots/archive/screenshot_009.png)

## 010 — Assigning the host lab address

I listed the host adapters and assigned a static address to the lab adapter. The immediate output included Tentative and Invalid states, so this command alone was not my final validation.

![Screenshot 010: Assigning the host lab address](../screenshots/archive/screenshot_010.png)

## 011 — Rechecking the host adapter

I queried the adapter again to confirm its static address and /24 prefix. I then moved on to a connectivity test.

![Screenshot 011: Rechecking the host adapter](../screenshots/archive/screenshot_011.png)

## 012 — Confirming host-to-server connectivity

The host received four replies from the directory server. That gave me a successful check after correcting the host's lab addressing.

![Screenshot 012: Confirming host-to-server connectivity](../screenshots/archive/screenshot_012.png)

## 013 — Checking available resources

I reviewed available memory, VM allocations, and free disk space before adding another VM. These are the allocations I captured, not a recommendation to give every lab VM the same CPU count.

![Screenshot 013: Checking available resources](../screenshots/archive/screenshot_013.png)
