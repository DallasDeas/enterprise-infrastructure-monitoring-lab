# Building the Ubuntu monitoring server

[Walkthrough](../README.md) · [Full archive](../archive.md)

I built an Ubuntu virtual machine to run the monitoring system. Ubuntu is a Linux operating system. I gave it one network connection for downloading software from the internet and another for reaching the Windows machines in my lab. I tested both connections separately.

## 027 — Preparing the Ubuntu VM

I prepared the Ubuntu virtual machine with two virtual processors and 4 GB of starting memory. Ubuntu is the Linux operating system I used for the final monitoring server.

![Screenshot 027: Preparing the Ubuntu VM](../screenshots/archive/screenshot_027.png)

## 028 — Catching setup mistakes

I checked the new machine's disk, network, and installation media. I found Windows installation media in the settings and an incorrectly combined command in my checks. I needed to confirm the setup before continuing with Ubuntu.

![Screenshot 028: Catching setup mistakes](../screenshots/archive/screenshot_028.png)

## 029 — Checking mounted installation media

I listed the installation files attached to the virtual machines and could see both Windows and Ubuntu files. These ISO files act like installation DVDs. This check helped me confirm that the Ubuntu installer was available.

![Screenshot 029: Checking mounted installation media](../screenshots/archive/screenshot_029.png)

## 030 — Checking how Ubuntu is allowed to start

I checked Secure Boot, which controls which startup software a machine is allowed to load. The Ubuntu machine had it enabled with the Microsoft UEFI Certificate Authority setting shown in the screenshot.

![Screenshot 030: Checking how Ubuntu is allowed to start](../screenshots/archive/screenshot_030.png)

## 031 — Starting Ubuntu networking

The Ubuntu installer found its first network connection and received an address automatically through DHCP. DHCP supplies network settings without me typing in a fixed address. I used this connection during installation.

![Screenshot 031: Starting Ubuntu networking](../screenshots/archive/screenshot_031.png)

## 032 — Leaving the installer proxy blank

I left the installer's proxy field empty. A proxy is an optional intermediary for network requests, and I did not document a separate proxy for this lab.

![Screenshot 032: Leaving the installer proxy blank](../screenshots/archive/screenshot_032.png)

## 033 — Choosing Ubuntu storage

I selected the 60 GB virtual disk and LVM, a Linux tool for organizing disk space. The screen also shows that disk encryption was not selected at this stage.

![Screenshot 033: Choosing Ubuntu storage](../screenshots/archive/screenshot_033.png)

## 034 — Checking how disk space is divided

I reviewed how Ubuntu would divide the disk between startup files, the main system, and unused space. The main system area was about 28.5 GB; it did not use the entire 60 GB disk.

![Screenshot 034: Checking how disk space is divided](../screenshots/archive/screenshot_034.png)

## 035 — Reaching profile setup

I reached the screen for creating the Ubuntu account. I took this screenshot before entering the account information, so it does not expose a username or password.

![Screenshot 035: Reaching profile setup](../screenshots/archive/screenshot_035.png)

## 036 — Skipping Ubuntu Pro enrollment

I chose to continue without signing up for Ubuntu Pro, Ubuntu's optional subscription service. I then continued with the lab installation.

![Screenshot 036: Skipping Ubuntu Pro enrollment](../screenshots/archive/screenshot_036.png)

## 037 — Reviewing SSH installation options

I reviewed the option to install SSH, which lets me control Linux remotely through a command window. The box was not selected in this particular capture. I used later installation and connection checks to confirm SSH was actually installed.

![Screenshot 037: Reviewing SSH installation options](../screenshots/archive/screenshot_037.png)

## 038 — Watching installation finish

I watched the installation log, which is the installer's running record of its work. It showed the OpenSSH server being installed. This confirmed more than the earlier options screen did.

![Screenshot 038: Watching installation finish](../screenshots/archive/screenshot_038.png)

## 039 — Removing installation media

Ubuntu asked me to remove the installation media and continue. This was the end-of-installation step before starting the system from its installed virtual disk.

![Screenshot 039: Removing installation media](../screenshots/archive/screenshot_039.png)

## 040 — Checking VM hardware after installation

I checked the new machine's memory, processors, disk, and network after installation. I also confirmed the installation DVDs were no longer attached. A saved checkpoint was visible, but I had not tested it as a backup.

![Screenshot 040: Checking VM hardware after installation](../screenshots/archive/screenshot_040.png)

## 041 — Logging into Ubuntu

I logged into the installed Ubuntu system and confirmed it reported version 26.04.1 LTS. I covered account information, network details, and identifying key information before sharing the screenshot.

![Screenshot 041: Logging into Ubuntu](../screenshots/archive/screenshot_041.png)

## 042 — Checking remote access

I checked that SSH was running and accepting connections on port 22. A port is a numbered entry point for a network service. The screen also showed that SSH could be started through its connection listener, which explains the otherwise confusing disabled label.

![Screenshot 042: Checking remote access](../screenshots/archive/screenshot_042.png)

## 043 — Refreshing package information

I refreshed Ubuntu's list of available software updates with apt update. This step checks what updates exist; it does not install them by itself. The result showed 40 packages available to upgrade.

![Screenshot 043: Refreshing package information](../screenshots/archive/screenshot_043.png)

## 044 — Completing package updates

I reviewed the software-update results and notices about programs that might need restarting. Some sessions were still using older program files, so finishing an update did not automatically mean every running session had switched to the new version.

![Screenshot 044: Completing package updates](../screenshots/archive/screenshot_044.png)

## 045 — Correcting SSH syntax

My first remote-login command was written incorrectly, and PowerShell rejected it. I corrected the SSH command before trying again.

![Screenshot 045: Correcting SSH syntax](../screenshots/archive/screenshot_045.png)

## 046 — Connecting over SSH

The corrected SSH command connected me to Ubuntu from Windows. I could now manage the Linux machine from a Windows command window instead of opening its virtual-machine screen each time.

![Screenshot 046: Connecting over SSH](../screenshots/archive/screenshot_046.png)

## 047 — Adding the lab network connection

I checked Ubuntu's two network connections. One connected to Hyper-V's Default Switch for internet access. The other connected to the internal switch so Ubuntu could communicate with the Windows lab machines.

![Screenshot 047: Adding the lab network connection](../screenshots/archive/screenshot_047.png)

## 048 — Checking both Linux interfaces

I checked the addresses on both Ubuntu connections. The internet connection already had an automatically assigned address, but the lab connection still needed its fixed address.

![Screenshot 048: Checking both Linux interfaces](../screenshots/archive/screenshot_048.png)

## 049 — Reading the current network settings

I read Ubuntu's existing Netplan file before editing the network settings. Netplan is the configuration system Ubuntu uses here to describe its network connections. Checking the existing file helped me avoid changing the working internet connection by mistake.

![Screenshot 049: Reading the current network settings](../screenshots/archive/screenshot_049.png)

## 050 — Giving Ubuntu a fixed lab address

I created a separate network-settings file for Ubuntu's lab connection and gave it a fixed address. I restricted access to the file and checked that Netplan could process it. The actual address is covered in the screenshot.

![Screenshot 050: Giving Ubuntu a fixed lab address](../screenshots/archive/screenshot_050.png)

## 051 — Verifying the Linux routes

After correcting a small command typo, I checked the resulting network settings. The lab connection had its fixed address, and internet traffic still used the other connection. This was the arrangement I wanted.

![Screenshot 051: Verifying the Linux routes](../screenshots/archive/screenshot_051.png)

## 052 — Checking the address again after login

I checked again in a later session. The automatically assigned internet address had changed, but the fixed lab address remained the same. This helped confirm that the two connections were doing their separate jobs.

![Screenshot 052: Checking the address again after login](../screenshots/archive/screenshot_052.png)

## 053 — Testing a website connection

I asked ubuntu.com for a web response and received a successful status, HTTP 200. This gave me a direct check that Ubuntu could reach that website. I cropped the shared image to the relevant terminal output.

![Screenshot 053: Testing a website connection](../screenshots/archive/screenshot_053.png)
