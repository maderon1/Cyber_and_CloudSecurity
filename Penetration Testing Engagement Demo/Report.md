Penetration Testing Engagement

In this engagement, I will play the role of an independent penetration tester hired by GoodCorp Inc. to perform security tests against their CEO’s workstation.

Scenario:  
- The CEO claims to have passwords that are long and complex and therefore unhackable.
- I am tasked with gaining access to the CEO's computer and using a Meterpreter session to search for two files that contain the strings recipe and seceretfile.

Scope:
- The scope of this engagement is limited to the CEO's workstation only. I am not permitted to scan any other IP addresses or exploit anything other than the CEO's IP address.
- The CEO has a busy schedule and cannot have the computer offline for an extended period of time. Therefore, denial of service and brute force attacks are prohibited.
- After you gain access to the CEO’s computer, you may read and access any file, but you cannot delete them. Nor are you allowed to make any configurations changes to the computer.
- Since I have already been provided access to the network, OSINT won't be necessary.


1.	High-Level Summary

GoodSecurity was tasked with performing an internal penetration test on GoodCorp’s CEO, Hans Gruber. An internal penetration test is a dedicated attack against internally connected systems. The goal of this test is to perform attacks similar to those of a hacker and attempt to infiltrate Hans’ computer to determine if it is at risk. GoodSecurity’s overall objective was to exploit any vulnerable software, find a secret recipe file on Hans’ computer, and report the findings back to GoodCorp.
The internal penetration test found several alarming vulnerabilities on Hans’ computer: When performing the attacks, GoodSecurity was able to gain access to his machine and find the secret recipe file by exploiting two programs with major vulnerabilities. The details of the attack are below.


2.	Findings

Machine IP: 192.168.0.20
Hostname: MSEDGEWIN10
Vulnerability Exploited: Icecast is the vulnerability that has been found. I used Metasploit – a tool suite for hacking servers and other networked devices to identify this vulnerability. MSFconsole is the main interface of this tool suite. The command to launch Metasploit on Kali is ‘msfconsole’ or ‘msfdb run’. ‘Search Icecast’ allowed me to look for all the modules related to this vulnerability. As shown in the screenshot below, exploit is the only module. The exploit module is generally used to deliver exploit code to a target system. 

![image](https://user-images.githubusercontent.com/78771318/128652452-1e580d38-05d7-4890-8835-730e2146adbb.png)

Vulnerability Explanation: We can learn more about a specific vulnerability using ‘searchsploit’ on Kali Linux. I ran ‘searchsploit icecast’ (screenshot below) to view all the icecast exploits on my machine. This command also allows me to apply the available exploits to the compromise machine without having to download additional tools.  

![image](https://user-images.githubusercontent.com/78771318/128652468-1f58c77b-4abf-4a1e-84fc-878ededddb2e.png)

According to the these .txt files listed in the above screenshot and threatpost.com, Icecast is an open-source audio streaming server maintained by Xiph.org Foundation. It supports a huge number of MP3 and other audio streams across the globe daily, mainly for independent ratio stations and personal jukeboxes. 
This vulnerability is caused by a boundary error when processing URL in url_add_client() function in auth_url.c. A remote unauthenticated attacker can send an overly long URL to the affected server. Since Icecast does not filter encoded characters from URLs when receiving web requests, if an attacker crafts a URL containing the ASCII equivalent of directory traversal characters, it is possible to escape the root directory of Icecast. 

Severity: This vulnerability may cause a serious security issue to GoodCorp because it allows the attacker to view files readable by the ownership and group membership of the server. It also triggers buffer overflow and crash the server or execute arbitrary code on the compromised machine.

Proof of Concept: To examine this vulnerability, I ran the Icecast exploit after setting the RHOST to the target machine (192.168.0.20). The Kali VM switched from msf5 to meterpreter after running ‘exploit‘ indicating that the target machine has been hacked. I then tried to search for two .txt files with the commands ‘search -f *secretfile*.txt‘ and ‘search -f *recipe*.txt‘ on the target machine. I sucessfully opened the user.secretfile.txt where someone’s sensitive personal information is saved. 


