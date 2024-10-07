
# Analyze Network Layer Attack

## Objective
Analyze Network attack and document to present to management for further action and mitigation strategies

---

### Skills Learned
- Network traffic analysis and packet inspection via tcpdump
- Understanding three-way handshake and SYN flood attacks
- Detecting and mitigating Denial of Service (DoS) attacks

---

### Tools Used
- Packet Sniffer
  
## Scenario
You work as a security analyst for a travel agency that advertises sales and promotions on the company’s website. The employees of the company regularly access the company’s sales webpage to search for vacation packages their customers might like. 

One afternoon, you receive an automated alert from your monitoring system indicating a problem with the web server. You attempt to visit the company’s website, but you receive a connection timeout error message in your browser.

You use a packet sniffer to capture data packets in transit to and from the web server. You notice a large number of TCP SYN requests coming from an unfamiliar IP address. The web server appears to be overwhelmed by the volume of incoming traffic and is losing its ability to respond to the abnormally large number of SYN requests. You suspect the server is under attack by a malicious actor. 

You take the server offline temporarily so that the machine can recover and return to a normal operating status. You also configure the company’s firewall to block the IP address that was sending the abnormal number of SYN requests. You know that your IP blocking solution won’t last long, as an attacker can spoof other IP addresses to get around this block. You need to alert your manager about this problem quickly and discuss the next steps to stop this attacker and prevent this problem from happening again. You will need to be prepared to tell your boss about the type of attack you discovered and how it was affecting the web server and employees.

### Identify type of attack that caused incident
When looking at the provided logs it is clear that a DOS (Denial of service) attack is what is causing the issue. In this specific instance, it is explicitly clear that a SYN flood attack is what is causing this.
SYN flood attacks overhwlem the web server until it stops responding after being bombarded with SYN packets which is what we see in the logs

---
## Explain how the attack is causing the incident
The following is a break down of a three-way handshake. This is what normal network communication should look like for visitors attempting to establish a connection using the TCP protocol.
| Step | Description |
|---|---|
| 1 | `SYN` : Client sends SYN packet to the server, requesting a connection. |
| 2 | `SYN/ACK` : Server responds with SYN/ACK packet, acknowledging the client's SYN and requesting confirmation of the connection. |
| 3 | `ACK` : Client sends ACK packet to the server, acknowledging the server's SYN/ACK. |

We can see this reflected several times at the beginning of the logs around 3.144521 milliseconds from the first packet capture.
![image](https://github.com/user-attachments/assets/e0a05fb7-ee25-4c88-8f7e-c205f127d58d)


In the above image we see the IP 198.51.100.23 send a request via TCP protocol to 192.0.2.1 and be recieving with a SYN/SYN,ACK/ACK. 
This interaction stands in stark contrast to the following set of logs where we see IP 203.0.113.0 make their first connections to the site.
![image](https://github.com/user-attachments/assets/9002964a-aec1-43e4-acab-6f7209c67e50)

In response to this the destination/company website begins to restart in an attempt to combat any issues that it is encountering
![image](https://github.com/user-attachments/assets/8274f4f8-6d1d-42c5-946f-3093897fce50)

Until eventually the IP 203.0.113.0 completely overtakes the company website.
![image](https://github.com/user-attachments/assets/dc54459a-1400-48e1-b28c-ba9bacab5768)

