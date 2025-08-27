# VulnHub – DC-1 (Linux, Easy)

## Table of Contents
- [General Info](#general-info)
- [Objectives](#objectives)
- [Enumeration](#enumeration)
- [Exploitation](#exploitation)
- [Initial Shell](#initial-shell)
- [Privilege Escalation](#privilege-escalation)
- [Post-Exploitation Proof](#post-exploitation-proof)
- [Cleanup](#cleanup)
- [Key Takeaways](#key-takeaways)

## General Info
- **Machine name:** DC-1  
- **Platform:** VulnHub  
- **Author:** DCAU  
- **Release year:** 2015  
- **Difficulty:** Easy  
- **Type:** Boot2Root (goal: root access)  

## Objectives
- Gain initial foothold through the web application  
- Escalate privileges to root  
- Practice CMS exploitation (Drupal) and privilege escalation techniques  

## Enumeration
The target machine was identified in the NAT network.

**Network discovery**  
```bash
sudo netdiscover -r 192.168.25.0/24
```
Discovered host: 192.168.25.148

Nmap scan

```bash
sudo nmap -sC -sV 192.168.25.148
```
Open ports:

22/tcp – OpenSSH 6.0p1 Debian 4+deb7u7

80/tcp – Apache 2.2.22 (Debian), running Drupal 7

111/tcp – rpcbind 2-4```

Initial observation:
The Drupal CMS on port 80 appears to be the main attack surface. The robots.txt file reveals multiple standard Drupal directories and files (e.g., /CHANGELOG.txt, /install.php), which may expose the exact version.

![Nmap scan](./images/01_nmap.png)

_The scan reveals SSH (22), HTTP (Drupal, port 80), and RPCBind (111) as the main exposed services._

