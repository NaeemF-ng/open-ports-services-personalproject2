## Basic Port Scanning and Service Enumeration - Personal Project #2

## Disclaimer
All testing was performed in my isolated homelab environment. No external systems were scanned.

## Overview
In this personal project I will be demonstrating some basic port scanning, enumeration, and reconnaissance in my homelab against a vulnerable target (Metasploitable3) from my attacker machine (Kali). It also demonstrates my ability to collect information, interpret service banners, and analyze how exposed services can create an attack surface for a machine, while aligning with my first project as it's still introductory and covering the following:

 How to interpret open ports and exposed services
 How attackers use basic recon to plan initial access

This project aligns with MITRE ATT&CK Reconnaissance (TA0043) techniques. TA0043 is divided into 2 types of scanning: 
**active** - Scanning the target for info
**passive** - Getting info without interacting with the target such as Google searching,facebook, or any publicly accessible data.

## Architecture

I have both Kali and Metasploitable3 virtual machines running on VirtualBox and a NAT Network called "Labnet". This setup ensures that both machines are away from the internet and can only communicate within my homelab, resulting in a safe pentesting environment. 
![Metasploitable3 VM Settings](/images/M3.png)
![Kali VM Settings ](/images/Kali.png)


## Objective 
• Demonstrate foundational pentesting skills
• Show that I understand recon, enumeration, and service discovery
• Interpret the results and show how they correlate to real world vulnerabilities
• To build a portfolio of hands-on pentesting projects
• Keep the project focused strictly on basic discovery and enumeration to build a strong foundation.

## Tools used
• Nmap
• Metasploit
• Kali Linux
• Metasploitable3

## Scanning Methodology that I used
• nmap -A -sV (Metasploitable3 ip)    # OS Detection + Service/Version detection + Default scripts + Traceroute
• nmap -p- (Metasploitable3 ip)       # Full port scan
![nmap -A -sV (Metasploitable3 ip)](/images/Anmap-scan-r1.png)
![nmap -A -sV (Metasploitable3 ip)](/images/Anmap-scan-2.png)
![nmap -A -sV (Metasploitable3 ip)](/images/Anmap-scan-3.png)
![nmap -A -sV (Metasploitable3 ip)](/images/Anmap-scan-4.png)
![nmap -A -sV (Metasploitable3 ip)](/images/Anmap-scan-5.png)
![nmap -A -sV (Metasploitable3 ip)](/images/Anmap-scan-6.png)
![nmap -p- (Metasploitable3 ip)](/images/full-port-scanS.png)
![nmap -p- (Metasploitable3 ip)](/images/full-port-scan2.png)


## The importance of knowing service versions
• Knowing what version is running regarding a service can help direct you to an exploit, which can be used to gain access. For example:
Looking at my port scan we see that the FTP port is open and the version that is being used is 2.3.4. If you search on Metasploit (which is a tool used for many things such as recon, exploitation, post exploitation, persistence, etc., but in this case for an exploit) for FTP version 2.3.4, there is a backdoor that allows command execution for that exact version.
![FTP exploit in Metasploit](/images/ftp-exploit-screenshot.png)


## Findings
Open Ports:
• 21, 22, 23, 25, 53, 80, 111, 139, 445, 512, 513, 514, 1099, 1524, 2049, 2121, 3306, 5432, 5900, 6000, 6667, 8009, 8180
• Ports above 10000
• Operating system, version, and service info
• Some MAC addresses, which I covered
• Traceroute info which stated that 1 hop was needed
• The high number of exposed services highlights the need for proper mitigation and hardening in real-world systems. I did not perform any exploitation or attacks in this project, as it focuses strictly on discovery and enumeration.


## Key Ports To Focus On:

Port 21 - FTP  
• File Transfer Protocol  
• Older versions of FTP have exploits, which can then be used to gain access  
• Attackers can gain access from anonymous login, enumerate users, and it is very commonly used in CTFs  

Port 22 - SSH  
• Secure remote login  
• Attackers can banner grab  
• From my personal experience doing CTFs, SSH is usually never the access point at first 9 times out of 10. You usually have to find credentials before you can connect. When I see, for example, two open ports being HTTP and SSH on a CTF, I know instantly the entry point is HTTP. 

Port 23 - Telnet  
• Is an unencrypted remote shell, was replaced by SSH because it was more secure  
• Credentials are sent in plaintext  
• Still found on older systems and misconfigured devices  

Port 80 - HTTP  
• Used for server websites without encryption  
• Often upgraded to port 443 (HTTPS) for security  
• Credentials, cookies, and data can be intercepted or modified  
• Common attack surface for web vulnerabilities (SQLi, LFI, XSS, etc.)

Port 139 - SMB (NetBIOS Session Service)  
• Legacy Windows file sharing over NetBIOS  
• Allows access to shared folders, printers, and network resources  
• Gather information from SMB enumeration  

Port 445 - SMB over TCP  
• Modern SMB file sharing without NetBIOS  
• Used for file share authentication and Windows domain communication  
• It's a common target for attacks like EternalBlue, SMB relay, and credential harvesting  

Port 3306 - MySQL  
• A service for databases  
• Used for storing and querying application data  
• It's a common attack surface for weak credentials, SQL injection, and misconfigurations  

Port 5432 - PostgreSQL  
• Advanced open-source relational database  
• Supports encrypted connections  
• Used for application data and storage queries  
• Risks include weak credentials, exposed services, and misconfigurations  

Port 8009 - AJP13  
• A protocol used by Apache to talk to Tomcat, a web application server  
• Should always be firewalled when not needed, it is very dangerous when exposed  
• It's vulnerable to the Ghostcat (CVE-2020-1938) exploit, which allows file reading or remote code execution.  

Port 8080 - Apache Tomcat  
• A web application server that runs Java applications  
• Misconfigurations can expose manager panels or allow .WAR file uploads that lead to RCE (Remote Code Execution). I actually completed a challenge on TryHackMe regarding this — I’ll probably publish the writeup for it.  
• It usually has weak/default credentials

## Lessons Learned 
• How service versions can directly map to vulnerabilities
• The importance of different argument usage within nmap
• Why enumeration and is the foundation of an attack
