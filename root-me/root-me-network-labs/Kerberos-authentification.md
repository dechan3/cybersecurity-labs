# Kerberos Authentication & Offline Password Recovery (PCAP Analysis)

This write-up explains how **Kerberos authentication** works, how credentials are verified **without sending passwords**, and how **offline password recovery** can be performed when Kerberos pre-authentication traffic is captured.

>  No real credentials are used — all values are placeholders.

---

## 1. What is Kerberos?

Kerberos is a **network authentication protocol** designed to allow users and services to authenticate securely over an untrusted network.

### Key goals
- Passwords are **never sent** over the network  
- Authentication is based on **cryptographic proofs**  
- Access is controlled using **tickets**, not credentials  

Kerberos is widely used in **Active Directory (AD)** environments.

---

## 2. Core Kerberos Components

| Component | Meaning |
|---------|--------|
| Client | The user trying to authenticate |
| KDC | Key Distribution Center (authentication authority) |
| Realm | Kerberos security domain (usually an AD domain) |
| Ticket | Proof of authentication |
| Service | Resource the client wants to access |

---

## 3. The Realm

The **realm** identifies the Kerberos domain and tells the KDC which identity database to use.

Example:
  EXAMPLE.LOCAL
A full Kerberos identity:
  username@REALM


The same username in different realms represents **different identities**.

---

## 4. Kerberos Authentication Flow 

Kerberos authentication happens in **two main phases**:

1. **Authentication Service (AS)** – Prove who you are  
2. **Ticket Granting Service (TGS)** – Get access to services  

---

## 5. Authentication Service Exchange (AS)

### AS-REQ (Authentication Service Request)

The client requests authentication from the KDC.

If **pre-authentication** is enabled (default), the client must prove knowledge of the password:

- Take the current timestamp  
- Encrypt it using a key derived from the user’s password  
- Send it as **PA-ENC-TIMESTAMP**

This proves password knowledge **without revealing the password**.

---

### AS-REP (Authentication Service Reply)

If the KDC successfully decrypts the timestamp and verifies it is recent:

- Authentication is accepted  
- A **Ticket Granting Ticket (TGT)** is issued  

The TGT allows access to services without reusing the password.

---

## 6. Ticket Granting Service Exchange (TGS)

### TGS-REQ
- The client uses the TGT to request access to a specific service  
- The password is **no longer involved**

### TGS-REP
- The KDC returns a **service ticket**  
- Authorization is enforced at this stage

---

## 7. Pre-Authentication and PA-ENC-TIMESTAMP

Pre-authentication protects Kerberos from **online password guessing attacks**.

Instead of sending a password:
- The client encrypts a timestamp  
- Only someone who knows the password can generate valid ciphertext  
- The KDC decrypts it using stored credentials  

This encrypted timestamp is called:
  PA-ENC-TIMESTAMP

---

## 8. Encryption Type (etype)

Kerberos supports multiple encryption algorithms, identified by **etype values**.

Examples:
- `etype 23` → RC4  
- `etype 17` → AES128  
- `etype 18` → AES256  

The encryption type determines:
- Key derivation method  
- Encryption algorithm  
- Applicable cracking technique  

---

## 9. What an Attacker Can See

From a captured **PCAP**, an attacker may obtain:
- Username  
- Realm  
- Encryption type  
- Encrypted timestamp (ciphertext)  

They **cannot** see:
- Clear-text password  
- Decrypted timestamp  

---

## 10. Offline Password Recovery Concept

Kerberos encryption is **deterministic**:
> Same inputs → same output

This enables **offline attacks**:

1. Guess a password  
2. Derive the Kerberos key using:
   - Password  
   - Username  
   - Realm  
   - Encryption type  
3. Encrypt a timestamp  
4. Compare it with the captured ciphertext  

If it matches → **password is correct**

No interaction with the KDC is required.

---

## 11. Hashcat and Kerberos

**Hashcat** supports Kerberos pre-authentication cracking.

Example format:
  $krb5pa$<etype>$<username>$<realm>$<cipher>

Hashcat:
- Recreates Kerberos key derivation  
- Re-encrypts timestamps  
- Compares results with captured data  

This is **cryptographic verification**, not guessing.

---

## 12. Why Only One Password Works

Cryptography guarantees:
- Same inputs → same output  
- Different inputs → completely different output  

Only the **correct password** produces the exact encrypted timestamp seen in the capture.

---

## 13. Why This Attack Works

This attack is possible when:
- Pre-authentication traffic is captured  
- Passwords are weak or reused  
- Offline cracking is feasible  

It fails when:
- Strong passwords are enforced  
- Smart cards or MFA are used  
- Passwords are long and random  

---

## 14. Final Notes

Kerberos is secure **by design**, but its strength depends on:
- Password hygiene  
- Proper configuration  
- Monitoring authentication traffic  

Understanding Kerberos helps both attackers and defenders identify:
- Credential exposure risks  
- Misconfigurations  
- Real-world attack paths  

---

🛡️ *Strong passwords and modern authentication methods remain the best defense.*
