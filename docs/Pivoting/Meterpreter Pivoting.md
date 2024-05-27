# Meterpreter Pivoting

## Static Port Forwarding

With a meterpreter session established, we can use `portfwd` to easily forward ports.

```meterpreter
portfwd add -l <local-port> -p <target-port> -r <target-ip>
```

## Dynamic Port Forwarding

1. When our meterpreter session is established, we can use the `auxiliary/server/socks_proxy` module start a listener on the port specified by `SRVPORT`. The `SRVHSOT` should be set to `0.0.0.0` to listen on every interface and the SOCKS `version` should be set to `4a`.

2. Add the following line to `/etc/proxychains.conf`:

		socks4 	127.0.0.1 <SRVPORT>

3. We can then create the routes with the `post/multi/manage/autoroute` module. All we have to set is the meterpreter `SESSION` and the target `SUBNET`.

4. We can now use external tools with proxychains, or use metasploit's modules directly against the target network's hosts.

> **Note:** The first 2 steps are only necessary if we want to use tools external to metasploit. (i.e. Nmap or a RDP client)

## Reverse Port Forwarding

As for static port forwarding, we can use `portfwd` from within the meterpreter session. `portfwd` has a built-in option for reverse port forwarding.

```meterpreter
portfwd add -R -l <attacker-port> -p <pivot-port> -L <pivot-ip>
```
