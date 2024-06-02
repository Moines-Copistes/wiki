---
id: Deep links
aliases:
  - Deep links
tags:
  - Android
  - Mobile
  - deep links
---
## Find Deep links 

1. Use [Android-Deeplink-Parser](https://github.com/Shapa7276/Android-Deeplink-Parser) script:
```bash
python3 deeplinkparser.py "$APK_FILE"
```

## Trigger a Deep Link using adb

```bash
adb shell am start -a android.intent.action.VIEW -d "<deeplink>" <package>
```

