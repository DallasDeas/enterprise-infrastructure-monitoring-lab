# Building the Ubuntu monitoring server

[Walkthrough](../README.md) · [Full archive](../archive.md)

I provisioned Ubuntu as the final monitoring server and separated its internal lab connection from its internet-facing interface. I validated installation media, remote access, static addressing, and outbound connectivity before deploying Zabbix. These checks established the operating-system and network prerequisites for monitoring.

## 027 — Preparing the Ubuntu VM

I inspected a Generation 2 VM with two virtual processors and 4 GB of startup memory for the Linux monitoring server.

![Screenshot 027: Preparing the Ubuntu VM](../screenshots/archive/screenshot_027.png)

## 028 — Catching setup mistakes

The VM had a 60 GB dynamic disk and a Default Switch connection, but the output also showed Windows installation media and a malformed combined command. I checked the actual VM settings before proceeding.

![Screenshot 028: Catching setup mistakes](../screenshots/archive/screenshot_028.png)

## 029 — Checking mounted installation media

The DVD inventory showed both Windows and Ubuntu 26.04.1 ISO paths. This helped me verify the Ubuntu installation media was present.

![Screenshot 029: Checking mounted installation media](../screenshots/archive/screenshot_029.png)

## 030 — Checking Secure Boot

I confirmed Secure Boot was on with the Microsoft UEFI Certificate Authority template for the Linux guest.

![Screenshot 030: Checking Secure Boot](../screenshots/archive/screenshot_030.png)

## 031 — Starting Ubuntu networking

The Ubuntu installer detected eth0 and obtained an IPv4 address through DHCP. I used this connection during the initial installation.

![Screenshot 031: Starting Ubuntu networking](../screenshots/archive/screenshot_031.png)

## 032 — Leaving the installer proxy blank

The proxy field was empty in this setup screen. I did not document a separate proxy service for the lab.

![Screenshot 032: Leaving the installer proxy blank](../screenshots/archive/screenshot_032.png)

## 033 — Choosing Ubuntu storage

I used the 60 GB virtual disk with LVM selected. The installer screen shows disk encryption was not selected.

![Screenshot 033: Choosing Ubuntu storage](../screenshots/archive/screenshot_033.png)

## 034 — Reviewing the partition layout

I reviewed the EFI, /boot, root filesystem, and remaining LVM space before continuing. The root logical volume was about 28.5 GB, rather than the full disk capacity.

![Screenshot 034: Reviewing the partition layout](../screenshots/archive/screenshot_034.png)

## 035 — Reaching profile setup

I captured the Ubuntu profile screen before entering account details. No account credentials are included in this repository.

![Screenshot 035: Reaching profile setup](../screenshots/archive/screenshot_035.png)

## 036 — Skipping Ubuntu Pro enrollment

I selected the option to continue without Ubuntu Pro enrollment for this lab build.

![Screenshot 036: Skipping Ubuntu Pro enrollment](../screenshots/archive/screenshot_036.png)

## 037 — Reviewing SSH installation options

The SSH selection screen is an intermediate capture with the install checkbox clear. I use the later install log and service check to establish that OpenSSH was actually installed.

![Screenshot 037: Reviewing SSH installation options](../screenshots/archive/screenshot_037.png)

## 038 — Watching installation finish

The installer log shows OpenSSH server installation and the remaining configuration steps. This is stronger evidence of installation than the earlier option screen.

![Screenshot 038: Watching installation finish](../screenshots/archive/screenshot_038.png)

## 039 — Removing installation media

At the end of setup, Ubuntu prompted me to remove the installation medium and press Enter.

![Screenshot 039: Removing installation media](../screenshots/archive/screenshot_039.png)

## 040 — Checking VM hardware after installation

I inspected the VM's memory, processors, disk, network adapter, and empty DVD drives. The settings also showed a checkpoint existed; I did not treat that as a tested backup.

![Screenshot 040: Checking VM hardware after installation](../screenshots/archive/screenshot_040.png)

## 041 — Logging into Ubuntu

I reached Ubuntu 26.04.1 LTS on the installed guest. Account details, network identifiers, and SSH key information have been removed from the screenshot.

![Screenshot 041: Logging into Ubuntu](../screenshots/archive/screenshot_041.png)

## 042 — Checking the SSH listener

I confirmed ssh.service was active and listening on port 22. The service was socket-triggered in this capture, so the disabled service label did not mean SSH was unavailable.

![Screenshot 042: Checking the SSH listener](../screenshots/archive/screenshot_042.png)

## 043 — Refreshing package information

I ran apt update and checked the available updates. This capture showed 40 packages could be upgraded.

![Screenshot 043: Refreshing package information](../screenshots/archive/screenshot_043.png)

## 044 — Completing package updates

I reviewed the package-update output and service restart notices. The output also identified sessions still using older binaries.

![Screenshot 044: Completing package updates](../screenshots/archive/screenshot_044.png)

## 045 — Correcting SSH syntax

My first SSH attempt used the wrong command syntax and PowerShell rejected it. I corrected the command before retrying the connection.

![Screenshot 045: Correcting SSH syntax](../screenshots/archive/screenshot_045.png)

## 046 — Connecting over SSH

The corrected SSH connection reached the Ubuntu login banner. This confirmed remote shell access from Windows to the Linux VM.

![Screenshot 046: Connecting over SSH](../screenshots/archive/screenshot_046.png)

## 047 — Adding the lab network connection

I inspected the Ubuntu VM's two virtual adapters: one on the Default Switch and one on the internal lab switch.

![Screenshot 047: Adding the lab network connection](../screenshots/archive/screenshot_047.png)

## 048 — Checking both Linux interfaces

I checked addresses and routes after adding the second adapter. The internet-facing interface had DHCP addressing; the lab interface did not yet have its static IPv4 address.

![Screenshot 048: Checking both Linux interfaces](../screenshots/archive/screenshot_048.png)

## 049 — Reading the existing Netplan configuration

I inspected the installer-generated Netplan file and current routes before adding a separate configuration for the lab interface.

![Screenshot 049: Reading the existing Netplan configuration](../screenshots/archive/screenshot_049.png)

## 050 — Writing the static lab configuration

I created a Netplan file for eth1, disabled DHCP on that interface, set its static /24 address, restricted file permissions, and ran netplan generate. The address itself is redacted.

![Screenshot 050: Writing the static lab configuration](../screenshots/archive/screenshot_050.png)

## 051 — Verifying the Linux routes

After a small ip-command typo, I checked the corrected address and route output. The lab interface had its static address and the default route remained on the internet-facing interface.

![Screenshot 051: Verifying the Linux routes](../screenshots/archive/screenshot_051.png)

## 052 — Checking the address again after login

In a later session, the DHCP address had changed while the lab address remained in place. This was useful confirmation that the two interfaces had different jobs.

![Screenshot 052: Checking the address again after login](../screenshots/archive/screenshot_052.png)

## 053 — Checking outbound HTTPS

I requested response headers from ubuntu.com and received HTTP/2 200. The published image is cropped to the terminal evidence and removes incidental session information.

![Screenshot 053: Checking outbound HTTPS](../screenshots/archive/screenshot_053.png)
