# Socks Tunnelling

## Server on Pivot

1. Start the server on the *pivot host*:
```shell
./chisel server -v -p <server-port> --socks5
```

2. Connect to the server using the client on the *attack host*:
```shell
./chisel client -v <server-host>:<server-port> socks
```

3. Modify proxychain's config by adding:
```plain
socks5 127.0.0.1 1080
```

## Client on Pivot

1. Start the server on the *attack host*:
```shell
sudo ./chisel server --reverse -v -p <server-port> --socks5
```

2. Connect to the server using the client on the *pivot host*:
```shell
./chisel client -v <server-host>:<server-port> R:socks
```

3. Modify proxychain's config by adding:
```plain
socks5 127.0.0.1 1080
```
