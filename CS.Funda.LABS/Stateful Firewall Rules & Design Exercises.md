# **Cybersecurity: Stateful Firewall Rules & Design Exercises**

**Authors:** AJANA Chadi   
**Focus:** Malware Classifications, Network Terminology, and Stateful Rules Configurations.

## **1\. Malware Categorization & Structural Profiles**

* **Worm:** A self-replicating, autonomous malware strain that spreads across network connections by exploiting system vulnerabilities. It operates independently without requiring human interaction or a host file to execute.  
* **Virus:** A malicious code snippet that cannot run independently. It relies on a user executing a compromised host file to activate, replicate, and infect other binaries on the system.  
* **Trojan Horse:** Malware disguised as legitimate, safe software. It relies on social engineering to trick users into running it, hiding its malicious payload until executed.  
* **The Role of Suspicious Packers:** Threat actors use executable packers to compress, encrypt, and obfuscate a malware binary's structure. This alters the file's binary signature, preventing signature-based antivirus solutions from identifying known malicious code.

## **2\. Cybersecurity Component Terminology Matrix**

| Element Identifier | Category | Core System Functionality Description |
| :---- | :---- | :---- |
| **SSL / TLS** | Secure Transport Protocol | Establishes encrypted end-to-end communication tunnels across untrusted network structures. |
| **WPA** | Wireless Security Standard | Protects over-the-air transmissions within 802.11 wireless local networks. |
| **Backdoor** | Exploitation Vector | Provides persistent remote access to an attacker, bypassing standard authentication checks. |
| **MAC (Message Authentication)** | Cryptographic Primitive | Validates data integrity and confirms message authenticity using a shared secret key. |
| **Wireshark** | Diagnostic/Auditing Tool | Intercepts, parses, and decodes frame configurations from active network streams. |
| **Diffie-Hellman** | Key Agreement Protocol | Allows two parties to securely establish a shared secret key over an unencrypted channel. |
| **AES** | Symmetric Block Cipher | Encrypts data blocks using a standardized, high-performance symmetric algorithm. |
| **SHA1** | Cryptographic Hash | Generates a fixed 160-bit digest to verify file integrity (now deprecated for high-security use cases due to collision vulnerabilities). |

## **3\. Stateless Packet Inspection & Firewall Policy Rules**

### **Scenario Context Evaluator**

A basic packet filter firewall processes an inbound network packet containing the following parameters:

* **Source Configuration:** IP \= 180.14.17.2, Port \= 53  
* **Destination Configuration:** IP \= 152.23.25.5, Port \= 80  
* **Transport Protocol:** TCP

### **Evaluation & Analysis:**

1. **Rule Match & Traversal:** The packet is evaluated against the firewall's rule table. Based on standard ingress configurations that permit traffic targeting destination port 80 (web services), this packet matches the allowed criteria and **passes through the firewall**.  
2. **The Malicious Content Variable:** *What happens if this incoming packet carries a known exploit payload or virus signature?*  
   * **Result:** The packet **still passes through the firewall unimpeded**.  
   * **Technical Justification:** A standard stateless or packet-filtering firewall operates at layers 3 and 4 of the OSI model. It inspects only headers (IP addresses, ports, and protocols) and does not inspect the application payload layer. Blocking embedded threats requires a Next-Generation Firewall (NGFW) or Intrusion Prevention System (IPS) capable of Deep Packet Inspection (DPI).

## **4\. Stateful Firewall Rule Design Challenge**

The firewall rule architecture below regulates and secures traffic flows between the public internet and an organization's perimeter DMZ services.  
       Internet  
          |  
  \[ Stateful Firewall \]  
    |      |        |  
    |      |        \+-- HTTPS Server \[122.10.20.4\]  
    |      \+----------- DNS Server   \[122.10.20.2\]  
    \+------------------ Email Server \[215.16.15.3\]

### **Strategic Administrative Rule Set Configuration Matrix:**

| Policy ID | Source IP Scope | Source Port | Destination IP | Destination Port | Protocol Profile | Action Policy |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Rule 1** | 122.10.20.4 | 443 | any | any | **TCP** | **ACCEPT** |
| **Rule 2** | any | any | 122.10.20.4 | 443 | **TCP** | **ACCEPT** |
| **Rule 3** | 122.10.20.2 | 53 | any | any | **UDP / TCP** | **ACCEPT** |
| **Rule 4** | any | any | 122.10.20.2 | 53 | **UDP / TCP** | **ACCEPT** |
| **Rule 5** | 215.16.15.3 | 25 | any | any | **TCP** | **ACCEPT** |
| **Rule 6** | any | any | 215.16.15.3 | 25 | **TCP** | **ACCEPT** |
| **Rule 7** | any | any | any | any | **any** | **DROP** |

### **Rule Set Design Logic:**

* **Rules 1 & 2:** Allow inbound HTTPS requests to the web server (122.10.20.4:443) and permit outbound responses.  
* **Rules 3 & 4:** Allow bidirectional DNS queries on port 53 via both UDP and TCP to support domain resolution for 122.10.20.2.  
* **Rules 5 & 6:** Allow inbound and outbound mail transport (SMTP) via port 25 for the corporate mail server (215.16.15.3).  
* **Rule 7 (Implicit Deny):** Drops all other traffic that does not explicitly match the allowed rules, adhering to least-privilege security principles.
