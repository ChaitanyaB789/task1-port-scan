# Task 1 — Local Network Port Scan

**Author:** Chaitanya B  
**Date:** 2025-09-22

## Objective
Discover open ports on devices in my local network and assess exposure. Only hosts I own or have permission to scan were targeted.

## Scope
- Primary network observed on system: `10.40.6.102/21` → network `10.40.0.0/21` (DO NOT scan without permission).
- Safe/test networks used (VMs): `192.168.174.0/24` (VMnet1), `192.168.86.0/24` (VMnet8).
- Local host: `127.0.0.1` and `10.40.6.102` (only scan if you own the host).

## Tools
- Nmap (version: fill-your-version)
- Wireshark / tshark (optional)
- Windows PowerShell (Administrator)

## Commands executed (examples)
# Discovery (ping-sweep, host discovery)
nmap -sn 192.168.86.0/24 -oN scans/discovery-vmnet8.txt

# Top 1000 TCP ports (SYN scan if Npcap + Admin; otherwise -sT)
nmap -sS --top-ports 1000 -T4 -v -oN scans/top1000-vmnet8.txt 192.168.86.0/24
nmap -sT --top-ports 1000 -T4 -v -oN scans/top1000-vmnet8-connect.txt 192.168.86.0/24

# All TCP ports on a single host (slow)
nmap -sS -p- -T3 -v -oN scans/allports-192.168.86.10.txt 192.168.86.10

# Service / version detection on a single host
nmap -sV -p 22,80,443 192.168.86.10 -oN scans/svc-192.168.86.10.txt

# UDP top ports (slow)
nmap -sU --top-ports 200 -T3 -v -oN scans/udp-vmnet8.txt 192.168.86.0/24

# Save XML as well
nmap -sS --top-ports 1000 -oN scans/result.txt -oX scans/result.xml 192.168.86.0/24

## Files included
- `scans/` — nmap outputs (txt, xml)
- `pcaps/` — optional Wireshark captures
- `screenshots/` — Wireshark / terminal screenshots
- `report.md` — analysis + remediation

## Notes & Legal
- Do not scan networks you don't own or have explicit permission to scan (e.g., corporate or university networks).
- For this task I used VM networks and local host scans to avoid unauthorized scans.
