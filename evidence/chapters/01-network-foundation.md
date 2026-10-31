# Getting the lab network talking

[Walkthrough](../README.md) · [Full archive](../archive.md)

I started with two virtual computers already inside Hyper-V: a Windows server and a Windows workstation. Hyper-V is the software that lets my physical computer run these separate machines. Before adding monitoring, I needed to check that they could talk to one another and to the physical computer.

## 001 — Getting permission to manage the lab

I first tried to list my virtual machines, which are separate computers running inside my physical computer. PowerShell refused because I had not opened it with administrator permissions. I needed those permissions to manage the lab.

![Screenshot 001: Getting permission to manage the lab](../screenshots/archive/screenshot_001.png)

## 002 — Finding the existing VMs

I reopened PowerShell as an administrator and could now see the Windows server and workstation. Both were in a saved state, meaning their session information had been stored and they were not currently running. I checked this before trying to connect to them.

![Screenshot 002: Finding the existing VMs](../screenshots/archive/screenshot_002.png)

## 003 — Checking the virtual switches

I checked the virtual switches—the software equivalent of the network equipment that connects computers. I also checked which switch each virtual machine was connected to. This helped me understand the network before changing anything.

![Screenshot 003: Checking the virtual switches](../screenshots/archive/screenshot_003.png)

## 004 — Starting the lab machines

I started the two existing virtual machines and checked that both showed Running. With the machines switched on, I could begin testing whether they could communicate.

![Screenshot 004: Starting the lab machines](../screenshots/archive/screenshot_004.png)

## 005 — Checking the server network settings

I checked the Windows server's network address and DNS settings. A fixed address makes the server easier for other machines to find. DNS translates computer names into network addresses; this server's setting pointed back to itself.

![Screenshot 005: Checking the server network settings](../screenshots/archive/screenshot_005.png)

## 006 — Checking the workstation

I checked the Windows 11 workstation's network settings. Its DNS setting pointed to the Windows server, so that server would help it find other lab computers by name. I compared the two machines' settings before testing the connection.

![Screenshot 006: Checking the workstation](../screenshots/archive/screenshot_006.png)

## 007 — Fixing a ping command

My first test failed because I pasted example text where the command needed a real network address. After correcting it, I received four replies. This ping test showed that the other computer could answer a basic network request.

![Screenshot 007: Fixing a ping command](../screenshots/archive/screenshot_007.png)

## 008 — Testing from the Hyper-V host

The workstation could reach the server, but my physical computer could not. I tested from the physical computer separately because a working connection from one machine does not prove every other machine can connect.

![Screenshot 008: Testing from the Hyper-V host](../screenshots/archive/screenshot_008.png)

## 009 — Finding the wrong network address

I found that my physical computer's lab connection had given itself an automatic fallback address, called APIPA. That address did not match the lab's network range. This gave me a specific setting to correct.

![Screenshot 009: Finding the wrong network address](../screenshots/archive/screenshot_009.png)

## 010 — Giving the host a fixed address

I assigned a fixed address to my physical computer's lab connection so it could use the same network as the virtual machines. The first results still showed temporary or invalid states, so I did not assume the change had worked yet.

![Screenshot 010: Giving the host a fixed address](../screenshots/archive/screenshot_010.png)

## 011 — Checking the new address

I checked the network settings again and confirmed the fixed address and network range. My next step was to send a test request to the server. Seeing the right setting is useful, but a successful connection is stronger evidence.

![Screenshot 011: Checking the new address](../screenshots/archive/screenshot_011.png)

## 012 — Confirming host-to-server connectivity

I repeated the test from my physical computer and received all four replies from the server. This showed that the basic connection worked after I corrected the address.

![Screenshot 012: Confirming host-to-server connectivity](../screenshots/archive/screenshot_012.png)

## 013 — Checking available resources

I checked how much memory and storage my computer had available before adding another virtual machine. Virtual machines share the physical computer's resources, so I needed to make sure there was room for the next part of the lab.

![Screenshot 013: Checking available resources](../screenshots/archive/screenshot_013.png)
