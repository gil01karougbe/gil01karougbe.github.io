---
layout: post
title:  "Unconstrained Delegation: The Key to the kingdom"
description: "A break down of all in know about microsoft kerberos uncontrained delegration, It is more a review than a discovery"
date: "2024-10-16"
pin: true
category: "Active Directory"
tags: ["Active Directory", "kerberos double-hop problem", "kerberos delegation", "Unconstrained"]
---

## Introduction
<p style="text-align: justify;">
The <b>double-hop problem</b> emerged in the early 2000s as applications required seamless user authentication across multiple services. To address this, Microsoft introduced Kerberos delegation in Windows Server 2003, enabling trusted accounts (user or computer) to act on behalf of other users across services. The classic senarios of why delegation is needed is when a user authenticates to a web server, using Kerberos or other protocols, and the server wants to nicely integrate with a SQL backend. Unconstrained delegation allows an account with the <b>TrustedForDelegation</b> attribute to impersonate any user—except those in <em>Protected Users</em> group or marked as <em>Account is sensitive and cannot be delegated</em>.
</p>

![delegation](./assets/img/blog/delegation/delegation1.png)

## Exploitation MindMap
![KUD Mindmap](https://www.thehacker.recipes/assets/KUD%20mindmap.DDYXGSWu.png)


## Computer with unconstrained delegation
### Enumeration
```powershell
Get-DomainComputer -Unconstrained
```
### Windows Exploitation


### Linux Exploitation



## User with unconstrained delegation
### Enumeration
```powershell
Get-DomainUser -Unconstrained
```

### Case1: has spn with no valid dns record

### Case2: can add an spn 

## References
[d-hop problem101](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/kerberos-double-hop-problem)<br>
[d-hop problem102](https://techcommunity.microsoft.com/blog/askds/understanding-kerberos-double-hop/395463)<br>
[adsecurity](https://adsecurity.org/?p=1667)<br>
[thehacker.recipes](https://www.thehacker.recipes/ad/movement/kerberos/delegations/unconstrained)<br>
[stecterops](https://posts.specterops.io/hunting-in-active-directory-unconstrained-delegation-forests-trusts-71f2b33688e1)<br>
[ired team](https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/domain-compromise-via-unrestricted-kerberos-delegation)<br>
[hacktricks](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/unconstrained-delegation)<br>