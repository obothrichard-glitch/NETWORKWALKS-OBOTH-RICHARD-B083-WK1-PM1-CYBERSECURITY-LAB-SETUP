Cyber security Lab Environment Setup
Building an isolated virtual lab for penetration testing and ethical hacking practice

Prepared by: Oboth Richard
Date: 09th September, 2026
Module: W1-PM1 – Project Module 1
Instructor: Waqas Karim (NetworkWalks)

1. Introduction
The purpose of this lab was to build a working virtual environment for cybersecurity and ethical hacking practice. I followed the NetworkWalks lab guide to install VirtualBox, create a NAT Network, import Kali Linux, configure its IP settings, and confirm it had internet access and could communicate with other virtual machines on the same network.
This report documents the exact steps I took, the problems I ran into, and how I fixed them.

2. Hardware and Software Used
· Host OS: Windows 11
· RAM: 8 GB
· Storage: 256 GB SSD
· Processor: Intel Core i5
· Virtualization Software: Oracle VirtualBox (version 7.x)
· Guest OS: Kali Linux 2024.1 (VirtualBox image)
· Other Tools: 7-Zip

3.Step-by-Step Setup
3.1 Installing 7-Zip
I started by downloading 7-Zip from the official site (https://7-zip.org/download.html) and installed it with the default options. This was needed later to extract the Kali Linux VirtualBox image, which comes as a compressed file.



3.2 Installing VirtualBox
Next, I downloaded Oracle VirtualBox from https://virtualbox.org/wiki/Downloads and ran the installer. I kept all the default settings, including the network drivers, and restarted my PC after the installation finished.



3.3 Creating the NAT Network
This was one of the most important steps. I opened VirtualBox, went to File → Tools → Network Manager, and clicked the NAT Networks tab.
I created a new network with these settings:
· Name: NatNetwork
· IPv4 Prefix: 10.0.0.0/24
· Enable DHCP: Checked
I clicked Apply and confirmed the network appeared in the list.



3.4 Importing Kali Linux
I downloaded the Kali Linux VirtualBox image from https://kali.org/get-kali. After extracting it with 7-Zip, I opened VirtualBox and used Machine → Add to import the .vbox file.
Once imported, I opened the VM settings and changed the network adapter:
· Attached to: NAT Network
· Name: NatNetwork
· Adapter Type: Intel PRO/1000 MT Desktop
· Promiscuous Mode: Allow All
· Cable Connected: Checked



3.5 Configuring the Kali Linux IP Address
I started the Kali VM and logged in with the default credentials (kali / kali).
To set a static IP of 10.0.0.2, I used the NetworkManager command line:
```bash
sudo nmcli connection modify "Wired connection 1" ipv4.addresses 10.0.0.2/24
sudo nmcli connection modify "Wired connection 1" ipv4.gateway 10.0.0.1
sudo nmcli connection modify "Wired connection 1" ipv4.dns 8.8.8.8
sudo nmcli connection modify "Wired connection 1" ipv4.method manual
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```
After that, I verified the IP with:
```bash
ip a
```
The output showed inet 10.0.0.2/24 on the wired interface.



3.6 Fixing the “No Internet” Issue
At first, Kali had no internet access. I checked the NAT Network settings, confirmed no other VM was using 10.0.0.2, and then ran these commands as suggested in the lab guide for VirtualBox 7 and Kali 2024.1+:
```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```
After reconnecting, the internet worked immediately.

3.7 Taking a Snapshot
Once everything was working, I took a snapshot in VirtualBox:
· Machine → Take Snapshot
· Name: Kali-Clean-Install
· Description: Working Kali VM with NAT Network and static IP 10.0.0.2
This gives me a safe restore point if I break something later during practice.



4. Problems Encountered and Fixes
Problem Fix
Kali had no internet after import Ran the nmcli DAD-timeout commands from the lab guide
NAT Network not showing in VM settings Restarted VirtualBox and recreated the NAT Network
IP address not applying Used nmcli instead of the GUI, then restarted the connection

5. Conclusion
The lab environment is now fully working. Kali Linux boots with a static IP of 10.0.0.2, has internet access through the NAT Network, and can communicate with other VMs on the 10.0.0.0/24 subnet. I took a snapshot so I can return to this clean state at any time.
This setup gives me a safe, isolated lab for practicing penetration testing and ethical hacking without affecting my main Windows installation.

6. References
· NetworkWalks Lab Guide: W1-PM1 – Week1 – Project Module1 – Lab Setup Virtualbox and Kali Linux v4.2
· VirtualBox Downloads: https://virtualbox.org/wiki/Downloads
· Kali Linux Downloads: https://kali.org/get-kali
· 7-Zip Downloads: https://7-zip.org/download.html
