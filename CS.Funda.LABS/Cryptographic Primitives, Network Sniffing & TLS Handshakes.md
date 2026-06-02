# **Cryptography Lab 08: Cryptographic Primitives, Network Sniffing & TLS Handshakes**

**Authors:** AJANA Chadi   
**Focus:** Message Authentication Codes, Digital Signatures, Wireshark Deep-Dives, and Eavesdropping Mitigation.

## **1\. Cryptographic Primitive Evaluation Matrix**

| Primitive Mechanism | Confidentiality | Authenticity | Non-Repudiation | Integrity Verification |
| :---- | :---- | :---- | :---- | :---- |
| **Message Authentication Code (MAC)** | ❌ No | Yes | ❌ No | Yes |
| **Digital Signature Assembly** | ❌ No | Yes | Yes | Yes |

### **Architectural Explanations:**

* **Message Authentication Code (MAC):** Uses a symmetric shared key to verify data integrity and authenticate the source. Because both parties share the exact same key configuration, it cannot provide non-repudiation; either party could theoretically forge the message transmission metadata.  
* **Digital Signature Assembly:** Uses an asymmetric private key to sign data. Because the private key is uniquely held by the signer, it provides non-repudiation, ensuring the sender cannot deny originating the transmission.

## **2\. Core Security Scenarios & Realities**

### **Scenario A: Standard Email Privacy**

* **Assertion:** *If Alice's personal mailbox account remains uncompromised, only Alice can read her transit emails.*  
* **Verdict:** **FALSE**. Standard email transport networks do not use end-to-end encryption by default. Unencrypted data can be intercepted by intermediate routers, network administrative tools, or service providers (such as Google backend services) along the delivery path.

### **Scenario B: Active Session TLS Encryption**

* **Assertion:** *Alice connects to her online banking terminal via SSL/TLS. An on-path attacker can see the domain endpoint destination she visits but cannot read her account passwords.*  
* **Verdict:** **TRUE**. TLS establishes an encrypted layer for transactional payloads, shielding sensitive fields like credentials. However, tracking properties like IP headers and the Server Name Indication (SNI) string can still expose the destination domain identity.

### **Scenario C: PGP vs. SSL/TLS Deployment Standards**

* **Selected Preference:** **SSL/TLS**  
* **Technical Justification:** SSL/TLS operates natively within modern web browsers without requiring manual key management, software plugins, or user-side cryptographic tracking, offering a frictionless user experience. PGP requires explicit key distribution and technical overhead, making it less practical for general public web traffic.

## **3\. LAN Interception Matrix: HTTP vs. HTTPS**

This evaluation assumes an on-path attacker (**Mallory**) occupies a position inside the localized switching domain (LAN) alongside target system **Alice**.

| Intercept Capabilities Evaluation | Unencrypted HTTP Target Node | Encrypted HTTPS Target Node |
| :---- | :---- | :---- |
| **Can Mallory read raw text payloads?** | **Yes**. Payloads are transmitted in cleartext. | **No**. Payload data is encrypted using symmetric session keys. |
| **Can Mallory alter data in transit?** | **Yes**. There are no built-in integrity verification mechanisms. | **No**. Built-in MAC or AEAD verification blocks data tampering. |
| **Can Mallory track explicit page URLs?** | **Yes**. Full URI path strings are visible in cleartext HTTP headers. | **Partially**. Hostnames are visible via SNI/IP mapping, but deep paths remain encrypted. |

## **4\. Deep-Dive Wireshark Analysis: TLS 1.2 Handshake (RSA Exchange)**

The handshake establishes secure communications using asymmetric algorithms for authentication and symmetric keys for subsequent data encryption.  
Client                                                                Server  
  |                                                                    |  
  | \------------\> \[1. Client Hello (Siphers, Random, SNI)\] \-----------\> |  
  |                                                                    |  
  | \<------------ \[2. Server Hello (Cipher Chosen, Random)\] \---------- |  
  | \<------------ \[3. Certificate Transmission\] \---------------------- |  
  |                                                                    |  
  | \------------\> \[4. Client Key Exchange (Pre-Master Secret)\] \------\> |  
  | \------------\> \[5. Change Cipher Spec & Finished\] \----------------\> |  
  |                                                                    |  
  | \<------------ \[6. Change Cipher Spec & Finished\] \----------------- |  
  |                                                                    |  
  v                                                                    v

### **Handshake Sequence:**

1. **Client Hello:** The client initiates the session by sending its maximum supported protocol version, a client-generated random string, supported cipher suites, and extensions (such as the Server Name Indication header).  
2. **Server Hello:** The server responds by selecting the highest mutually supported TLS version, a server-generated random string, and the specific cipher suite that will secure the connection.  
3. **Certificate:** The server sends its public key certificate chain to the client. This allows the client to verify the server's identity against a trusted Certificate Authority (CA).  
4. **Client Key Exchange:** The client generates a high-entropy Pre-Master Secret string, encrypts it using the server's public key (extracted from the certificate), and transmits it back. Only the server's corresponding private key can decrypt this value.  
5. **Key Derivation Stage:** Both endpoints combine the client random, server random, and Pre-Master Secret to derive identical symmetric session keys (such as the Master Secret).  
6. **Change Cipher Spec & Finished:** Both nodes signal that subsequent traffic will be encrypted using the newly derived session keys, concluding the structural handshake.
