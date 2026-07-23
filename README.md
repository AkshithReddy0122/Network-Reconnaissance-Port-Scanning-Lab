# Network-Reconnaissance-Port-Scanning-Lab
This repository documents an educational laboratory exercise focused on discovering active hosts and open TCP ports within a isolated local network (LAN). The primary goal is to gain hands-on experience with network service discovery, attack surfaces, analyzing network traffic with Wireshark, and identifying security risks.
# Objectives
Host & Service Discovery: Locate active devices on a local subnet using TCP SYN (Stealth) scans.
•	Exposure Analysis: Identify listening services, default ports, and potential risk factors.
•	Traffic Analysis: Observe network protocol behaviours and packet structures during port scanning via Wireshark.
•	Defensive Hardening: Formulate actionable security recommendations to reduce network exposure.

# Tools & Technologies
Nmap 
 Role: Network Scanner
 Purpose: Host discovery, port scanning, and service identification.
Wireshark
 Role: Packet Analyzer
 Purpose: Capturing and analyzing network traffic generated during scanning.
Command Line / Shell
 Role: Execution Environment 
 Purpose: Running Nmap commands and exporting stdout logs. 

# Execution Guide & Methodology
Step 1: Subnet Identification
Identify the network interface and internal IPv4 subnet:
Windows: “ipconfig”
IP Address: “192.168.56.1”
Step 2: Wireshark Packet Capture Setup
1. Open “Wireshark” and select the active network interface (e.g., eth0 or wlan0).
2. Apply a display filter to isolate scan traffic:
   “ip.addr == 192.168.56.1 and tcp.flags.syn and tcp.flags.ack”
3. Start capture
 
 
Step 3: TCP SYN Scan Execution
Execute a TCP SYN stealth scan (`-sS`) across the host range. Note: Root/Administrator privileges are required to forge raw TCP SYN packets.
 
Execute TCP SYN scan with standard output saved to text
sudo nmap -sS -v 192.168.1.0/24 -oN scan_results.txt
 
# Results & Service Analysis
Below is the summary of findings from the local network scan:
 

# Defensive Hardening & Risk Mitigation
To minimize exposure identified during port scanning:
1. Disable Unnecessary Services:
   - Terminate unused network daemons and disable unneeded background services.
2. Implement Firewall Policies:
   - Apply host-based firewalls (e.g., `ufw`, `iptables`, Windows Firewall) to enforce a default-deny policy for incoming traffic on non-essential ports.
3. Restrict Management Access:
   - Ensure administrative interfaces (SSH, Web Configs) are restricted to trusted IP segments or accessible only via a secure VPN.
4. Enforce Strong Authentication & Encryption:
   - Replace unencrypted protocols (HTTP/Telnet) with secure equivalents (HTTPS/SSH).
   - Use public key authentication for SSH and enforce multi-factor authentication (MFA).
# Disclaimer
This laboratory exercise was conducted strictly inside an authorized, private home/lab environment for educational purposes. Scanning networks without explicit permission from the network owner is illegal and violates computer misuse regulations. Always ensure proper authorization before performing any network assessment tasks.
