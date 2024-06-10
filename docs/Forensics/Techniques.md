#### T1218 (System Binary Proxy Execution)
- Evasion technique
- Use a binary signed with a digital signature of Microsoft (whether native from Windows or downloaded on Microsoft Website)
- Used by attackers to use legitimate tool but for malicious purposes
- Seen in Sysmon EventID 1
#### T1204 (User Execution)
- Execution technique
- Technique related to user interaction
#### T1070 (Indicator Removal)
- Defense Evasion technique
- Subtechnique T1070.006 - TimeStomping (file creation date is changed to make it appear old)
- Seen in Sysmon EventID 2
#### T1036 (Masquerading)
- Manipulate features of their artifacts to make them appear legitimate (e.g: APT32 has disguised a Cobalt Strike beacon as a Flash Installer)
#### T1055 (Process Injection)
- Privilege Escalation 