# MITM_ARPspoofing
🔐 Man-in-the-Middle (MITM) Attack Detection & Mitigation Lab
📌 Overview
This lab demonstrates how a Man-in-the-Middle (MITM) attack works at Layer 2 (ARP spoofing) and how it can be detected and mitigated using practical defensive techniques.

The project is built as a home lab using virtual machines and focuses on hands-on learning, not theory.

🎯 Objectives
Understand how ARP spoofing enables MITM attacks
Perform a MITM attack in a controlled lab environment
Detect MITM activity using ARP tables and Wireshark
Apply defensive measures to stop and prevent the attack
Learn why certain network modes (NAT) block ARP-based attacks

🧪 Lab Architecture
Role	OS	Purpose
Attacker	Kali Linux	Launch ARP spoofing attack
Victim	Ubuntu Linux	Target machine
Gateway	Host-only gateway (VirtualBox)	Traffic routing

Network Mode: VirtualBox Host-Only Adapter
Subnet: 192.168.56.0/24
Gateway: 192.168.56.1

🧰 Tools & Technologies

Kali Linux
Ubuntu Linux
VirtualBox
Wireshark
arpspoof (dsniff)
Linux networking utilities (ip, arp, sysctl)

⚠️ Legal Disclaimer

This lab is strictly for educational purposes and must only be performed in an environment you own or have permission to test.
Unauthorized MITM attacks on real networks are illegal.

🔧 Environment Setup
1️⃣ Configure VirtualBox Networking

For both Kali and Ubuntu VMs:

Adapter 1:
- Attached to: Host-only Adapter
- Name: vboxnet0
- Promiscuous Mode: Allow All
- Cable Connected: ✔

2️⃣ Verify IP Addresses
Kali
ip a
Example:
192.168.56.103

Ubuntu
ip a
Example:
192.168.56.102

Gateway
192.168.56.1

🔴 Attack Phase: ARP Spoofing (MITM)
Step 1: Enable IP Forwarding (Kali)
sudo sysctl -w net.ipv4.ip_forward=1

Step 2: Launch ARP Spoofing (Kali)

Terminal 1 – Poison Victim
sudo arpspoof -i eth0 -t 192.168.56.102 192.168.56.1

Terminal 2 – Poison Gateway
sudo arpspoof -i eth0 -t 192.168.56.1 192.168.56.102

🔍 Detection Phase (Ubuntu)
1️⃣ ARP Table Inspection
arp -a
Expected Result During Attack
192.168.56.1 at <KALI_MAC_ADDRESS>

➡️ Gateway IP resolving to attacker MAC confirms MITM.

2️⃣ Wireshark Analysis

Interface: enp0s3
Display filter:
arp

Indicators of MITM
Repeated unsolicited ARP replies
Gateway IP mapped to Kali MAC
High frequency of ARP packets

🛡️ Mitigation & Defense
1️⃣ Stop the Attack (Kali)
Ctrl + C
sudo sysctl -w net.ipv4.ip_forward=0
2️⃣ Clear Poisoned ARP Cache (Ubuntu)
sudo ip -s -s neigh flush all
3️⃣ Preventive Control: Static ARP Entry (Ubuntu)
sudo arp -s 192.168.56.1 <REAL_GATEWAY_MAC>
➡️ Prevents ARP poisoning for critical systems.

🔐 Additional Security Insight
HTTPS/TLS encryption protects data even if MITM is active
ARP spoofing relies on Layer-2 visibility
VirtualBox NAT mode blocks ARP spoofing, which is why Host-Only networking is required

📚 Key Learnings
How ARP poisoning enables MITM attacks
Why NAT networks prevent Layer-2 attacks
How to detect MITM using packet analysis
Practical mitigation techniques used in real environments
Difference between attack feasibility and data confidentiality (HTTP vs HTTPS)

🧠 Skills Demonstrated
Network Security
Traffic Analysis
MITM Detection
Linux Networking
Defensive Security Controls
Red Team & Blue Team fundamentals

🚀 Future Enhancements
DNS spoofing MITM
HTTPS downgrade testing
IDS/IPS detection (Snort / Suricata)
pfSense firewall with Dynamic ARP Inspection
VPN-based MITM prevention testing

📌 Author
Vinay Raj
Cybersecurity Home Lab Project
