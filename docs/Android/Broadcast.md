---
id: Broadcast
aliases:
  - Deep links
tags:
  - Android
  - Mobile
  - Broadcast
---
# Broadcasts

## Overview
Broadcast is a messaging system that allows applications and the system to send (broadcast) or receive messages using broadcast receivers (events) across the whole system. It is used for communication between different parts of the system and various applications. Broadcasts can inform all apps about events such as:

- Low battery
- AirPlane mode changed
- Headset plugged
- ...

## Send a broadcast using am

The intent-filter is the event that the broadcast is subscribed to
```bash
am broadcast -a <intent-filter> -n <package/.BroadcastActivity>
```

## Sniff a broadcast using Drozer

```bash
run app.broadcast.sniff --action <intent-filter>
```


