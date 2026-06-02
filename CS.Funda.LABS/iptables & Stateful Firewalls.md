# **Cybersecurity Lab 06: Introduction to iptables & Stateful Firewalls**

**Authors:** AJANA Chadi 
**Focus:** Host Security, Port Protection, Stateful Rules, and Web Server Hardening.

## **1\. Lab Objectives**

The goal of this lab was to implement network security policies using Linux iptables on an infrastructure server. This involved configuring a stateful host-based firewall to allow only necessary services (SSH and HTTP) while blocking unauthorized or malicious traffic.

## **2\. Infrastructure Topology**

The lab environment consists of two virtual machines connected to a shared internal NAT network layout:

* **Server Infrastructure Node (Ubuntu 16.04.7 LTS):** Implements iptables and hosts an active Apache2 web service (172.16.147.130).  
* **Auditing/Client Node (Kali Linux):** Acts as the external user and penetration testing agent (172.16.147.128).

  \[ Kali Linux Audit Host \] (172.16.147.128)  
             |  
             |  (Internal NAT Subnet)  
             v  
  \[ Ubuntu Production Server \] (172.16.147.130 \- Apache2 Web Host)

## **3\. Apache2 Web Server Directory Structure**

Before applying firewall rules, the layout of the Apache2 web platform on Ubuntu was reviewed:  
/etc/apache2/  
  ├── apache2.conf       (Master Configuration File)  
  ├── ports.conf          (Defines Listening Port Bindings, e.g., Port 80\)  
  ├── mods-enabled/       (Active Modules: .load and .conf)  
  ├── conf-enabled/       (Active Global Configuration Snippets)  
  └── sites-enabled/      (Active Virtual Host Domain Profiles)

### **Configuration Components:**

* **apache2.conf:** The primary configuration file. It links all sub-modules, site profiles, and environment variables during service startup.  
* **ports.conf:** Determines the ports where Apache listens for incoming connections (such as Port 80 for HTTP or Port 443 for HTTPS).  
* **mods-enabled/ and sites-enabled/:** Define the active software modules and virtual host directories that manage routing.

## **4\. Phase-by-Phase Firewall Implementation**

### **Phase 1: Establishing the Default Drop Policy (Zero-Trust)**

By default, standard iptables configurations allow all traffic. To transition the system to a Zero-Trust posture, the default operational rules for incoming packets were modified to reject traffic unless explicitly permitted.  
\# Set default policies to drop traffic on INPUT and FORWARD chains  
iptables \-P INPUT DROP  
iptables \-P FORWARD DROP  
iptables \-P OUTPUT ACCEPT

*At this point, all inbound connections to the server are dropped, temporarily severing active connections.*

### **Phase 2: Restoring Loopback Communications**

To ensure internal system processes can communicate with local resources, traffic on the loopback interface must be allowed:  
iptables \-A INPUT \-i lo \-j ACCEPT

* **Flag Analysis:** \-A INPUT appends the rule to the inbound chain, \-i lo targets the loopback interface, and \-j ACCEPT allows the traffic.

### **Phase 3: Allowing Public Web Traffic (HTTP)**

To allow public web traffic to reach the hosted platform, inbound requests on port 80 must be permitted:  
iptables \-A INPUT \-p tcp \--dport 80 \-j ACCEPT

* **Flag Analysis:** \-p tcp filters for TCP traffic, and \--dport 80 targets the default port for HTTP.

### **Phase 4: Restricting Secure Shell Access (SSH)**

To protect administrative access, secure shell entry was locked down so that only the dedicated management console can establish a connection:  
iptables \-A INPUT \-p tcp \-s 172.16.147.128 \--dport 22 \-j ACCEPT

* **Flag Analysis:** \-s 172.16.147.128 applies a source restriction matching only the Kali administrative IP, and \--dport 22 targets the default listening port for SSH.

## **5\. Firewall Verification & Testing**

After applying the rule set, auditing assessments were performed from the Kali Linux client machine to confirm the firewall's behavior:

### **1\. ICMP Echo Validation (Ping Test)**

ping 172.16.147.130

* **Expected Result:** Packets time out.  
* **Technical Reason:** The firewall drops inbound ICMP packets because there is no explicit rule allowing them.

### **2\. HTTP Port Verification (Port 80\)**

Navigating to http://172.16.147.130 successfully loads the Apache default index page, confirming port 80 traffic is allowed.

### **3\. SSH Service Verification (Port 22\)**

ssh ubuntu@172.16.147.130

* **Expected Result:** The connection succeeds, prompting for credentials and granting access.  
* **Security Validation:** If the client's IP changes from 172.16.147.128, the firewall drops the packets, protecting the interface from unauthorized remote connection attempts.

## **6\. Active Rules Summary Table**

The resulting active configuration on the Ubuntu server can be viewed with iptables \-L \-v:

| Chain | Target | Protocol | Source | Destination | Options / Port |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **INPUT** | ACCEPT | all | anywhere | anywhere | limit to loopback (lo) |
| **INPUT** | ACCEPT | tcp | anywhere | anywhere | destination port 80 |
| **INPUT** | ACCEPT | tcp | 172.16.147.128 | anywhere | destination port 22 |
| **INPUT** | DROP | all | anywhere | anywhere | default fallback policy |
