# Network Attacks and System Security Analysis

##  Project Overview
This lab documents a series of hands-on cybersecurity exercises focused on **packet sniffing, protocol analysis, and Denial of Service (DoS) simulations** using **Kali Linux**.

The objective was to observe how network traffic can be captured and analyzed, and to understand how different DoS attack techniques affect a target system.

---

#  Part 1: Traffic Sniffing and Protocol Analysis

## Objective
Capture and analyze live network traffic to identify potential security vulnerabilities in common protocols.

---

## 1. Packet Capture with tcpdump

The first phase involved capturing raw network traffic on the `eth0` interface using **tcpdump**.

### Capture Process
A command was executed to capture **500 packets**, but the capture was manually stopped after **71 packets**.

### Validation
The generated file:
`/tmp/macapture.pcap`

was verified as a **valid tcpdump capture file in little-endian format**.

### Reading Packets
Captured packets were reviewed using:
`tcpdump -r /tmp/macapture.pcap | more`

This allowed the packets to be analyzed line-by-line.

---

## 2. Advanced Filtering & Hex Inspection

To refine the analysis, several capture options and filters were applied.

### Snapshot Length
The **snapshot length** was set to **1500 bytes** to capture the maximum number of bytes from each Ethernet frame.

### Hexadecimal Inspection
The `-x` flag was used to display packet contents in **hexadecimal format** for deeper inspection of packet data.

### Targeted Filters

Different filters were used to isolate specific network traffic:

**HTTP Traffic**
`tcp port 80`

**ARP Traffic**
`ether proto 0x806`

**Combined FTP and HTTP Monitoring**
`tcp and (port 21 or port 80)`

These filters helped narrow down the captured data to specific protocols of interest.

---

## 3. Vulnerability Assessment via Wireshark

The captured `.pcap` files were imported into **Wireshark** for further analysis.

### Protocols of Interest
The analysis focused on identifying traffic from:

- **Telnet**
- **FTP**

### Security Finding
Both protocols transmit **credentials in plain text**, including:

- Usernames
- Passwords

Because the data is **not encrypted**, an attacker performing packet sniffing could easily capture sensitive information. This represents a **major security vulnerability**.

---

#  Part 2: Denial of Service (DoS) Simulations

## Objective
Simulate and compare different **Denial of Service (DoS)** attack techniques against a **Windows 7 target machine** to observe system impact.

---

## 1. Lab Environment Troubleshooting

### Initial Issue
During early testing, both **Kali Linux** and **Windows 7** virtual machines were configured using **NAT networking**.  
This caused the machines to share the same IP address space, preventing the attack from working correctly.

### Resolution
Both machines were switched to **Host-Only networking**, which allowed:

- Proper communication between the machines
- Isolation of the testing environment
- Successful execution of the attacks

---

## 2. SYN Flood Attack

### Execution
The attack was launched using:
`sudo hping3 --flood --syn -p 80 192.168.166.128`

### Mechanism
The SYN Flood attack exploits the **TCP three-way handshake**:

1. SYN  
2. SYN-ACK  
3. ACK  

The attacker repeatedly sends **SYN packets** but never completes the handshake.

### Impact
This results in:

- Thousands of **half-open connections**
- Exhaustion of server resources
- Inability for legitimate clients to establish connections

This attack specifically targets **services listening on a particular port**.

---

## 3. UDP Flood Attack

### Execution
The attack was launched using:
`sudo hping3 --flood --udp -p 53 192.168.166.128`

### Mechanism
Instead of exploiting connection handshakes, this attack sends a **large volume of UDP packets** to the target system.

### Impact
Because UDP is **connectionless**, the system must process every packet it receives, which can lead to:

- High CPU usage
- Increased memory consumption
- Network congestion

This attack targets the **entire network stack and bandwidth** rather than a specific service.

---

## 4. Observed Results

Both attacks successfully degraded the performance of the Windows 7 target system.

### System Impact

**CPU Usage**
- Reached **100% utilization**

**RAM Usage**
- Increased significantly as the system attempted to process incoming packets

**Packet Transmission**
- `11,025,511` packets were transmitted during testing

**Packet Loss**
- `100% packet loss` was observed from the attacker's perspective, indicating the target system was overwhelmed

### Overall Effect
The system became **slow and unresponsive**, demonstrating the effectiveness of flood-based DoS attacks against poorly protected systems.

---

#  Conclusion

This lab demonstrated how attackers can:

- Capture and analyze network traffic using **tcpdump** and **Wireshark**
- Identify insecure protocols that transmit sensitive data in **plain text**
- Disrupt system availability using **SYN Flood** and **UDP Flood** attacks

The exercises highlight the importance of:

- Using **encrypted protocols** instead of Telnet and FTP
- Implementing **network defenses** such as firewalls, rate limiting, and intrusion detection systems to mitigate DoS attacks.
