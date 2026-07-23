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

 
# Open Ports and their Services

 • Port 135/tcp — msrpc Service:  
      
      Microsoft Remote Procedure Call Function:

      Used by Windows operating systems to allow programs to request services from programs sitting on another computer in a network.

 • Port 139/tcp — netbios-ssnService:  
      
      NetBIOS Session Service (NetBIOS-SSN)Function: 
 
      Used for connection-oriented communication over NetBIOS, primarily facilitating legacy file and printer sharing in Windows networks.

 • Port 445/tcp — microsoft-dsService:   
      
      Server Message Block (SMB) over TCPFunction: 
      
      Handles modern Windows file sharing, network printing, and domain authentication without needing a legacy NetBIOS layer.

 • Port 3306/tcp — mysqlService:  
      
      MySQL Database Service (MySQL)Function: 
      
      Connects client applications to the open-source MySQL relational database management system to execute queries and manage data.

 • Port 6646/tcp — unknownService:  
      
      Typically unassigned by IANA, but often registered by custom applications.Common Association: 
      
      Most commonly utilized by legacy McAfee Network Agent services for local asset and peer discovery via heartbeat packets.


# Potential Risks associated with Open Ports

 • Port 445/TCP — Microsoft-DS (SMB over TCP)
 
   Risk Vectors: 
   
   SMB is arguably the most targeted service in enterprise infrastructure. Outdated implementations or missing patches leave systems open to critical Remote Code Execution (RCE) exploits (such as EternalBlue/CVE-2017-0143).
   
      Impact: A remote, unauthenticated attacker can exploit cryptographic or buffer vulnerabilities to execute code with kernel-level SYSTEM privileges, paving the way for network-wide ransomware deployment.

 • Port 139/TCP — NetBIOS Session Service
 
   Risk Vectors:
 
   Often open alongside port 445, legacy NetBIOS communications are susceptible to data sniffing and Man-in-the-Middle (MitM) credential relay attacks (such as SMB Relay via tools like Responder).
   
      Impact: Attackers can intercept or spoof local authentication traffic, capture NetBIOS password hashes, and crack them offline or relay them to access adjacent target machines.

 • Port 3306/TCP — MySQL Database
 
   Risk Vectors: 
   
   Exposing database daemons directly to the network exposes them to brute-force password guessing, exploitation of unpatched daemon binaries, or direct data exfiltration.
   
      Impact: Successful compromise grants full visibility into persistent data stores, allowing attackers to download sensitive database records, drop malicious web shells via INTO OUTFILE commands, or wipe information for extortion.

 • Port 135/TCP — Microsoft RPC (MSRPC)
   
   Risk Vectors: 
   
   MSRPC serves as an information clearinghouse for Windows endpoint management. Attackers use it to enumerate operational structures like user accounts, shares, and system services via anonymous endpoints.
   
      Impact: Facilitates highly targeted internal reconnaissance and maps out the vector pathways required for lateral movement or domain privilege escalation.

 • Port 6646/TCP — Unknown (McAfee Network Agent)
 
   Risk Vectors: 
   
   Legacy versions of the McAfee Network Agent listening on this port have historical vulnerabilities related to Denial of Service (DoS) and memory corruption buffer overflows.
   
       Impact: Maliciously crafted input packets can crash the local management engine, forcing systems offline or interfering with endpoint security enforcement reporting.


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
