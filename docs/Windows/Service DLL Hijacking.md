### In practice

1. List services, look for any service that isn't in `C:\Windows\System32`:
```powershell
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}
```

2. Use `ProcMon` to search for the service

3. While procmon is running, restart the service and find the CreateFile calls

4. Look at the PATH to match the paths used in the CreateFile calls

5. If one of the paths is writeable, replace the DLL. Easiest way is to use msfvenom.