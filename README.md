# Cybersecurity-Homelab
## Overview

This is a project that describes how I created a personal cybersecurity homelab. This repo describes the hardware, software, and tools I used to build this homelab. The purpose of the homelab is to experiment and test tools in a controlled enviroment, learn new skills and expand my knowledge on a practical level.  For this project, I used the guide by Day Cyberwox (https://blog.cyberwoxacademy.com/post/building-a-cybersecurity-homelab). 

> Note: Since the guide uses some software that is considered outdated, like Windows 10 or Windows Server 2019, I used more modern versions and also made some personal tweaks.

## Table of Contents

1. Hardware Used - The host laptop which I used for this project.
2. Software Used - All of the software I used, and their purpose.
3. Setting up Kali and Windows
4. Small Brute Force test (For fun)
5. Configuring pfSense for Segmentation and Firewalls
6. Configuring SecOnion for IDS and Security Monitoring
7. Configuring Windows Server as a Domain Controller
8. Configuring Enterprise Windows Desktops
9. Configuring Splunk as a SIEM

## Hardware Used

For this project I used my Asus Zenbook S14. This laptop is a compact powerhouse that is perfect for this kind of task. Here are the specs: 

- 1 TB of Gen4 SSD Storage Space.
- 32 GB of RAM.
- Intel Core Ultra 9 (Lunar Lake), with 8 Cores.
- Intel ARC Integrated Graphics 140.
- 3K OLED 120hz Display.

This laptop is definetely powerful enough to host multiple VMs comfortable. Therefore, I chose it as the host.

## Software Used

I picked the software that was mentioned in the guide. I will list it here:

- ### Hypervisor
  I decided to choose VMware Workstation Pro, because due to past experience I noted that it performed better than Virtual Box.    I also found the UI more enjoyable and optimised. 
- ### Attacker
  For the attacker VM, I obviously chose Kali Linux, since it comes with pre-installed tools for cybersecurity, and is the         standart Linux distro used by cybersecurity enthusiasts.
- ### Victims
  For the victim VM, I used the Windows 11 Evaluation Copy for the Enterprise-Grade Victims, and a standart Windows 11 Home for    the Personal Victim. Although the guide only used Windows 11 Evals, I added the Personal victim VM to simulate targeted          attacks on a personal laptop, since the eval operates as Corporate Desktop. 
- ### Intrusion Detection System and Monitoring
  According to the guide, for the IDS position I choose Security Onion since it is an open-source software that provides           comprehensive Log Management and Security Monitoring. To use the Security Onion UI through the web, I installed a separate       Ubuntu Desktop instance.
- ### Network Segmentation and Firewalls.
  As per the guide, I used pfSense, since it provides enterprise-grade Network Security, Traffic Control, and even capabilities    such as Ad-Blocking, VPN control and WAN hosting. To use the pfSense UI through the web, I connected it with the Kali Linux      machine.
- ### Windows Active Directory
  I used the active directory to simulate an professional setting with Admin rights, User groups and Permissions. For this, I      used Windows Server 2025, although the guide used Windows Server 2019. There were some differences in the UI but it wasn't       hard to set it up. As I mentioned earlier, I used Windows 11 Eval copies for the "Employee" desktops.
  
  > Note: I also added a separate VM using a Windows 11 Home ISO. This one was added to simulate attacks on a Home Network.
  
- ### Security Information and Event Manager
  For the SIEM, I used Splunk, just as in the guide. Splunk is a pretty powerful tool that detects security threats, minimizes     IT downtime, accelerates application troubleshooting, and drives business intelligence using the digital footprint! To use       it's UI, I installed an Ubuntu Server VM and connected it to the Splunk VM.
- ### Ubuntu Server and Desktop
  This is more of secondary role, but in order to monitor and use the data from SecOnion and Splunk, I used 2 extra VMs. The       first one being Ubuntu Desktop, and the second one Ubuntu Server.

## Setting up Kali Linux
