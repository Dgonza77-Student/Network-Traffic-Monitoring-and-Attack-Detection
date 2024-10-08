# Analyze Network Layer Communication- Cybersecurity Incident Report

## Objective

Respond to a security issue involving organizations company webstie. Identify the likely cause of the service interruption and explain how the attack occured and service impacts caused.

---

### Skills Learned
- Analyzing network layer protocols (DNS, ICMP, and UDP)

- Troubleshooting network-related issues using tcpdump and packet analysis

- Understanding of DNS resolution and how ICMP error messages work

- Investigating common causes of DNS resolution failures


---

### Tools Used

- tcp dump
- ICMP and UDP packet analysis
---

## Scenario
You are a cybersecurity analyst working at a company that specializes in providing IT services for clients. Several customers of clients reported that they were not able to access the client company website www.yummyrecipesforme.com, and saw the error “destination port unreachable” after waiting for the page to load. 

You are tasked with analyzing the situation and determining which network protocol was affected during this incident. To start, you attempt to visit the website and you also receive the error “destination port unreachable.” To troubleshoot the issue, you load your network analyzer tool, tcpdump, and attempt to load the webpage again. To load the webpage, your browser sends a query to a DNS server via the UDP protocol to retrieve the IP address for the website's domain name; this is part of the DNS protocol. Your browser then uses this IP address as the destination IP for sending an HTTPS request to the web server to display the webpage  The analyzer shows that when you send UDP packets to the DNS server, you receive ICMP packets containing the error message: “udp port 53 unreachable.” 
![image](https://github.com/user-attachments/assets/470ccebe-81c1-4f9c-827a-a389504e0516)

In the tcpdump log, you find the following information:

The first two lines of the log file show the initial outgoing request from your computer to the DNS server requesting the IP address of yummyrecipesforme.com. This request is sent in a UDP packet.

The third and fourth lines of the log show the response to your UDP packet. In this case, the ICMP 203.0.113.2 line is the start of the error message indicating that the UDP packet was undeliverable to port 53 of the DNS server.

In front of each request and response, you find timestamps that indicate when the incident happened. In the log, this is the first sequence of numbers displayed: 13:24:32.192571. This means the time is 1:24 p.m., 32.192571 seconds.

After the timestamps, you will find the source and destination IP addresses. In the first line, where the UDP packet travels from your browser to the DNS server, this information is displayed as: 192.51.100.15 > 203.0.113.2.domain. The IP address to the left of the greater than (>) symbol is the source address, which in this example is your computer’s IP address. The IP address to the right of the greater than (>) symbol is the destination IP address. In this case, it is the IP address for the DNS server: 203.0.113.2.domain. For the ICMP error response, the source address is 203.0.113.2 and the destination is your computers IP address 192.51.100.15.

After the source and destination IP addresses, there can be a number of additional details like the protocol, port number of the source, and flags. In the first line of the error log, the query identification number appears as: 35084. The plus sign after the query identification number indicates there are flags associated with the UDP message. The "A?" indicates a flag associated with the DNS request for an A record, where an A record maps a domain name to an IP address. The third line displays the protocol of the response message to the browser: "ICMP," which is followed by an ICMP error message.

The error message, "udp port 53 unreachable" is mentioned in the last line. Port 53 is a port for DNS service. The word "unreachable" in the message indicates the UDP message requesting an IP address for the domain "www.yummyrecipesforme.com" did not go through to the DNS server because no service was listening on the receiving DNS port.

The remaining lines in the log indicaasaeasete that ICMP packets were sent two more times, but the same delivery error was received both times. 

Now that you have captured data packets using a network analyzer tool, it is your job to identify which network protocol and service were impacted by this incident. Then, you will need to write a follow-up report. 

### Provide a summary of the problem found in the DNS and ICMP traffic log
Upon review of the logs it is made clear that the ICMP request packets indicate that the packet was not delivered to the port 53 of the DNS server. Hence a connection was not established.

We can see the attempted connection a total of three times over a 4 minute period which is indicated by the time stamps (13:24:32:192571 -> 13:28:50.022967)

This is verified when continuing to read down the log and viewing the same message repeated 192.51.100.15.52444 > 203.0.113.2.domain which explains that the user IP(192.51.100.15.524444) on the left side of the greater than symbol (>) is the source address and attempting to contact 203.0.113.2.domain, the destination IP address.

Finally the port and the protocol number when analyzed cement our findings. The port number in the error log states "udp port 53 unreachable length 150" which means that udp protocol was used to attempt a request for a domain name over port 53. With port 53 being a well known port for DNS service it is evident that the message simply did not go through the DNS server and hence the users browser was unable to obtain the IP address for yummyrecipesforme.com.

---
## Explain your Analysis
The incident occured around 1:24 pm
The IT team was made aware after a customer report was given to the company about being unable to gain access to the company's website.
The security engineers began to take a look at the webpage and read the tcp dump logs, seeing what was described above
It is likely this incident occured because of a DOS or a firewall misconfig




