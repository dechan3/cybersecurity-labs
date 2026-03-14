##  Objective

Demonstrate two common security attack techniques:

* **Password hash cracking** on Linux systems using dictionary attacks.
* **DNS spoofing** to redirect a victim machine to attacker-controlled resources.

These exercises highlight weaknesses in **password security and network trust mechanisms**.

---

#  Part 1: Linux User & Password Hash Cracking

## Objective

Extract Linux password hashes and attempt to crack them using a **dictionary attack** with **John the Ripper**.

---

## 1. Environment Setup & Tool Installation

The lab environment was prepared on a **Kali Linux system**.

### User Creation

Two additional users were created:

* **user1** → simple password
* **user2** → strong password

This allowed testing the effectiveness of password cracking against **weak vs strong passwords**.

### Downloading John the Ripper

The source code for **John the Ripper v1.8.0** was downloaded from Openwall using:

```
wget https://www.openwall.com/john/
```

### Extraction

The downloaded archive `john-1.8.0.tar.xz` was extracted using:

```
tar -xvf john-1.8.0.tar.xz
```

This provided access to the tool binaries and documentation.

---

## 2. Hash Extraction and Preparation

Linux stores authentication data in two separate files:

* `/etc/passwd` → contains usernames
* `/etc/shadow` → contains hashed passwords

To prepare the hashes for cracking, the **unshadow** utility was used to merge them into a single file:

```
unshadow /etc/passwd /etc/shadow > mypasswd
```

This created a file called **mypasswd**, which contained both usernames and their corresponding password hashes.

### Session Data

When John the Ripper runs, it automatically creates a hidden directory:

```
/root/.john
```

This directory stores:

* Cracking session data
* Progress checkpoints
* Recovered passwords

---

## 3. Cracking Results

John the Ripper was executed using **dictionary attack mode** against the `mypasswd` file.

### Results

| Account | Password Found  | Status        |
| ------- | --------------- | ------------- |
| kali    | kali            | ✅ Cracked     |
| user1   | user1           | ✅ Cracked     |
| user2   | Strong password | ❌ Not cracked |

### Terminal Observation

* The tool loaded **3 password hashes**
* Each hash used **different salts**
* Weak passwords were successfully cracked because they existed in the **default wordlist**

This demonstrates the **importance of strong passwords and complexity requirements**.

---

#  Part 2: DNS Spoofing Attack

## Objective

Perform a **DNS spoofing attack** to redirect a victim machine to attacker-controlled IP addresses.

---

## 1. Attack Execution

### Hosts Configuration

A custom mapping file was created using the `nano` editor:

```
nano hosts.txt
```

### Target Mapping

The following domains were mapped to the **attacker's IP address**:

```
example.com      192.168.17.131
testboom.com     192.168.17.131
google.com       192.168.17.131
```

### Tool Execution

The attack was launched using **dnsspoof** on the `eth0` interface:

```
dnsspoof -i eth0 -f hosts.txt
```

This allowed the attacker to send **forged DNS responses** before the legitimate DNS server could respond.

---

## 2. Findings & Security Analysis

### Successful Spoofing

The domains **example.com** and **testboom.com** were successfully redirected to the attacker's machine.

The terminal output confirmed that the attacker was **forging DNS A-record replies** for these queries.

### Failed Spoofing

Attempts to spoof **google.com** failed.

### Explanation

Modern web security mechanisms prevented successful impersonation:

* **HTTPS certificate validation**
* **HSTS (HTTP Strict Transport Security)**

Even though DNS responses were manipulated, the browser detected a **certificate mismatch**, preventing the user from accessing the fake website.

---

#  Security Implications

This lab highlights several important security principles:

**Password Security**

* Weak passwords are highly vulnerable to **dictionary attacks**.

**Network Trust Issues**

* DNS responses can be forged because traditional DNS lacks authentication.

**Modern Protections**

* HTTPS and HSTS provide critical protection against **DNS spoofing and Man-in-the-Middle attacks**.

---

#  Conclusion

This lab demonstrated two important security attack techniques:

* **Password hash cracking** using John the Ripper
* **DNS spoofing** using dnsspoof

The exercises emphasize the importance of:

* Using **strong, complex passwords**
* Implementing **secure DNS and encrypted protocols**
* Relying on **HTTPS-based protections** to prevent impersonation attacks.
