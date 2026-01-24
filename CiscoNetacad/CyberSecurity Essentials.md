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
