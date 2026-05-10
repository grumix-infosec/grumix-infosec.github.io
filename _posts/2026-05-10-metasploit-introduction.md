---
title: Metasploit Introduction - TryHackMe Writeup
author: Grumix
date: 2026-05-10 14:10:00 +0300
categories: [Writeups, TryHackMe]
tags: [metasploit, windows, eternalblue, ms17-010]
render_with_liquid: false
media_subpath: assets/img/Metasploit_Introduction_TryHackMe_Writeup/metasploitintro.webp
---
Compromising Windows 7 via MS17-010 (EternalBlue) - A Metasploit Writeup for TryHackMe

Metasploit Exploitation was a room that focused on the fundamentals of the **Metasploit Framework** and legacy service exploitation. We started by performing an **Nmap** scan to identify an unpatched Windows 7 target, followed by vulnerability verification using an SMB scanner to confirm the presence of MS17-010. From there, we utilized the EternalBlue exploit to obtain an interactive Meterpreter session with NT AUTHORITY\SYSTEM privileges, which allowed us to enumerate the filesystem for flags and extract **NTLM** password hashes from the **SAM database**.

This writeup details the exploitation of a legacy Windows system, specifically focusing on the transition from initial reconnaissance to full system compromise using the Metasploit Framework.

## Initial Enumeration
Nmap Scan
We begin starting with **nmap** scan inside msfconsole: 
```
 msf > nmap -sS -sV -sC -p- 10.112.163.200 
[*] exec: nmap -sS -sV -sC -p- 10.112.163.200 

Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-10 13:18 +0300
Nmap scan report for 10.112.163.200
Host is up (0.042s latency).
Not shown: 65528 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Jon-PC
|   NetBIOS computer name: JON-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2026-05-10T05:20:21-05:00
| smb2-time: 
|   date: 2026-05-10T10:20:21
|_  start_date: 2026-05-10T10:18:13
|_clock-skew: mean: 1h40m00s, deviation: 2h53m12s, median: 0s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: JON-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:a0:d5:de:81:9b (unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 118.81 seconds
```
