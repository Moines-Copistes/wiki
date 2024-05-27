---
tags:
  - linux
  - privesc
  - cheatsheet
---
## Manual

```
# User(s)
id
cat /etc/passwd
env
cat ~/.bashrc
sudo -l

# Machine
hostname
cat /etc/issue
cat /etc/os-release
uname -a

# Disks
lsblk
/etc/fstab
mount

# Processes
ps aux
watch -n 1 "ps -aux | grep pass"

# Network
ip a
routedel
ss -anp
cat /etc/iptables/rules.v4
sudo tcpdump -i lo -A | grep "pass"

# CRON
la -lah /etc/cron*
crontab -l

# Applications
dpkg -l

# Files permissions
find / -writable -type d 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
ls -l /etc/passwd
ls -l /etc/shadow

# Modules
lsmod
/sbin/modinfo $MODULE

# Interesting dirs
ls -la /opt/
```

## Tools

- [LinPEAS](https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh)
- [unix-privesc-check](https://pentestmonkey.net/tools/unix-privesc-check/unix-privesc-check-1.4.tar.gz)
- [pspy](https://github.com/DominicBreuker/pspy)