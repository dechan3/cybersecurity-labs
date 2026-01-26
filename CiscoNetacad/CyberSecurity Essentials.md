# **Threat domains**

which are areas of control, authority or protection that attackers can exploit to gain access to a system; hence why it is important for an organization to know the vulnerabilities in their threat domains.
Some examples of those vulnerabilities that attackers can exploit :

-Direct access to software or hardware content as well as networks
-Malicious email attachments 
-Less secure elements within a supply chain

**Raising awareness becomes essential between employees to avoid internal and external cyber threats as user domains pose a significant threat to the three essentials of cybersecurity (confidentiality, integrity and availability)**

# **Backdoors and rootkits**

-**Backdoors** such as netbus and back orifice are used to bypass the normal authentification procedures, typically through an authorized user who unknowingly runs a remote admin tool program (RAT) which installs a backdoor for the attacker;
Backdoors grant access to the attacker even if the original breach or vulnerability gets fixed later on

-**Rootkits** are malwares that modify the OS to create a backdoor by taking advantage of software vulnerabilities, which leads to unauthorized system resources making it very hard to detect and require, most of the time, a complete wipe off and reinstallation.

# **Social engineering** 
Less tactical and nerdy, yet very effective, plays on emotions and needs, social engineering exploits human nature, plays around greed or vanity.

- Pretexting : attacker establishes trust with the victim, so the attacker lies to gain access
- Quid pro quo (smth for smth) : Request for information in exchange for something
- Identity fraud : Pretty straight forward, the use of someone else's identity to obtain good and services such as creating a credit card in your name with your personal information.

# **Cyber attacks**
- Malwares (viruses, worms, trojan horses...)
- Logic bombs, "detonate" at a certain specific date or query, waits for a trigger implementing then a malicious code
- Ransomware : Holds a computer until a certain payment is done
- Denial of service (Dos) : sent of overwhelming quantity of packets or malicious formatted ones
- Distributed Dos (DDos) : similar but originate from multiple sources, the attacker creates a network (botnet) of infected hosts called zombies.
- Domain name system (DNS) : Translates a domain name (ex : www.google.com into IP -> 8.8.8.8) so if a DNS doesn't know the ip, it'll ask another DNS server, hence why organizations need to monitor their domain reputation to protect against external domains attacks such as **DNS spoofing** (redirects traffic somewhere else), **domain hijacking** (make changes to the target's DNS infos), **uniform ressource location (URL)** (redirecting to a malicious website instead of the official one) DNS attacks are usually used in phishing.
  
*Translation Layer: When a user types a URL into a browser, the browser queries a DNS server to translate the domain name portion of that URL into an IP address.
Accessing Resources: The URL specifies where to go (e.g., https://www.example.com/page), and DNS provides the necessary IP address to actually get there.*

- Layer2 (OSI) : data link, in the switch, IPs are mapped to devices (MAC) on the network using ARP which finds the MAC adress using the IP "who has IP? Tell me your MAC", attackers take advantage of vulnerabilities in this layer. Attacks like the following :

                **Spoofing** : impersonation attacks such as MAC spoofing, ARP spoofing and IP spoofing.
                One disguises their device in a network to seem like it's a valid one,
                the second links an attacker's MAC to the IP of an authorized device in the LAN,
                and the third sends IP packets from a spoofed source address in order to disguise it.

                **MAC flooding** : Devices on a network are connected via a network switch by using packet switching to receive and forward data to the destination device.
                An attacker floods the network with fake MAC addresses, compromising the security of the network switch.

- Zero day : exploits software vulnaribilites before they're even detected

*Organizations can take several steps to defend against various attacks. These include the following:*

  Configure firewalls to remove any packets from outside the network that have addresses indicating that they originated from inside the network. 
  Ensure patches and upgrades are current.
  Distribute the workload across server systems.
  Network devices use Internet Control Message Protocol (ICMP) packets to send error and control messages, such as whether or not a device can communicate with another on the network. To prevent DoS     and DDoS attacks, organizations can block external ICMP packets with their firewalls.

# **Wireless and mobile device attacks**
We're talking grayware which is an app that, altough may not be considered a very dangerous malware, it still poses risks to the user and lack of privacy like location, (because let's be honest, who wants and thinks a random app on their phone that does nothing has its place there, surely it comes with some kind of threats) and smishing, which is fake texts to lead you to click on unwanted links or call some numbers all to get you some malware or any other possible threats.

**Rogue access point attacks** are often combined with social engineering to gain physical or logical access to networking infrastructure, allowing an unauthorized access point to be connected to the network and used as a man-in-the-middle to capture credentials and traffic.
In parallel or separately, an evil twin access point may be deployed, impersonating a legitimate AP by using the same SSID and configuration. On networks without protected management frames, attackers can abuse unauthenticated deauthentication frames to force client devices to disconnect from the legitimate AP.
Once disconnected, client devices automatically scan for known networks and may reconnect to the attacker’s access point if it appears stronger or more available, enabling traffic interception.
This attack primarily affects older or misconfigured networks, as modern standards such as WPA3 and 802.11w protect management frames using cryptographic validation, causing forged frames to be ignored.

# **Attacks Against Wi-Fi Protocols**
WEP was an early wireless security protocol that encrypted Wi-Fi traffic for a security and protection that's said to be as good as wired networks but suffered from serious flaws, including poor key management and weak initialization vectors, making it easy to crack by capturing traffic.
WPA and WPA2 were introduced to fix these issues by using stronger encryption and dynamic key management. While WPA2 prevents attackers from recovering the encryption key from captured traffic, encrypted packets can still be intercepted and analyzed.

# **Application Attacks**
### **Buffer Overflow**
Buffers are memory areas allocated to certain applications, a buffer overflow overwhelms a memory buffer with too much data. On system terms, it's like a DDoS of memory, but they're very different. A buffer Overflow, you write more data than a memory buffer can hold, extra data spills into adjacent memory, which can lead to crashing the program, overwrite return addresses/variables, and most importantly lead to code execution, it's about memory corruption and control flow.
DDoS = flooding a door so no one else can enter
Buffer overflow = shoving extra stuff into a drawer until it breaks and messes up the whole cabinet
One blocks access.
The other breaks structure.

### **Defending against application attacks**
- Solid code
- Prudent programming (validation inputs from outside of a function)(regex)

      Real-world example
      Find an IP address:
      \d{1,3}(\.\d{1,3}){3}
      Check if text looks like an email:
      [A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}

      Why it matters (especially for you)

      Parsing logs
      Filtering network traffic
      Grep, sed, awk, SIEM tools
      CTFs & OverTheWire challenges
      Input validation & security checks

- keep all softwares up to date
