# SSH Tunnelling

## Linux
### Local Port Forwarding
> **Use case:** we know what port we want to forward / what service to target.

```shell
ssh -N -L 0.0.0.0:$PIVOT_PORT:$TARGET_IP:$TARGET_PORT $SSH_USER@$SSH_IP
```

- `PIVOT_PORT` -> port on which the pivot will listen
- `TARGET_IP` -> IP address of the target machine
- `TARGET_PORT` -> port on the target machine

### Dynamic Port Forwarding
> **Use case:** we don't know the target port.

1. Enable dynamic port forwarding with SSH:
```shell
ssh -N -D 0.0.0.0:1080 $SSH_USER@$SSH_IP
```

2. Add the following to `/etc/proxychains.conf`:
```plain
socks5 	$PIVOT_IP 1080
```

### Reverse Port Forwarding
> **Use case:** forward a local service to the remote port.

```shell
ssh -N -R $PIVOT_IP:$PIVOT_PORT:0.0.0.0:$LOCAL_PORT $SSH_USER@$SSH_IP
```

Every request made from the internal network to `$PIVOT_IP` on the `$PIVOT_PORT` will be routed to our machine on the `$LOCAL_PORT`

### Remote Port Forwarding
> **Use case:** inbound SSH connections are blocked.

1. Setup a SSH server on attacker host
2. From pivot, run:
```bash
ssh -R 0.0.0.0:$LOCAL_PORT:$TARGET_IP:$TARGET_PORT $SSH_USER@$SSH_IP
```

### Remote Dynamic Port Forwarding
> **Use case:** inbound SSH connections are blocked & we don't know the target port.

Combination of `Remote Port Forwarding` and `Dynamic Port Forwarding`.

1. From pivot, run:
```bash
ssh -R 1080 $SSH_USER@$SSH_IP
```

2. Modify `proxychains.conf` as shown in Dynamic Port Forwarding

## Windows

### ssh.exe

Same usage as on Linux.
May not be in PATH by default, generally in `C:\Windows\System32\OpenSSH\`.

### Plink (reverse port forwarding)

1. Setup a SSH server on attacking machine (`LOCAL`)
2. Transfer `plink.exe` to pivot
3. Start the reverse port forward:
```cmd
plink.exe -ssh -l $LOCAL_USER -pw $LOCAL_PASSWORD -R 127.0.0.1:$LOCAL_PORT:$TARGET_IP:$TARGET_PORT $LOCAL_IP
```

## Netsh
TODO
