---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

## WhoAmI
![Whoami meme](../assets/img/default/whoami.png)<br>
<p style="text-align: justify;">
I'm <em>gilles karougbe</em>, a cybersecutity enthusiast and fresh graduate as information security and cybersecurity state engineer. I'm always looking for ways to sharpen my hacking skills to better contribute to the filed. I have started this blog to document my journey, share my ctf walkthroughs and my  thoughts. My goal is to continuously learn, improve and grow. I'm not an expert (at least not yet hahaha...), if you find some gaps in my  knowledge just make sure you teaches me on your way—I'm probaly not aware of them. I hope this blog becomes a valuable resource for others on a similar path.</p>

<p style="text-align: justify;">
This Blog will be more about Web apps security, Active Directory, Android Hacking, Malware Dev, Frida, Defense evasion on the road toward Red Teaming ops...</p> 


## Education
- 🎓[2020-2024] CyberSecurity State Engineer  (National School of Applied Science Oujda, Morocco)<br>
- 🎓[2017-2019] High School  (Scientific High School Lomé, Togo)


## Skills
- Networking: TCP/IP, Switching, Routing, Cisco solutions.
- Programing: C, Python, JavaScript, PowerShell, Bash.
- Web apps pentesting: Owasp Top 10, APIs Testing, Fuzzing, Burpsuite, Postman.
- Active Directory Pentesting: FootHold, Attack Paths Management, Persistence Techniques. 
- Android Pentesting: Reverse Enginering, Instrumentation with Frida.
- Red Teaming: Malware Dev, AV Evasion.

## Experience
- [Feb 2024-Jul 2024]  Internship  HenceForth, R&D, Rabat, Morocco.<br>
- [Jun 2023-Aug 2023]  Internship  Dataprotect, SOC, Casablanca, Morocco.

## Certifications 
- [Certified Red Team Professional (CRTP)](https://api.accredible.com/v1/auth/invite?code=2eedd227c5af9d01a80a&credential_id=e2af0bb8-9e80-4c4b-83ce-0b7a6e80e77b&url=https%3A%2F%2Fwww.credential.net%2Fe2af0bb8-9e80-4c4b-83ce-0b7a6e80e77b&ident=15b3aa12-191c-40aa-b673-ad9e0161253e)
- [HackTheBox Dante Prolab](../assets/img/certificate/Dante.pdf)
- [Pratical Ethical Hacking (PEH)](../assets/img/certificate/peh.pdf)
- [Frida Labs](../assets/img/certificate/mhl-fridalabs.pdf)
- [Tryhackme Comptia Pentest+](../assets/img/certificate/THM-pentest+.png)
- [Tryhackme Jr Pentesting](../assets/img/certificate/THM-jrpentester.png)

## Some Projects
1. insecure deserialization POCs
I have Built 02 poc web apps that uses serialized tokens for session management purpose. The first one [nodeserialize lab](https://github.com/gil01karougbe/nodeserialize-poc) is a nodejs application that serialize a User json object and return it to the client as a Cookie and the later one [php lab](https://github.com/gil01karougbe/phpserialization-poc) is a PHP application the serialize a User Class object. Requests made to authenticated endpoints later on have the Cookie header which got deserialized on the server side allowing remote code execution. You can find these Pocs doker images [here](https://hub.docker.com/repositories/lig10) for testing purpose.

2. myadlab
I have configured an Active Directory domain with three machines (dc, pc01, srv01) and installed on the DC a Certificate Authority. In an assumed breach scenarios, i have practice enumeration of AD objects using powershell and  performed virious Kerberos attacks (ASRepRoasting, Kerberoasting, Golden/Silver/Diamond Tickets, Delegation abuse). I have configured and exploited following the specterOps ADCS white paper ESC1 to ESC4 and ESC7 privilege escalation senarios. I have also  practiced some persistence techniques (AdminSDHolder, DSRM, SkeletonKey, Remote Services Security Descriptors). Checkt it [here](https://github.com/gil01karougbe/myadlab)


3. frida for all the things
In this project i have created various instrumentation scripts aimed at extracting and modifying arguments passed to functions or altering function return values. One of the key achievements in this project was the instrumentation of the `AmsiScanBuffer()` API from `amsi.dll`. By modifying the `AMSI_RESULT`value returned by `AmsiScanBuffer()` call, I was able to bypass AMSI checks, allowing the execution of PowerShell scripts that would normally be blocked. Check FridaForAllTheThings [here](https://github.com/gil01karougbe/FridaScriptsForAllTheThings)


3. smbsharesdumper
[smbsharesdumper](https://github.com/gil01karougbe/smbsharesdumper)


## CTF Profiles
- [HackTheBox](https://app.hackthebox.com/profile/983770)
- [TryHackme](https://tryhackme-badges.s3.amazonaws.com/gil01Karougbe.png)


## Social Medias 
- [LinKedin](https://ma.linkedin.com/in/essognim-gilles-karougbe-015979223)
- [Twitter](https://x.com/01karougbe)




