# How the lab fits together

[Project overview](../README.md) · [Build steps](setup.md)

I ran the lab inside one physical computer using Hyper-V. Each virtual machine has its own operating system and settings, even though the machines share the physical computer's hardware.

![The lab computers and their network connections](images/lab-topology.png)

## What each part does

| Part | Its job in my lab | Where I checked it |
|---|---|---|
| Hyper-V host | The physical computer that runs the virtual machines | Screenshots 003–013 |
| Windows Server | Provides the lab's directory services and DNS; also runs the monitoring agent | 005, 086, 091–099, 108 |
| Windows 11 workstation | A client computer I used for lab network checks | 006–007 |
| Ubuntu server | Runs Zabbix, its database, its website, and the time service | 027–082, 110–116 |
| Internal lab switch | A software network connection between the lab machines and the host | 003–012, 047–052 |
| Default Switch | The connection Ubuntu used for internet access | 028, 047–052 |

**Directory services** organize a Windows domain and its accounts. **DNS** translates names into addresses so computers can find one another. The screenshots begin with the original Windows server and workstation already present; they do not document their entire original setup.

## Why Ubuntu has two network connections

I used one connection to reach the internet for software downloads and external time checks. It received an address automatically. I used the second connection for the internal lab and gave it a fixed address, so the other lab machines could find it consistently.

Ubuntu used the internet-facing connection as its default route—the usual path to destinations outside its local networks. I checked the two connections separately instead of assuming one successful internet test proved the lab connection worked too.

## How information moves

| Connection | What happens | Technical detail |
|---|---|---|
| Zabbix to Windows | Zabbix asks the agent for health information | Passive agent checks, TCP port 10050 |
| Windows to Ubuntu | Windows requests accurate time from Chrony | NTP, UDP port 123 |
| Ubuntu to an external time source | Chrony checks its own clock against a reference | External NTP source |
| Zabbix through Gmail | The alert action sends problem and recovery emails | Gmail media type and trigger action |
| My Windows session to Ubuntu | I manage Linux through a remote command window | SSH, TCP port 22 |

A **port** is a numbered entry point for a network service. **TCP** and **UDP** are ways computers send data across a network. I kept the port numbers here so someone with networking experience can also follow the setup.

In the monitoring arrangement I demonstrated, Zabbix asks Windows for information. The installer also had settings for an alternative mode where the agent initiates checks, but the screenshots do not establish that mode working.

## How I reached the website through SSH

I also used an SSH tunnel to open the Zabbix website. My browser connected to `127.0.0.1:8080`, the local end of the tunnel on my computer. SSH carried that connection to the monitoring server's web service on port 80. The local address was not the Ubuntu machine's lab address.

On Ubuntu, Apache serves the website and PHP-FPM processes the code that builds its pages. MariaDB stores the application's information. These are separate programs, which is why I checked each one when the website failed.

## Captured system details

I recorded Ubuntu 26.04.1 LTS with two virtual processors, 4 GB of starting memory, and a 60 GB virtual disk. The installed stack showed Zabbix 7.4.16, MariaDB 11.8.6, and PHP 8.5.4. The Windows server was a Windows Server 2022 evaluation installation.

An earlier Windows virtual machine had four virtual processors, 4 GB of starting memory, and a 100 GB disk. I included that early build in the archive, but it was not the final Ubuntu monitoring system and does not demonstrate a completed PRTG installation.

I use role labels instead of real computer names or network addresses in this explanation. [Full screenshot archive](../evidence/archive.md).
