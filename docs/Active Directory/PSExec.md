---
tags:
    - windows
    - lateral
    - sysinternals
---
## Prerequisites

- User that authenticates to target need to be admin
- `ADMIN$` share must be writeable
- File and Printer Sharing must be enabled
## Usage

```cmd
./PsExec64.exe -i \\$TARGET -u $DOMAIN\$USER -p $PASS
```