# Scenario

This report will demonstrate the tactics and techniques used by an attacker to identify endpoints accessible via Remote Desktop Protocol (RDP) and subsequently obtain valid credentials through the use of brute-force tools. The username of the victim was obtained using OSINT techniques, and the target organization was found to have a weak password policy, allowing commonly used password lists to be leveraged effectively.

As part of this demonstration, **Network Level Authentication (NLA)** will be deliberately disabled, which represents a significant security misconfiguration. Without NLA, RDP connections can proceed to the full graphical login interface without verifying credentials at the network layer. This not only exposes the system to brute-force attempts but also increases the risk of exploitation through unauthenticated RDP vulnerabilities.

The combination of weak password policies, exposed RDP access, and the absence of NLA creates a high-risk scenario that attackers can easily exploit to gain unauthorized access to internal systems. This report aims to simulate such an attack chain to generate security events for detection validation and highlight the importance of RDP hardening measures.

---

# Tools Used

- **Hydra**  
- **Kali Linux**

---

# Attack Steps

1. **Identify vulnerable endpoints on a network**


Command used: `nmap -p 3389 192.168.1.113`

2. **Target the RDP endpoint with known username and password list**
Command used: `hydra -t 1 -V -f -l attackerlab -P password_lists rdp://192.168.1.113`

Output:
<img width="1727" height="348" alt="image" src="https://github.com/user-attachments/assets/51275625-df57-4905-b1d3-f224448c9ce6" />

# MITRE ATT&CK Framework Mapping
The brute force attack on RDP credentials demonstrated in this report aligns with specific techniques outlined in the MITRE ATT&CK framework, which is widely used for categorizing and understanding adversary tactics and techniques.

T1110 – Brute Force
Repeated guessing of passwords or credentials to gain unauthorized access. In this demonstration, brute force tools systematically attempted password combinations against the RDP service.

T1021.001 – Remote Services: Remote Desktop Protocol (RDP)
Use of RDP for lateral movement or initial access. The attack exploited exposed RDP endpoints with weak/guessable credentials.

# Summary & Recommendations
This demonstration highlights how a combination of exposed RDP services, weak passwords, and lack of NLA significantly increase an organization's attack surface. Attackers can leverage automated tools such as Hydra to quickly compromise systems, potentially gaining persistent internal access.

## Key Recommendations
Enable Network Level Authentication (NLA): Adds a pre-authentication layer that blocks unauthenticated connections and mitigates brute-force attacks.

Implement strong password policies: Enforce complex passwords and regular changes to reduce the effectiveness of password lists.

Limit RDP exposure: Restrict RDP access using firewalls or VPNs to prevent unauthorized external access.

Monitor logs and alerts: Ensure SIEM and intrusion detection systems detect repeated failed RDP login attempts.

