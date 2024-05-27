---
tags:
  - windows
  - privesc
---
## Manual
```
# Current User
whoami
whoami /groups
whoami /priv

# Local Users
net user
net user $USERNAME
Get-LocalUser

net group
net group $GROUP_NAME
Get-LocalGroup
Get-LocalGroupMember $GROUP_NAME

# System
systeminfo

# Processes
tasklist
Get-Process

# Network
ipconfig /all
route print
netstat -ano

# Powershell History
Get-History
(Get-PSReadlineOption).HistorySavePath

# Scheduled tasks
schtasks /query /fo LIST /v
Get-ScheduledTask | where {$_.Author -notlike "Microsoft*"} | ft TaskName,URI

# Softwares
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname 

Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname


# Interesting files
Get-ChildItem -Path C:\xampp -Include *.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue

Get-ChildItem -Path C:\Users\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx,*.ovpn,*.kdbx -File -Recurse -ErrorAction SilentlyContinue


# Services
wmic service get name,pathname |  findstr /i /v "C:\Windows\\"
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}

# If putty is installed, check credentials or keys
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions" /s
reg query HKCU\Software\SimonTatham\PuTTY\SshHostKeys\
```

## Tools
- winPEAS
	- [winPEAS.ps1](https://raw.githubusercontent.com/carlospolop/PEASS-ng/master/winPEAS/winPEASps1/winPEAS.ps1)
	- [winPEAS.exe](https://github.com/carlospolop/PEASS-ng/releases/latest/download/winPEASx64.exe)
- [Seatbelt](https://github.com/GhostPack/Seatbelt)
- [JAWS](https://github.com/411Hall/JAWS)

