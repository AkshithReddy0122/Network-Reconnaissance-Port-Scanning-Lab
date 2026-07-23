# Network-Reconnaissance-Port-Scanning-Lab
This repository documents an educational laboratory exercise focused on discovering active hosts and open TCP ports within a isolated local network (LAN). The primary goal is to gain hands-on experience with network service discovery, attack surfaces, analyzing network traffic with Wireshark, and identifying security risks.
# Objectives

•	Host & Service Discovery: Locate active devices on a local subnet using TCP SYN (Stealth) scans.

•	Exposure Analysis: Identify listening services, default ports, and potential risk factors.

•	Traffic Analysis: Observe network protocol behaviours and packet structures during port scanning via Wireshark.

•	Defensive Hardening: Formulate actionable security recommendations to reduce network exposure.

# Tools & Technologies

•	Nmap

     Role: Network Scanner
  
     Purpose: Host discovery, port scanning, and service identification.

 
•	Wireshark

     Role: Packet Analyzer
 
     Purpose: Capturing and analyzing network traffic generated during scanning.

 
•	Command Line / Shell

     Role: Execution Environment
 
     Purpose: Running Nmap commands and exporting stdout logs. 

# Execution Guide & Methodology

•	Step 1: Subnet Identification

   Identify the network interface and internal IPv4 subnet:

   On Windows, open terminal and run “ipconfig”

   My target IP Address: “192.168.XX.X”



•	Step 2: Wireshark Packet Capture Setup

   1. Open “Wireshark” and select the active network interface (e.g., eth0 or wlan0).
   2. Apply a display filter to isolate scan traffic:
      “ip.addr == 192.168.XX.X and tcp.flags.syn and tcp.flags.ack” (Paste your target IP address)
   3. Start capture

While running "nmap 192.168.XX.X"

<img width="1917" height="1020" alt="image" src="https://github.com/user-attachments/assets/b54a6d93-3f16-4d65-9deb-98def5513e24" />




While running "sudo nmap -sS 192.168.XX.X" 

<img width="1917" height="1010" alt="image" src="https://github.com/user-attachments/assets/95302238-2d3b-4a1c-85d1-fcc9cce13240" />





 
 •	Step 3: TCP SYN Scan Execution
 
    Execute a TCP SYN stealth scan (`-sS`) across the host range.
    
    Note: Root/Administrator privileges are required to forge raw TCP SYN packets.

 <img width="1917" height="1015" alt="image" src="https://github.com/user-attachments/assets/7eaddda8-b28f-4d1d-8f4d-618b5a7de38a" />


 
    
    Execute TCP SYN scan with standard output saved to text
    "sudo nmap -sS -v 192.168.XX.X -oN scan_results.txt" (Paste your target IP address)
    

 <img width="1916" height="1020" alt="image" src="https://github.com/user-attachments/assets/2fc7c798-4494-47ec-bb96-b39726b36565" />


    
 


# Results & Service Analysis

Below is the summary of findings from the local network scan:

<img width="1917" height="1030" alt="image" src="https://github.com/user-attachments/assets/a8d13661-7f37-4f37-ae82-187aeeaf33f7" />

 

# Defensive Hardening & Risk Mitigation
To minimize exposure identified during port scanning:
1. Disable Unnecessary Services:
   - Terminate unused network daemons and disable unwanted/unnecessary background services.
2. Implement Firewall Policies:
   - Apply host-based firewalls (e.g., 'ufw', 'iptables', 'Windows Firewall') to enforce a default-deny policy for incoming traffic on non-essential ports.
3. Restrict Management Access:
   - Ensure administrative interfaces (SSH, Web Configs) are restricted to trusted IP segments or accessible only via a secure VPN.
4. Enforce Strong Authentication & Encryption:
   - Replace unencrypted protocols (HTTP/Telnet) with secure equivalents (HTTPS/SSH).
   - Use public key authentication for SSH and enforce multi-factor authentication (MFA).
# Disclaimer
This laboratory exercise was conducted strictly inside an authorized, private home/lab environment for educational purposes. Scanning networks without explicit permission from the network owner is illegal and violates computer misuse regulations. Always ensure proper authorization before performing any network assessment tasks.
