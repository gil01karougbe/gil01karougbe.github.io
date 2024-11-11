---
layout: post
title:  "Unconstrained Delegation: The Key to kingdom"
description: "a break down of all in know about microsoft kerberos uncontrained delegration. This is more a review than a discovery"
author: "_lig10"
date: "2024-10-16"
pin: true
category: "Active Directory"
tags: ["Active Directory", "kerberos delegation", "Unconstrained"]
---

## Introduction
<p style="text-align: justify;">
The `double-hop problem` emerged in the early 2000s as applications required seamless user authentication across multiple services. To address this, Microsoft introduced Kerberos delegation in Windows Server 2003, enabling trusted accounts (user or computer) to act on behalf of users across services. Unconstrained delegation allows an account with the `TrustedForDelegation` attribute to impersonate any user—except those in `Protected Users` or marked as `Account is sensitive and cannot be delegated`.
</p>


## Exploitation MindMap
![KUD Mindmap](https://www.thehacker.recipes/assets/KUD%20mindmap.DDYXGSWu.png)


## Computer with unconstrained delegation
### Enumeration
```powershell
Get-DomainComputer -Unconstrained
```
### Windows


### Linux



## User with unconstrained delegation
### Enumeration
```powershell
Get-DomainUser -Unconstrained
```

### Case1: has spn with no valid dns record

### Case2: can add an spn 

## References