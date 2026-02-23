# Wireshark Packet Analysis

---
## 1.ICMP Traffic Capture

### 1.1.Action

* Wireshark capture filter `icmp`

* **ICMP request:** Pinging from kali linux `192.168.251.3` to metasploitable `192.168.251.5` 

### 1.2.Observation
This capture verifies successful bidirectional communication between the Kali Linux attacker (192.168.251.3) and the metasploitable target (192.168.251.5). The presence of matching ICMP Type 8 (Request) and Type 0 (Reply) packets confirms that the network path is clear and the target is reachable for further security testing.

### 1.3.Result

* **Success Rate:** 20 packets transmitted, 20 received, 0% packet loss, time 19443ms

![kali](./screenshots/kali1.png)

[**ICMP**](./pcap_files/icmp.pcapng)

---

## 2.TCP Three-Way Handshake 

### 2.1.Action

* Wireshark capture filter `tcp port http`

* Wireshark display filter `tcp.flags.syn == 1 && tcp.flags.ack == 1`, `tcp.flags.syn == 1 || tcp.flags.ack == 1`

* **3-way-handshake:** Visiting the DVWA in metasploitable `http://192.168.251.5:80` in firefox browser

### 2.2.Observation

* This capture demonstrates the fundamental SYN -> SYN ACK -> ACK sequence required to establish a reliable connection over TCP.

* **Packet 1 (SYN):** The Attacker sends a synchronization request to the target on Port 80.

* **Packet 2 (SYN-ACK):** The Target responds, acknowledging the request and sending its own synchronization signal.

* **Packet 3 (ACK):** The Attacker sends the final acknowledgment, moving the connection state to established.

### 2.3.Result

* The handshake completed in 3 packets with a total time of only a few milliseconds.

![kali](./screenshots/kali2.png)

[**TCP-3-way**](./pcap_files/tcp.pcapng)

---

## 3.TCP Three-Way Handshake Fail

### 3.1.Action

* Wireshark capture filter `tcp port 81`

* Wireshark display filter `tcp.flags.reset == 1`

* **3-way-handshake fail:** Making connection to the metasploitable telnet using closed port `telnet 192.168.251.5:81`

### 3.2.Observation

* **Connection Termination:** Only two packets exist in the entire conversation. The target sends a Reset (RST) flag immediately after your SYN, which kills the connection instantly without completing the three-way handshake.

* **Packet 1 (SYN):** The Attacker sends a synchronization request to the target on Port 81.
 
* **Packet 2 (RST, ACK):** The Target responds by acknowledging (ACK) request but simultaneously sending a Reset (RST) flag. This tells machine to "Stop" because the port is closed.

### 3.3.Result

* The connection was "Refused" because the port is closed.

* The RST-ACK is an "Active No." It tells the attacker (or user) that the machine is online, but the specific service they are looking for is not running.

![kali](./screenshots/kali3.png)

[**TCP-3-way-fail**](./pcap_files/tcpfail.pcapng)

---

## 4.Clear-Text Disclosure (Telnet)

### 4.1.Action

* Wireshark capture filter `tcp port 23`

* Wireshark tcp `follow` > `tcp stream`

* **Login:** Making a connection and entering the login credentials in the metasploitable telnet using command `telnet 192.168.251.5`
  
### 4.2.Observation

* This capture demonstrates the extreme risk of using unencrypted legacy protocols like Telnet.

* The protocol does not use TLS/SSL, the authentication process is sent in plain text. 

* **Individual Packets:** In Telnet packets, you will only see one letter at a time. This is because Telnet sends data character-by-character.

### 4.3.Result

* By using the "Follow TCP Stream" feature in Wireshark to reconstruct the entire session and view the username and password exactly as they were entered by the user.

* _**Solution:** Telnet is obsolete for security purposes and must be replaced by SSH (Secure Shell), which encrypts the entire stream._

![kali](./screenshots/kali4.png)

[**Telnet**](./pcap_files/telnet.pcapng)

---

## 5.The "Credential Thief" (HTTP Login)

### 5.1.Action

* Wireshark capture filter `tcp port http`

* Wireshark display filter `http.request.method == "POST"`

* **Request:** Entering the login credentials in the Damn Vulnerable Machine in firefox browser `http://telnet 192.168.251.5` to make a get/post request.

### 5.2.Observation

* **The POST Request:** Unlike a GET which asks for data, a POST sends data to the server.

* **The HTML Form:** In the "Packet Details" pane, Expanded the titled "HTML Form URL Encoded" to see the credentials.
  
* **Plain-Text Leak:** Variables like username: `admin` and password: `password` listed clearly.

### 5.3.Result

* The credentials were leaked exactly as typed, with no masking or encryption.

* HTTP (Port 80) provides zero security for data in transit. Attacker on the same network can easily harvest user credentials using a simple sniffer. 

* _**Solution:** Web applications must use HTTPS (TLS) to wrap this data in an encrypted tunnel._

![kali](./screenshots/kali5.png)

[**HTTP**](./pcap_files/http.pcapng)

---

## 6.Detecting a SYN Stealth Scan

### 6.1.Action

* Wireshark capture filter `tcp port 21 or tcp port 25 or tcp port 53 or tcp port 80`

* Wireshark display filter `tcp.flags.syn == 1 && tcp.flags.reset == 1`

* **Nmap:** Stealth scan on port FTP, SMTP, Domain, HTTP. `nmap -sS -p 21,25,53,80 192.168.251.5`
  
### 6.2.Observation

* **SYN & SYN-ACK:** In the display filter a SYN go out and a SYN-ACK come back (proving the port is open).

* **ACK & RST:** Instead of sending the final ACK to finish the connection, the Attacker (Kali) immediately sends a RST (Reset).

* By sending a RST instead of an ACK, the attacker never "fully" connects. On many older systems, this prevented the connection from being logged in the application logs, even though the attacker now knows the port is open.

### 6.3.Result

* The Wireshark "Info" column shows a repeating pattern of [SYN], [SYN, ACK], and then [RST].

* Normal user never sends a RST immediately after a SYN-ACK. Seeing this pattern across multiple ports (22, 80, 443) confirms that an automated scanning tool is mapping the network.

![kali](./screenshots/kali6.png)

[**Stealth**](./pcap_files/stealth.pcapng)

---

## 7.ICMP Flood (Denial of Service) Attack Detection

### 7.1.Action

* Wireshark capture filter `icmp`

* **Ping Flooding:** Sending many echo request to the 192.168.251.5 to flood the machine. `sudo hping3 --icmp --flood  192.168.251.5`
  
### 7.2.Observation

* This capture illustrates a Volumetric Denial of Service (DoS) attack using the ICMP protocol.

* Unlike a standard connectivity test, this "Ping Flood" shows a massive volume of Echo Requests sent at high frequency from the attacker (192.168.251.3) to the target (192.168.251.5).

* The extremely short time intervals between packets (visible in the "Time" column) demonstrate an attempt to consume the target's bandwidth and CPU resources.

### 7.3.Result

* **Graph:** The Wireshark I/O Graph (Statistics > I/O Graph) showed a vertical "spike" reaching higher packets in a fraction of a second, followed by an immediate drop to zero.

![kali](./screenshots/kali7.png)

[**DOS**](./pcap_files/dos.pcapng)

---

## 8.Detection of ARP Traffic

### 8.1.Action

* Wireshark capture filter `arp`

* **ipv4 forward**

* **arpspoof**

### 8.2.Observation

* This capture shows a successful Man-in-the-Middle attack. The attacker (kali linux) sent fake ARP messages to trick the network into thinking it was the Gateway (windows).

*  Wireshark detected this trick and flagged it with a "Duplicate IP address" warning. This proves that the attacker has successfully intercepted the network traffic.

### 8.3.Result

![kali](./screenshots/kali8.png)

[**ARP**](./pcap_files/arp.pcapng)

---
