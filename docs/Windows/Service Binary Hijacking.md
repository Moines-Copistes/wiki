## Bad Permissions

> A service's binary is not properly protected

1. List services, look for any service that isn't in `C:\Windows\System32`:
```shell
wmic service get name,pathname |  findstr /i /v "C:\Windows\\"
```
```powershell
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}
```

2. Check permissions:
```cmd
icacls <path>
```

3. Cross-compile and replace the service binary (don't forget to backup the original)

4. Restart the service or reboot the machine:
```cmd
# Restart the service
net stop <service>
net start <service>

Stop-Service <service>
Start-Service <service>

# Reboot the machine
shutdown /r /t 0
```

## Unquoted Service Paths

> The executable path of a service is not quoted


### In practice

1. List services, and look for any service with an executable that has spaces & is unquoted (see above).

2. Check the permissions of each potential directory w/ `icacls`.

3. Replace the executable with a malicious one, and restart the service (see above).
