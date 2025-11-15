## Basic Port Scanning and Service Enumeration - Personal Project #2

## Overview
In this personal project I will be demonstrating some basic port scanning, enumeration, and reconnaissance in my homelab against a vulnerable target (metasploitbale3) from my attacker machine (kali). This aligns with my first project as it's still introductory and covering the following:

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
- nmap -A -sV 10.x.x.x (The IP address has been redacted for safety risks)


## Findings 
Open Ports:
- 21, 22, 23, 25, 53, 80, 111, 139, 445, 512, 513, 514, 1099,1524,2049,2121,3306,5432,5900,6000,6667,8009,8180
- Operating System, version, and service info
- Some MAC addresses, which I covered
- Traceroute info which stated that 1 hop was needed

## What an attacker should do after gathering the findings
- 
