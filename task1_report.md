# Scan Report — Task 1
**Author:** Chaitanya B  
**Date:** 2025-09-22  
**Scope:** VM test networks `192.168.86.0/24`, `192.168.174.0/24` and local host `127.0.0.1` (only hosts owned by me)

## Methodology
1. Host discovery using Nmap `-sn` to find live hosts.  
2. Top-1000 TCP ports scan with `-sS` (when Npcap + Admin) or `-sT`.  
3. Targeted service/version detection (`-sV`) on interesting hosts.  
4. Optional: UDP scan for common UDP services and Wireshark packet capture for evidence.

## Commands run (representative)
- `nmap -sn 192.168.86.0/24 -oN scans/discovery-vmnet8.txt`
- `nmap -sS --top-ports 1000 -T4 -v -oN scans/top1000-vmnet8.txt 192.168.86.0/24`
- `nmap -sV -p 22,80,443 192.168.86.10 -oN scans/svc-192.168.86.10.txt`
- `nmap -sU --top-ports 200 -T3 -v -oN scans/udp-vmnet8.txt 192.168.86.0/24`

## Findings (example table)
| IP | Hostname | Ports (open) | Services | Risk Level | Recommendation |
|----|----------|--------------|----------|------------|----------------|
|192.168.86.10|vm-web|22,80|ssh, http|Medium|Use key-based SSH; update web server; enable firewall|
|192.168.86.15|vm-file|445|smb|High|Disable SMB if unused; restrict to admin VLAN; patch OS|
|127.0.0.1||22,631|ssh, ipp|Low|Ensure SSH keys; disable unnecessary services|

*(Populate this table from your actual `scans/*.txt` outputs.)*

## Risk assessment & recommendations
- Close unused services and stop listening daemons.  
- Apply vendor patches and updates.  
- Use host firewalls and restrict management ports to trusted subnets only.  
- Use key-based authentication for SSH; disable password auth.  
- Disable SMB or restrict to necessary hosts only.

## Raw outputs
See `/scans` directory for full Nmap outputs (text and XML). See `/pcaps` for packet captures.

## Conclusion
This assessment was performed on VM-hosted test networks and local host only to avoid scanning production/corporate networks. Next steps: patch high-risk services and re-scan to confirm.
