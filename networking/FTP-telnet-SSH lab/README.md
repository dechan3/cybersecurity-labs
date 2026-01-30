# FTP vs Telnet vs SSH — Traffic Analysis & Security Comparison

## CIA Triad

The **CIA Triad** represents the three core principles of cybersecurity:

### Confidentiality
Ensures that information is only accessible to authorized people or systems.  
**Goal:** prevent data leaks and unauthorized access.  
**Examples:** encryption, authentication, access control.

### Integrity
Ensures that data remains accurate, complete, and unaltered without authorization.  
**Goal:** prevent unauthorized modification or tampering.  
**Examples:** hashes, digital signatures, checksums, version control.

### Availability
Ensures that systems and data are accessible when needed.  
**Goal:** prevent downtime and service disruption.  
**Examples:** backups, redundancy, load balancing, DDoS protection.

This lab evaluates **FTP**, **Telnet**, and **SSH** through the lens of the CIA Triad, with a primary focus on **confidentiality** during network communication.

---

## Overview
This lab analyzes three remote communication protocols  **FTP**, **Telnet**, and **SSH**  from a cybersecurity perspective.  
The objective is to observe how each protocol behaves at the network level and assess their security by inspecting captured traffic.

## Objectives
- Identify the OSI layer and transport protocol used by FTP, Telnet, and SSH
- Observe TCP connection establishment (3-way handshake)
- Analyze authentication mechanisms
- Determine whether credentials and session data are exposed
- Compare the protocols based on confidentiality and overall security

## Tools & Environment
- Linux and windows 7 virtual machines
- FTP, Telnet, and SSH clients
- **Wireshark** (network traffic analysis) and PuTTy for SSH

---

## Part 1: FTP Analysis

### Protocol Information
- **OSI Layer:** Application (Layer 7)
- **Transport Protocol:** TCP
- **Server Port:** 21 (control channel)
- **Client Port:** Ephemeral (changes every connection)

### TCP Connection Establishment
- SYN  
- SYN-ACK  
- ACK  

### Authentication Messages
- `USER`
- `PASS`

### Observations
- Credentials are transmitted **in plaintext**
- Username and password are visible directly in captured packets
- No encryption is used at any stage of the authentication process

### CIA Triad Impact
- **Confidentiality:** ❌ Violated (credentials exposed)
- **Integrity:** ❌ Not protected
- **Availability:** ✅ Not directly affected

### Security Implications
FTP is vulnerable to:
- Credential sniffing
- Man-in-the-middle (MITM) attacks
- Unauthorized access through intercepted credentials

---

## Part 2: Telnet Analysis

### Protocol Information
- **Transport Protocol:** TCP
- **Server Port:** 23
- **Client Port:** Ephemeral

### TCP Connection Establishment
- SYN  
- SYN-ACK  
- ACK  

### Observations
- Entire session is transmitted **in plaintext**
- Credentials, commands, and server responses are visible
- Data can be recovered by following the TCP stream

### CIA Triad Impact
- **Confidentiality:** ❌ Violated
- **Integrity:** ❌ No protection against tampering
- **Availability:** ✅ Not directly affected

### Security Implications
Telnet provides no protection against:
- Session hijacking
- Credential theft
- Command interception

---

## Part 3: SSH Analysis

### Protocol Information
- **Transport Protocol:** TCP
- **Server Port:** 22
- **Client Port:** Ephemeral

### TCP Connection Establishment
- SYN  
- SYN-ACK  
- ACK  

### SSH Session Flow
1. Key Exchange Initialization  
   - Client → Server  
   - Server → Client  
2. Key Exchange Reply  
3. New Encryption Keys Established  
4. Authentication Phase  
5. Encrypted Commands & Responses  

### Observations
- All packets are encrypted after key exchange
- Credentials are never transmitted in plaintext
- Captured traffic is unreadable without decryption keys

### CIA Triad Impact
- **Confidentiality:** ✅ Preserved
- **Integrity:** ✅ Ensured via cryptographic mechanisms
- **Availability:** ✅ Supported (depends on infrastructure)

### Security Implications
SSH protects against:
- Passive network sniffing
- Credential exposure
- Unauthorized session access

---

## Comparison Summary

| Protocol | Encryption | Credentials Exposed | CIA Compliance |
|--------|------------|---------------------|----------------|
| FTP    | ❌ No       | ✅ Yes              | ❌ Weak        |
| Telnet | ❌ No       | ✅ Yes              | ❌ Weak        |
| SSH    | ✅ Yes      | ❌ No               | ✅ Strong      |

---

## Conclusion
FTP and Telnet fail to preserve **confidentiality**, as credentials and session data are transmitted in plaintext.  
SSH, through encryption and secure key exchange, successfully protects authentication data and communication channels.

**SSH is the only protocol among the three that aligns with the CIA Triad and is suitable for secure environments.**

---

## Defensive Takeaways
- Disable FTP and Telnet on modern systems
- Enforce SSH for remote administration
- Monitor networks for legacy plaintext protocols

