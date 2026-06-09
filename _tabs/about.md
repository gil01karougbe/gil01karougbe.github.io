---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

## WhoAmI
![Whoami meme](../assets/img/default/whoami.png)<br>
<p style="text-align: justify;">
I'm <em>gilles karougbe</em>, an offensive security consultant at dataprotect (casablanca, Morocco) with 2 years of experience in cybersecurity. I'm always looking for ways to sharpen my hacking skills to better contribute to the field. I started this blog to document my journey, share my CTF walkthroughs and my thoughts. My goal is to continuously learn, improve and grow. I'm not an expert (at least not yet hahaha...), if you find some gaps in my knowledge just make sure you teach me on your way — I'm probably not aware of them. I hope this blog becomes a valuable resource for others on a similar path.</p>

<p style="text-align: justify;">
This Blog will be more about Web apps security, Active Directory, Android Hacking, Malware Dev, Frida, Defense evasion on the road toward Red Teaming ops...</p> 


## Education
- 🎓[2020-2024] CyberSecurity State Engineer  (National School of Applied Science Oujda, Morocco)<br>
- 🎓[2017-2019] High School  (Scientific High School Lomé, Togo)


## Skills
- Networking: TCP/IP, Switching, Routing, Cisco solutions.
- Programming: C, Python, JavaScript, PowerShell, Bash.
- Web apps pentesting: Owasp Top 10, APIs Testing, Fuzzing, Burpsuite, Postman.
- Active Directory Pentesting: FootHold, Attack Paths Management, Persistence Techniques. 
- Android Pentesting: Reverse Engineering, Instrumentation with Frida.
- Red Teaming: Malware Dev, AV Evasion.

## Experience
- [Jan 2025-Present]   Pentester  Dataprotect, Offensive Security, Casablanca, Morocco.<br>
- [Feb 2024-Jul 2024]  Internship  HenceForth, R&D, Rabat, Morocco.<br>
- [Jun 2023-Aug 2023]  Internship  Dataprotect, SOC, Casablanca, Morocco.

## Certifications 
- [Offensive Security Certified Professional (OSCP+/OSCP)](https://api.accredible.com/v1/frontend/credential_website_embed_image/certificate/170317808)
- [Certified Red Team Professional (CRTP)](https://api.accredible.com/v1/auth/invite?code=2eedd227c5af9d01a80a&credential_id=e2af0bb8-9e80-4c4b-83ce-0b7a6e80e77b&url=https%3A%2F%2Fwww.credential.net%2Fe2af0bb8-9e80-4c4b-83ce-0b7a6e80e77b&ident=15b3aa12-191c-40aa-b673-ad9e0161253e)
- [Certified Red Team Analyst (CRTA)](https://api.accredible.com/v1/frontend/credential_website_embed_image/certificate/126052442)
- [HackTheBox Dante Prolab](../assets/img/certificate/Dante.pdf)
- [HackTheBox Zephyr Prolab](../assets/img/certificate/Zephyr.pdf)
- [Practical Ethical Hacking (PEH)](../assets/img/certificate/peh.pdf)
- [Frida Labs](../assets/img/certificate/mhl-fridalabs.pdf)
- [Tryhackme Comptia Pentest+](../assets/img/certificate/THM-pentest+.png)
- [Tryhackme Jr Pentesting](../assets/img/certificate/THM-jrpentester.png)

## Some Projects
1. insecure deserialization POCs
I built 2 POC web apps that use serialized tokens for session management. The first one, [nodeserialize lab](https://github.com/gil01karougbe/nodeserialize-poc), is a Node.js application that serializes a User JSON object and returns it to the client as a cookie. The latter, [php lab](https://github.com/gil01karougbe/phpserialization-poc), is a PHP application that serializes a User class object. Requests made to authenticated endpoints carry the Cookie header, which gets deserialized server-side — allowing remote code execution. You can find the Docker images for these POCs [here](https://hub.docker.com/repositories/lig10) for testing purposes.

2. myadlab
I configured an Active Directory domain with three machines (DC, PC01, SRV01) and installed a Certificate Authority on the DC. In assumed-breach scenarios, I practiced enumeration of AD objects using PowerShell and performed various Kerberos attacks (ASREPRoasting, Kerberoasting, Golden/Silver/Diamond Tickets, Delegation abuse). I configured and exploited ESC1 through ESC4 and ESC7 privilege escalation scenarios following the SpecterOps ADCS white paper. I also practiced persistence techniques (AdminSDHolder, DSRM, SkeletonKey, Remote Services Security Descriptors). Check it out [here](https://github.com/gil01karougbe/myadlab).


3. frida for all the things
In this project I created various instrumentation scripts aimed at extracting and modifying arguments passed to functions or altering their return values. One of the key achievements was instrumenting the `AmsiScanBuffer()` API from `amsi.dll`. By modifying the `AMSI_RESULT` value returned by the `AmsiScanBuffer()` call, I was able to bypass AMSI checks and execute PowerShell scripts that would normally be blocked. Check FridaForAllTheThings [here](https://github.com/gil01karougbe/FridaScriptsForAllTheThings).


4. smbsharesdumper
A Python-based tool to enumerate, download, and manage SMB shares across an Active Directory network. Built to replace the tedious one-share-at-a-time workflow of smbclient — pull down everything a user has access to in one shot, then focus on analysing the content. Check it out [here](https://github.com/gil01karougbe/smbsharesdumper).


## CTF Profiles
- [HackTheBox](https://app.hackthebox.com/profile/983770)
- [TryHackMe](https://tryhackme-badges.s3.amazonaws.com/gil01Karougbe.png)


## Social Media
- [LinkedIn](https://ma.linkedin.com/in/essognim-gilles-karougbe-015979223)
- [Twitter](https://x.com/01karougbe)




