---
layout: post
title:  "smbsharesdumper"
date: "2024-10-16"
category: "Tools"
tags: ["Active Directory", "SMB", "SMB Shares", "Dump"]
---


# What is smbsharesdumper?
A python based tools to manage SMB Shares accross an active directory network.

# Why i needed it?
When i enumerate a host and find multiple shares i usualy use `smbclient \\\\host\\shareX` to connect to each and download content one by one using either `get <file>` or `mget <folder>`. This was rudimentaty for me especially when we think that a user can have access to multiples shares on multiples hosts accross the network. Yeah...there is place for automation here. I wanted to put my coding knownledge in practice in a project and i was planing to build an Active Directory Lab (I will share the lab later on another post) to configure vulnerables senarios myself in order to level up my AD pentesting skills.

With smbsharesdumper, download all shares a user have access accross the network once and focus on anlysing the content to find juicy staff. You can either use `grep -iR 'keyword'` or browse the downloaded content graphically.

# Usage
```
$ python3 smbsharesdumper.py                                                                    
usage: smbsharesdumper.py [-h] [-d DOMAIN] [-u USERNAME] [-p PASSWORD] [-H HASHES] [--host HOST] [-P [destination port]] [--folder FOLDER]
                          [--file FILE] [--list-shares] [--debug] [--list-content] [--sharename SHARENAME] [--dump] [--dumpfile] [--mkdir]
                          [--delete] [--upload] [--destination DESTINATION] [--targets-file TARGETS_FILE]
                          [target]

smbsharesdumper for Pentesters

positional arguments:
  target                [[domain/]username[:password]@]<targetName or address>

options:
  -h, --help            show this help message and exit
  -d DOMAIN, --domain DOMAIN
                        Domain name
  -u USERNAME, --username USERNAME
                        Username
  -p PASSWORD, --password PASSWORD
                        Password
  -H HASHES, --hashes HASHES
                        LMHASH:NTHASH
  --host HOST           Target address or targetName
  -P [destination port], --port [destination port]
                        Destination port to connect to SMB Server
  --folder FOLDER       Folder name
  --file FILE           File name
  --list-shares         List of shares on the server
  --debug               Enable debug mode
  --list-content        List of shares content
  --sharename SHARENAME
                        Name of the share
  --dump                Dump content of a share locally
  --dumpfile            Dump a single file locally
  --mkdir               Make a directory
  --delete              Delete a directory or a file
  --upload              Upload a file
  --destination DESTINATION
                        Destination path
  --targets-file TARGETS_FILE
                        list of ipaddress of targets
```

# Examples

### 1-list shares
```
python3 smbsharesdumper.py -d DOMAIN -u USER -p PASS --targets-file /Path/To/targets.txt --list-shares
```

### 2-list content
```
python3 smbsharesdumper.py -d DOMAIN -u USER -p PASS --host IP --list-content --sharename SHARE
```

### 3-dump shares
```
python3 smbsharesdumper.py -d DOMAIN -u USER -p PASS --targets-file /Path/To/targets.txt --dump --destination OUTPUT_FOLDER
```

### 4-dump a single file
```
python3 smbsharesdumper.py  DOMAIN/USER:PASS@IP --dumpfile --share SHARE --file REMOTE_FILE --destination OUTPUT_FOLDER
```

### 5-Upload a file
```
python3 smbsharesdumper.py  DOMAIN/USER:PASS@IP --upload --share SHARE --folder REMOTE_FOLDER --file LOCAL_FILE
```

# Notes
The netexec and i guess his format crackmapexec does the job too.

### 1-list shares
```
nxc smb <NETWORK> -u USER -p PASS --shares
```

### 2-dump shares
```
nxc smb <NETWORK> -u USER -p PASS -M spider_plus -o DOWNLOAD_FLAG=True OUTPUT_FOLDER=.
```