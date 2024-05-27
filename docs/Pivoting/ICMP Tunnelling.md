# ICMP Tunnelling

ICMP tunnelling encapsulates your traffic within `ICMP packets` containing `echo requests` and `responses`. ICMP tunneling would only work when ping responses are permitted within a firewalled network.

The [ptunnel-ng](https://github.com/utoni/ptunnel-ng) tool allows to create a tunnel between the pivot and the attack host

1. Start the server on the *target host*:
```shell
sudo ./ptunnel-ng -r<server-ip> -R22
```
-> The `server-ip` is whatever IP is accessible from the attack host

2. Connect to the server from the *attack host*:
```shell
sudo ./ptunnel-ng -p<server-ip> -l<local-port> -r<target-ip> -R22
```

At this point, we can already access the target through the attack host on the `local-port`.
### Dynamic Port Forwarding

This assumes that the target-ip has a SSH server, and will serve as pivot.

1. Enable dynamic port forwarding with SSH:
```shell
ssh -D <proxy-port> -p<local-port> -l<ssh-username> 127.0.0.1
```

2. Configure proxychains
