# Lab 01: Network Recon with Nmap

## Goal
Practice recon using Nmap.

## Setup
- **Host:** Macbook Pro MacOS, VirtualBox
- **Attacker:** Kali Linux
- **Target:** Metasploitable 2
- **Network:** Host-only adapter (192.168.56.0/24), isolated from home network


## What I did
1. Created a VirtualBox host-only network and set both VMs to use it, keeping 
   the lab isolated from my home network.
2. Confirmed connectivity between Kali and Metasploitable with `ping`.
3. Ran a host discovery scan to confirm the target was live: nmap -sn -oN scan-discovery.txt 192.168.56.0/24
4. Ran a service/version detection scan: nmap -sV -T4 -oN scan-sv.txt 192.168.56.2
5. Ran a full port scan across all 65,535 ports for complete coverage: nmap -p- -oN scan-allports.txt 192.168.56.2
6. Cross-referenced the top findings against known CVEs and `searchsploit`.

| Flag | What it does | Why I used it |
|------|---------------|----------------|
| `-sn` | Host discovery | Find live hosts on the subnet |
| `-sV` | Service/version detection | ID what's running on open ports |
| `-p-` | All 65,535 ports | Full coverage |
| `-T4` | Speed up scan on a local lab network |

## Findings
| Port | Service | Version | Notable CVE / Issue |
|------|---------|---------|----------------------|
| 21   | vsftpd | 2.3.4 | CVE-2011-2523 — backdoored version, grants root shell |
| 139/445 | Samba smbd | 3.X-4.X | CVE-2007-2447 — remote code execution via username map script |
| 6667 | UnrealIRCd | — | CVE-2010-2075 — backdoored version, grants root shell |
| 1524 | bindshell | "Metasploitable root shell" | Unauthenticated root shell, no CVE needed |
| 23   | telnet | Linux telnetd | No encryption — credentials sent in plaintext 

## Next steps
Lab 02: add Wazuh and detect these scans.
