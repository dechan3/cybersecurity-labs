# Cisco NetAcad Lab — Investigating Three Threat Landscapes

## Lab Overview
This lab explores how different threat landscapes expose vulnerabilities that can be exploited by threat actors. Using Cisco Packet Tracer, three common scenarios were simulated:
1. Network configuration vulnerability
2. Phishing and ransomware attack
3. Rogue access point with DNS hijacking

The objective was to understand how misconfigurations and user behavior can lead to real-world security incidents.

---

## Part 1 — Network Configuration Vulnerability (Guest Network Abuse)

### Scenario
An open guest Wi-Fi network was accessible from outside a home network. The guest network was improperly configured and allowed access to internal devices.

### Steps Observed
- A smartphone connected to an **open guest network** with no authentication.
- A network scan (Nmap) revealed active devices on the internal network.
- A webcam was discovered at `192.168.100.101`.
- The device was reachable via ICMP (`ping`), confirming network access.

### Investigation
- The home router was accessed using its default gateway: `192.168.100.1`.
- Default credentials (`admin / admin`) were used to log in.
- Three wireless radios were active:
  - Two secured **HomeNet** SSIDs
  - One **Guest** SSID with **no security**
- The Guest network:
  - Was active
  - Had no authentication
  - Allowed access to local network devices

### Vulnerability Identified
- Use of **default credentials**
- **Unsecured guest network**
- No network segmentation between guest and internal devices

### Security Takeaway
Guest networks should:
- Be secured with authentication
- Have SSID broadcast disabled if unnecessary
- Be isolated from internal devices
Failing to do so allows attackers to pivot from guest access to internal assets.

---

## Part 2 — Phishing Malware & Ransomware Attack

### Scenario
A phishing attack was simulated to demonstrate how social engineering targets users rather than systems.

### Attack Simulation
- A phishing email was crafted to impersonate a legitimate security team.
- The email instructed users to visit `pix.example.com` to reset credentials.
- The email was sent to multiple users inside a branch office network.

### Exploitation
- A user copied and pasted the malicious URL into their browser.
- The system displayed a ransomware message:
  - Hard drive encrypted
  - $600 ransom demanded

### Impact
- Files on the victim system were encrypted.
- In a real-world scenario, such malware could spread by virus automatically via contact lists.
- Multiple endpoints could be compromised rapidly.

### Security Takeaway
- Humans are often the weakest link in security.
- Phishing awareness training is critical.
- Email filtering, firewalls, IPS, and malicious domain blocklists reduce risk.

---

## Part 3 — Rogue Access Point & DNS Hijacking

### Scenario
A threat actor deployed a **rogue Wi-Fi access point** in a public café to intercept traffic and redirect users to malicious websites.

### Attack Setup
- Multiple open networks named **“Cafe WiFi Fast”** were broadcast with a visible higher and faster signal.
- A café customer connected to one of the open networks.
- The rogue access point acted as:
  - Wireless access point
  - DHCP server
  - DNS server

### Exploitation
- The user visited `friends.example.com`, a legitimate-looking site.
- Traffic was redirected to a malicious server.
- The same ransomware message from Part 2 appeared.

### Investigation Findings
- DNS server assigned via DHCP was `192.168.10.199`
- That IP belonged to the attacker’s laptop.
- DNS records were modified to resolve:
  - `friends.example.com` → `10.6.0.250` (malicious server)

### Root Cause
- DHCP distributed a **malicious DNS server**
- DNS hijacking redirected legitimate traffic
- Ransomware was installed silently

### Security Takeaway
- Open Wi-Fi networks are inherently risky
- Rogue access points enable man-in-the-middle attacks
- VPN usage and DNS security are critical defenses

---

## Key Lessons Learned
- Misconfigurations are as dangerous as software vulnerabilities
- Default credentials are a major security risk
- User awareness is essential in preventing phishing attacks
- Rogue networks combined with DNS manipulation can fully compromise users
- Defense requires both technical controls and educated users

---

## Disclaimer
This lab was conducted in a **simulated environment** using Cisco Packet Tracer for educational purposes only.
