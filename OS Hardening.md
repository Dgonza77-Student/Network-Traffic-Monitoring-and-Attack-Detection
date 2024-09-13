# Analyze Network Layer Attack- Cybersecurity Incident Report

## Objective
Document the incident in detail, including identifying the network protocols used to establish the connection between the user and the website to make a reconmendation for a course of action


---

### Skills Learned
- Brute force investigation and documentation
- Analyzing network traffic 

---

### Tools Used

- TCP dump
  
## Scenario

You are a cybersecurity analyst for yummyrecipesforme.com, a website that sells recipes and cookbooks. A former employee has decided to lure users to a fake website with malware. 

The baker executed a brute force attack to gain access to the web host. They repeatedly entered several known default passwords for the administrative account until they correctly guessed the right one. After they obtained the login credentials, they were able to access the admin panel and change the website’s source code. They embedded a javascript function in the source code that prompted visitors to download and run a file upon visiting the website. After embedding the malware, the baker changed the password to the administrative account. When customers download the file, they are redirected to a fake version of the website that contains the malware. 

Several hours after the attack, multiple customers emailed yummyrecipesforme’s helpdesk. They complained that the company’s website had prompted them to download a file to access free recipes. The customers claimed that, after running the file, the address of the website changed and their personal computers began running more slowly. 

In response to this incident, the website owner tries to log in to the admin panel but is unable to, so they reach out to the website hosting provider. You and other cybersecurity analysts are tasked with investigating this security event.

To address the incident, you create a sandbox environment to observe the suspicious website behavior. You run the network protocol analyzer tcpdump, then type in the URL for the website, yummyrecipesforme.com. As soon as the website loads, you are prompted to download an executable file to update your browser. You accept the download and allow the file to run. You then observe that your browser redirects you to a different URL, greatrecipesforme.com, which contains the malware.  

The logs show the following process:

The browser initiates a DNS request: It requests the IP address of the yummyrecipesforme.com URL from the DNS server.

The DNS replies with the correct IP address. 

The browser initiates an HTTP request: It requests the yummyrecipesforme.com webpage using the IP address sent by the DNS server.

The browser initiates the download of the malware.

The browser initiates a DNS request for greatrecipesforme.com.

The DNS server responds with the IP address for greatrecipesforme.com.

The browser initiates an HTTP request to the IP address for greatrecipesforme.com.

A senior analyst confirms that the website was compromised. The analyst checks the source code for the website. They notice that javascript code had been added to prompt website visitors to download an executable file. Analysis of the downloaded file found a script that redirects the visitors’ browsers from yummyrecipesforme.com to greatrecipesforme.com. 

The cybersecurity team reports that the web server was impacted by a brute force attack. The disgruntled baker was able to guess the password easily because the admin password was still set to the default password. Additionally, there were no controls in place to prevent a brute force attack. 

Your job is to document the incident in detail, including identifying the network protocols used to establish the connection between the user and the website.  You should also recommend a security action to take to prevent brute force attacks in the future.

### Traffic Log
| Time           | Source                | Destination                | Flags  | Details                                                                                      |
|----------------|-----------------------|----------------------------|--------|----------------------------------------------------------------------------------------------|
| 14:18:32.192571 | your.machine.52444    | dns.google.domain           | -      | 35084+ A? yummyrecipesforme.com. (24)                                                        |
| 14:18:32.204388 | dns.google.domain     | your.machine.52444          | -      | 35084 1/0/0 A 203.0.113.22 (40)                                                              |
| 14:18:36.786501 | your.machine.36086    | yummyrecipesforme.com.http  | [S]    | seq 2873951608, win 65495, options [mss 65495,sackOK,TS val 3302576859 ecr 0,nop,wscale 7]    |
| 14:18:36.786517 | yummyrecipesforme.com | your.machine.36086          | [S.]   | seq 3984334959, ack 2873951609, win 65483, options [mss 65495,sackOK,TS val 3302576859 ecr 0] |
| 14:18:36.786529 | your.machine.36086    | yummyrecipesforme.com.http  | [.]    | ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859]                           |
| 14:18:36.786589 | your.machine.36086    | yummyrecipesforme.com.http  | [P.]   | seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 73: HTTP GET / HTTP/1.1 |
| 14:18:36.786595 | yummyrecipesforme.com | your.machine.36086          | [.]    | ack 74, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859]                           |
...<a lot of traffic on the port 80>...
| 14:20:32.192571 | your.machine.52444    | dns.google.domain           | -      | 21899+ A? greatrecipesforme.com. (24)                                                        |
| 14:20:32.204388 | dns.google.domain     | your.machine.52444          | -      | 21899 1/0/0 A 192.0.2.17 (40)                                                                |
| 14:25:29.576493 | your.machine.56378    | greatrecipesforme.com.http  | [S]    | seq 1020702883, win 65495, options [mss 65495,sackOK,TS val 3302989649 ecr 0,nop,wscale 7]    |
| 14:25:29.576510 | greatrecipesforme.com | your.machine.56378          | [S.]   | seq 1993648018, ack 1020702884, win 65483, options [mss 65495,sackOK,TS val 3302989649 ecr 0] |
| 14:25:29.576524 | your.machine.56378    | greatrecipesforme.com.http  | [.]    | ack 1, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649]                           |
| 14:25:29.576590 | your.machine.56378    | greatrecipesforme.com.http  | [P.]   | seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 73: HTTP GET / HTTP/1.1 |
| 14:25:29.576597 | greatrecipesforme.com | your.machine.56378          | [.]    | ack 74, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649]                           |
...<a lot of traffic on the port 80>...

### Explain the incident
A brute force attack is being conducted against the website.

The HTTP Protocol is being used to transport malicious files to the user machine on the application layer. This is shown in the TCP Dump logs where we see, after the first HTTP GET request, TCP dump detects an issue and starts recording the packets.

This was largely done by prompting the user to download files containing recipies, which is what initiated the brute force attack.


### Identify and Respond
The logs above show our machine (your.machine.52444) attempting to, and eventually establishing, a connection with yummyrecipesforme.com. 

After going to google and searching for the website we see a three-way handshake occur between our machine and the websites

| 14:18:36.786501 | your.machine.36086    | yummyrecipesforme.com.http  | [S]    | seq 2873951608, win 65495, options [mss 65495,sackOK,TS val 3302576859 ecr 0,nop,wscale 7]    |

| 14:18:36.786517 | yummyrecipesforme.com | your.machine.36086          | [S.]   | seq 3984334959, ack 2873951609, win 65483, options [mss 65495,sackOK,TS val 3302576859 ecr 0] |

| 14:18:36.786529 | your.machine.36086    | yummyrecipesforme.com.http  | [.]    | ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859]                           |

Following this, it is made clear that we are operating on port 80, the common port used for HTTP, with the following line. Here our machine asks the website for information 

| 14:18:36.786589 | your.machine.36086    | yummyrecipesforme.com.http  | [P.]   | seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 73: HTTP GET / HTTP/1.1 |

Followed by a response from the website in which the information is sent to us

| 14:18:36.786595 | yummyrecipesforme.com | your.machine.36086          | [.]    | ack 74, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859]                           |

It is here we can surmize that TCP dump began to capture the packet logs as the following message occurs. Indicating that our malicious file may have been transported at this step in our process

...<a lot of traffic on the port 80>...

Thus the HTTP protocol was what got attacked and was being used to transport malicious files to the user at the application layer.
---

### Course of Action Reconmendation

Properly password management systems need to be installed and practiced by the organization in order to ensure that admin accounts do not fall victim to brute force attacks.

Removing the ability to use old passwords and keeping a minimum password length, as mentioned in the Botium Toys' incident, to keep up with NIST password Standards would be benefical.

The implementation of two-factor authentication (2FA) and one-time passcode (OTP) send to the admins phone when attempting to log in will add another additional layer of security and nearly eliminate the possibility of brute force attacks from occuring again.
