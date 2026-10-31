# The early Windows VM build

[Walkthrough](../README.md) · [Full archive](../archive.md)

I first worked on another Windows virtual machine and checked its installation and network connection. These screenshots show that early part of the project. The final monitoring system uses Ubuntu instead; this section does not show a finished monitoring application on Windows.

## 014 — An early Windows monitoring VM

I checked an early Windows virtual machine with four virtual processors and 4 GB of starting memory. This was part of an earlier build path. The final monitoring system documented later runs on Ubuntu instead.

![Screenshot 014: An early Windows monitoring VM](../screenshots/archive/screenshot_014.png)

## 015 — Checking that VM's disk and network

I checked that this Windows virtual machine had a lab-network connection and a virtual disk that could grow up to 100 GB. These checks showed that the machine was prepared; they did not show a finished monitoring system.

![Screenshot 015: Checking that VM's disk and network](../screenshots/archive/screenshot_015.png)

## 016 — Starting Windows Server setup

I started the Windows Server installation and reviewed the language and keyboard settings. This was the beginning of setting up the operating system on the early Windows virtual machine.

![Screenshot 016: Starting Windows Server setup](../screenshots/archive/screenshot_016.png)

## 017 — Reviewing installation choices

I reached the screen asking how to install Windows. I kept it as a record of the installation process, but this image alone does not show which option I selected next.

![Screenshot 017: Reviewing installation choices](../screenshots/archive/screenshot_017.png)

## 018 — Selecting the Windows disk

I checked the disk that Windows would be installed on. The installer showed 100 GB of empty space available inside the virtual machine.

![Screenshot 018: Selecting the Windows disk](../screenshots/archive/screenshot_018.png)

## 019 — Reaching Server Manager

I reached the Windows Server desktop and its management screen after installation. The operating system was available, but I still needed to check its network connection before using it in the lab.

![Screenshot 019: Reaching Server Manager](../screenshots/archive/screenshot_019.png)

## 020 — Checking the Windows VM identity

I ran hostname to check which virtual computer I was using. When several machines are open at once, this helps avoid making a change on the wrong one. I covered the actual machine name in the shared screenshot.

![Screenshot 020: Checking the Windows VM identity](../screenshots/archive/screenshot_020.png)

## 021 — A connection can be on without working

The virtual machine reported that its network connection was on, but my physical computer still could not reach it. This showed me that an enabled connection and a working connection are two different things.

![Screenshot 021: A connection can be on without working](../screenshots/archive/screenshot_021.png)

## 022 — Verifying guest network and DNS

I checked the virtual machine's address, set its DNS server, and tested the connection to the directory server. I received four replies and successfully looked up the server by name. Both the basic connection and that name lookup worked.

![Screenshot 022: Verifying guest network and DNS](../screenshots/archive/screenshot_022.png)

## 023 — Rechecking the host path

I tested the connection from my physical computer to the early monitoring machine again, and it still failed. I kept this separate from the successful test between virtual machines because the requests were taking different paths.

![Screenshot 023: Rechecking the host path](../screenshots/archive/screenshot_023.png)

## 024 — Comparing a failing and a working destination

I compared two destinations from the same computer. The directory server answered, but the early monitoring machine did not. This narrowed down where to look, although it did not prove whether a firewall or another network setting was responsible.

![Screenshot 024: Comparing a failing and a working destination](../screenshots/archive/screenshot_024.png)

## 025 — Checking the route to the internet

I checked which network connection the Windows machine used to reach destinations outside the lab. Its internet connection provided the default route—the usual way out—while its lab connection did not have a separate gateway.

![Screenshot 025: Checking the route to the internet](../screenshots/archive/screenshot_025.png)

## 026 — Testing internet access and name resolution

I successfully tested an internet connection and looked up a Microsoft website address. A separate lookup for a lab computer name was refused. This showed why I needed to test internet access and local name lookup separately.

![Screenshot 026: Testing internet access and name resolution](../screenshots/archive/screenshot_026.png)
