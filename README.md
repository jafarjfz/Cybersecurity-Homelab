# Cybersecurity-Homelab
## Overview

This is a project that describes how I created a personal cybersecurity homelab. This repo describes the hardware, software, and tools I used to build this homelab. The purpose of the homelab is to experiment and test tools in a controlled environment, learn new skills and expand my knowledge on a practical level. The "images" folder contains various screenshots I took during the project. 

For this project, I used the guide by Day Cyberwox (https://blog.cyberwoxacademy.com/post/building-a-cybersecurity-homelab). 

<p align="center">
  <img width="1280" height="720" alt="maxresdefault" src="https://github.com/user-attachments/assets/48c607cf-ca5a-4328-a17b-7bc169ed87f0" />
</p>

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
10. Conclusion

## Hardware Used

For this project I used my Asus Zenbook S14. This laptop is a compact powerhouse that is perfect for this kind of task. Here are the specs: 

- 1 TB of Gen4 SSD Storage Space.
- 32 GB of RAM.
- Intel Core Ultra 9 (Lunar Lake), with 8 Cores.
- Intel ARC Integrated Graphics 140.
- 3K OLED 120hz Display.

This laptop is definitely powerful enough to host multiple VMs comfortable. Therefore, I chose it as the host.

## Software Used

I picked the software that was mentioned in the guide. I will list it here:

- ### Hypervisor
  I decided to choose VMware Workstation Pro, because due to past experience I noted that it performed better than Virtual Box.    I also found the UI more enjoyable and optimised. 
- ### Attacker
  For the attacker VM, I obviously chose Kali Linux, since it comes with pre-installed tools for cybersecurity, and is the         standart Linux distro used by cybersecurity enthusiasts.
- ### Victims
  For the victim VM, I used the Windows 11 Evaluation Copy for the Enterprise-Grade Victims, and a standard Windows 11 Home for    the Personal Victim. Although the guide only used Windows 11 Evals, I added the Personal victim VM to simulate targeted          attacks on a personal laptop, since the eval operates as Corporate Desktop. 
- ### Intrusion Detection System and Monitoring
  According to the guide, for the IDS position I choose Security Onion since it is an open-source software that provides           comprehensive Log Management and Security Monitoring. To use the Security Onion UI through the web, I installed a separate       Ubuntu Desktop instance.
- ### Network Segmentation and Firewalls.
  As per the guide, I used pfSense, since it provides enterprise-grade Network Security, Traffic Control, and even capabilities    such as Ad-Blocking, VPN control and WAN hosting. To use the pfSense UI through the web, I connected it with the Kali Linux      machine.
- ### Windows Active Directory
  I used the active directory to simulate an professional setting with Admin rights, User groups and Permissions. For this, I      used Windows Server 2025, although the guide used Windows Server 2019. There were some differences in the UI but it wasn't       hard to set it up. As I mentioned earlier, I used Windows 11 Eval copies for the "Employee" desktops.
  
  > Note: I also added a separate VM using a Windows 11 Home ISO. This one was added to simulate attacks on a Home Network.
  
- ### Security Information and Event Manager
  For the SIEM, I used Splunk, just as in the guide. Splunk is a pretty powerful tool that detects security threats, minimizes     IT downtime, accelerates application troubleshooting, and drives business intelligence using the digital footprint! To use       its UI, I installed an Ubuntu Server VM and connected it to the Splunk VM.
- ### Ubuntu Server and Desktop
  This is more of secondary role, but in order to monitor and use the data from SecOnion and Splunk, I used 2 extra VMs. The       first one being Ubuntu Desktop, and the second one Ubuntu Server.

## Setting up Kali Linux and Windows

This was probably the easiest part of the project, To do this, I installed the latest Kali Linux release, and then created a VM in VMware. I added the ISO file and installed Kali Linux normally. Here's a picture of an nmap scan on the Kali Linux console: 

<p align="center">
  <img width="640" height="395" alt="nmap" src="https://github.com/user-attachments/assets/72882f67-2d94-42a9-96ed-0adb48b2da1f" />
</p>
After, I added a Windows 11 Home ISO on a separate VM. For now, I haven't set up pfSense so they are both on NAT. I decided to try to get into the Windows 11 VM through the Kali Linux. For this, I set up a generic username and password on the Windows VM and started working on the Kali Console. I scanned with nmap and saw port 445 - Server Message Block.

<p align="center">
<img width="136" height="185" alt="bruteforcing" src="https://github.com/user-attachments/assets/b8070af9-c7d7-428e-8c24-54a482331867" />
</p>

Here, I installed 2 lists with generic usernames and common passwords. Then, I used a simple script I found online to try each one as a Brute Force attack (Script Kiddie, I know!)

After some attempts, it caught on and guessed the login and the password correctly. Then, I used FreeRDP to perform a remote desktop control of the Windows 11 Desktop through Kali.

<p align="center">
<img width="686" height="445" alt="Screenshot 2026-07-10 160011" src="https://github.com/user-attachments/assets/f1c1e494-be6e-4e29-8687-a60229b54ae8" />
</p>

But obviously, this wasn't a serious pentest, just something to test the waters and see how the VMs perform and interact with each other. Now to the next part.

## Configuring pfSense

pfSense is the backbone of this project, since it's a fundamental tool when it comes to networking. Here's some bullet points on why I used pfSense:

- ### Open Source, Secure Platform
  Enterprise-grade firewall/routing features without licensing costs, making it ideal for a homelab budget.
- ### Network Segmentation
  Native VLAN support made implementing the added 7th segment straightforward. Enforced isolation between zones.
- ### Traffic Visibility
  Built-in firewall logging captures activity at the network perimeter, filtered by IP, Ports, and etc.
- ### Access control
  Rule-based policies let you define exactly which segments can communicate, keeping the attack simulation contained to its        intended path.

As you can see, it's quite useful. I used this tool to segment the network over a WAN, and put each VM in it's own VMnet. Then, I made sure the settings were correct so that pfSense could also function as a firewall. This is important since this tool filters incoming and outgoing traffic based on IP, Ports, and Protocols. 

I installed pfSense community edition from it's official website then set it up as VM. I added 5 new network adapters per the guide, and 1 extra for the Win11 Home VM. 

<p align="center">
<img width="359" height="224" alt="pfsense" src="https://github.com/user-attachments/assets/ff9bbdf3-65f3-4351-bae1-7189a15d0e12" />
</p>

This is what the VM looked like after the installation and setup was complete. Then, I set up the adapters and then used an IP address to use it on the Kali machine, to access the WebGUI. From the WebGUI, I added the Firewall Rules, and set up the interfaces for the machines.

<p align="center">
<img width="1012" height="361" alt="image" src="https://github.com/user-attachments/assets/8c671c41-8c1f-4aec-91f0-39e7084fea34" />
</p>

Next, Security Onion.

## Configuring Security Onion

Security Onion is also a vital part of this homelab. This tool provides security monitoring, threat hunting and log management. Here's also some reasons why I used it:

- ### Free, Open-Source NSM Platform
  Combines multiple security tools (Zeek, Suricata, Elasticsearch) into one integrated distro, without any licensing costs.
- ### Network-Level Detection
  Built specifically to monitor traffic at the packet level, catching threats that host-based tools alone might miss.
- ### Full Packet Capture
  Allows going back and inspecting raw traffic after an alert fires, not just a flagged summary.
- ### Complements Splunk
  While Splunk focuses on host and log-level data, Security Onion watches the network layer, giving two vantage points on the      same activity.

To set up this tool, I installed the latest distro and then put the ISO in a new VM. After installing it, I set the username and password, after which the programme rebooted and finished the installation. I created an admin account, selected an IP for internet access and finished the configuration. 

<p align="center">
<img width="391" height="266" alt="Seconion" src="https://github.com/user-attachments/assets/7f50bfd0-bf46-4071-bff0-fbfc6b8908d0" />
</p>

Still, to use it's web interface I set up another Ubuntu Desktop. This simulates a SOC/Security Analyst accessing a SIEM or any other tool from their device. I used the IP of the Ubuntu and allowed it via the SecOnion console. After, I accessed the web page via IP and finished the WebGUI configuration.

<p align="center">
<img width="1458" height="919" alt="image" src="https://github.com/user-attachments/assets/2087a842-7894-48b8-ab8d-3f4bfdab90cc" />
</p>

## Configuring Windows Server as Domain Controller

The guide used the 2019 release of the version, but I decided to use the 2025 version since it's up to modern standards. This is an important part of the project, since this server simulates a real corporate environment. Here are the reasons for this pick: 

- ### Centralized User Management
  Active Directory allows managing users, groups, and permissions from a single point, instead of configuring each machine individually.
- ### Industry-Standard Directory Service
  Widely used in real corporate environments, making this directly applicable to actual IT and security roles.
- ### Group Policy Control
  Lets you enforce security settings, restrictions, and configurations across all domain-joined machines at once.
- ### Simulates a Realistic Attack Surface

In addition to setting up the Server as the Active Directory, I also made it added it as a CA (Certificate Authority). I installed the server as usual and then promoted it to Domain Controller. After, I added the role of Certificate Authority in the "Active Directory Certificate Services".

<p align="center">
<img width="1489" height="868" alt="image" src="https://github.com/user-attachments/assets/339945f3-6bcd-40cc-b687-f78e1019bebf" />
</p>

Here's what the Server UI looked like after I set it up. Now, I had to add the users to the Directory. Before setting up the eval, I created the user profile in the Windows Server Directory.
The guide used a Windows 10 Eval, but since that eval is outdated I used Windows 11 Eval. I installed the copy and set it up on a VM. I configured the OS and used the name for the user I created in the server. Then, I joined the domain through the Windows 11 Eval.

<p align="center">
<img width="822" height="583" alt="image" src="https://github.com/user-attachments/assets/3314e58e-f7de-4772-bcfb-1584839c87f3" />
</p>

This was the last part of the Active Directory. 
 > Note: The guide used 2 users, but since I added a separate victim instance, I decided that one was enough!

## Configuring Splunk on an Ubuntu Server
For this part of the project, I installed a separate Ubuntu Server to use it for Splunk. Splunk is one of the most widely used SIEMs in the Cybersecurity industry. Splunk essentially aggregates logs and datasets from various data sources and correlates all that information for easy searching, parsing & indexing. Reasons for choosing Splunk:

- ### Powerful Log Aggregation and Search
  Collects and indexes logs from across the network, letting you search and correlate data quickly during an investigation.
- ### Industry-Standard SIEM
  Widely used in real SOC environments, making this directly applicable to actual security monitoring roles.
- ### Custom Dashboards and Alerts
  Lets you build visualizations and set up alerts based on specific patterns or thresholds in the data.
- ### Complements Security Onion
  While Security Onion watches the network layer, Splunk focuses on host and log-level data, giving two vantage points on the same activity.

  To set it up, I decided to install a GUI on the Ubuntu Server via tasksel. Then, I installed Splunk through the browser on the server.

  <p align="center">
  <img width="743" height="389" alt="splunk" src="https://github.com/user-attachments/assets/dca91a8c-25da-4163-91d2-e04db1fffb8a" />
  </p>
  Here's the WebGUI after finishing the installation and setup. 

## Conclusion

Now that this project is finally complete, I'll run some tests later and set it up in a different repo. This was a very fun (a bit difficult) experience. This homelab is a multi-VM enviroment simulating an enterprise network with victims, attackers, and tools for monitoring and segmentation. I tried to emulate it to be as real as possible and I think this will do quite well. My original goal was to set up a lab that simulates a real-world enviroment from a security standpoint and allows to do real analysis on the logs for security monitoring. But, the additions and the personal tweaks were also important for the project. I think the most important skills I learned from this project were related to network segmentation, firewall configuration, log analysis and security monitoring, and enterprise-level system administration. Thank you for attention.
