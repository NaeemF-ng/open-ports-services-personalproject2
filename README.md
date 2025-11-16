## Basic Port Scanning and Service Enumeration - Personal Project #2

## Overview
In this personal project I will be demonstrating some basic port scanning, enumeration, and reconnaissance in my homelab against a vulnerable target (metasploitbale3) from my attacker machine (kali). It also demonstrates my ability to collect information, interpret service banners, and analyze how exposed services can create an attack surface for a machine, while aligning with my first project as it's still introductory and covering the following:

- How to interpret open ports and exposed services
- How attackers use basic recon to plan initial access

This project aligns with MITRE ATT&CK Reconnaissance (TA0043) techniques because TA0043 is divided into 2 types of scanning active and passive, I actually completed a room on tryhackme regarding that. Active recon is when you scan the target for info and passive is getting info without interacting with the target such as google searching or any other public accessible data.

## Architecture
I have both kali and metasploitable3 virtual machines running on virtualbox and NAT Network called "Labnet". This setup ensures that both machines are away from the internet and can only communicate within my homelab, resulting in a safe pentesting environment. 




## Objective 
- Demostrate foundational pentesting skills
- Show that I understand recon, enumeration, and service discovery
- Interpret the results and show how they correlate to real world vulnerabilities
- To Build a portfolio of hands-on pentesting projects
- Keep the project focused strictly on basic discovery and enumeration to build a strong foundation.


## Tools used
- nmap
- metasploit
- Kali linux
- metasploitable3

## Scanning Methodology that I used
- nmap -A -sV 10.x.x.x    # Aggressive + Service/Version detection
- nmap -p- 10.x.x.x       # Full port scan
- nmap -sC -sV -O -p- 10.x.x.x   # Default scripts + OS Detection + All port scans (My go to scan), you can also combine the -sC and -sV commands into one like the following which is what I usually do: -sCV



- ## Findings 
Open Ports:
- 21, 22, 23, 25, 53, 80, 111, 139, 445, 512, 513, 514, 1099, 1524, 2049, 2121, 3306, 5432, 5900, 6000, 6667, 8009, 8180, 8787, 45683, 48530, 48884, 58451

- Operating System, version, and service info
- Some MAC addresses, which I covered
- Traceroute info which stated that 1 hop was needed
- The high number of exposed services highlights the need for proper mitigation and hardening in real-world systems. I did not perform any exploitation or attacks in this project, as it focuses strictly on discovery and enumeration.

Key Ports To Focus On:

Port 21- FTP
• File Transfer Protocol
• Older versions of FTP have exploits, which can the be used to gain access
• Attackers can gain access from anonymous login, enumerate users, and is very commonly used in CTF's

Port 22 - SSH 
• Secure remote login
• Attackers can banner grab
• From my personal experience doing CTF's, ssh is usually never the access point at first 9 times out of 10. You usually have to find credentials before you can connect, when I see for example 2 open ports being HTTP and SSH on a CTF, I know instantly the entry point is HTTP. 

Port 23 - Telnet 
• Is a unencrypted remote shell, was replaced by ssh because data was it was more secure 
• Credentials are sent in plaintext
• Still found on older systems and misconfigured devices

Port 80 - HTTP
• Used for server websites without encryption
• Often upgraded to port 443 (HTTPS) for security
• Credentials, cookies, and data can be intercepted or modified
• Common attack surface for web vulnerabilities (SQLi, LFI, XSS, etc)

Port 139 - SMB (NetBIOS Session Service)
• Legacy Windows file sharing over NETBIOS
• Allows access to shared folders, printers, and network resources
• Gather information from SMB enumeration

Port 445 - SMB over TCP 
• Modern SMB file sharing without NetBIOS 
• Used for file shares authentication, and windows domain communication
• It's a common target for attacks like EternalBlue, SMB relay, and credential harvesting

Port 3306 - MySQL
• A service for databases
• Used for storing and querying aplication data 
• It's a common attack surface for weak credentials, SQL injection, and misconfigurations

Port 5432 - PostgreSQL
• Advanced open-source relational database
• Supports encrypted conncetions 
• Used for application data and storage queries
• Risks include weak credentials, exposed services, and misconfigurations

Port 8009 - ajp13
• A protocol used by Apache to talk to Tomcat, a web application server
• Should always be firewalled when not needed, it is very dangerous when exposed
• It's vulnerable to the Ghostcat (CVE-2020-1938) exploit, which allows file reading or remote code execution.

Port 8080 - Apache Tomcat 
• A web application server that runs Java applications 
• Misconfigurations can expose manager panels or allow .WAR file uploads that lead to RCE (Remote code execution). I actually completed a challenge on TryHackMe that regarding this, i'll probably publish the writeup for it.
•  It usually has weak/default credentials

## The importance of knowing service versions 
• Knowing what version is running regarding a service can help direct you to an exploit, which can be used to gain access. For example:
Looking at my port scan we see that the FTP port is open and the version that is being used is 2.3.4 .If you search on metasploit (which is a tool used for so many things such as recon, exploitation, post exploitation, persistence, etc, but in this case for an exploit) for ftp version 2.3.4, there is a backdoor that allows command execution for that exact version.



