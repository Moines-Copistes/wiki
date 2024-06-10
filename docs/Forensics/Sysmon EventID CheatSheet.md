# Sysmon EventID CheatSheet

## Overview
Sysmon (System Monitor) is a Windows system service and device driver that logs system activity to the Windows event log. It provides detailed information about process creations, network connections, file creations, and more. Each type of activity is logged with a specific EventID.

## EventIDs and Descriptions

### Process Events
- **EventID 1**: **Process Creation**
  - Logs when a process is created.
  - **Fields**: `ProcessId`, `Image`, `CommandLine`, `ParentProcessId`, `ParentImage`, `ParentCommandLine`

- **EventID 5**: **Process Terminated**
  - Logs when a process terminates.
  - **Fields**: `ProcessId`, `Image`, `User`

- **EventID 6**: **Driver Loaded**
  - Logs when a driver is loaded.
  - **Fields**: `ImageLoaded`, `Hashes`, `Signed`, `Signature`

### File Events
- **EventID 2**: ** Process changed a file creation time**
  - Logs when a process changed the time metadata.
  - Can be related to timestoping attack
  - **Fields**: `ImageLoaded`, `Hashes`, `Signed`, `Signature`, `Image`

- **EventID 7**: **Image Load**
  - Logs when an image (e.g., DLL) is loaded by a process.
  - **Fields**: `ImageLoaded`, `Hashes`, `Signed`, `Signature`, `Image`

- **EventID 8**: **CreateRemoteThread**
  - Logs when a process creates a thread in another process.
  - **Fields**: `SourceImage`, `SourceProcessId`, `TargetImage`, `TargetProcessId`, `NewThreadId`

### Network Events
- **EventID 3**: **Network Connection**
  - Logs when a network connection is initiated.
  - **Fields**: `SourceIp`, `SourcePort`, `DestinationIp`, `DestinationPort`, `Protocol`

### File and Registry Events
- **EventID 11**: **File Create**
  - Logs when a file is created or overwritten.
  - **Fields**: `FileName`, `Hashes`

- **EventID 12**: **Registry Object Added/Deleted**
  - Logs when a registry object is added or deleted.
  - **Fields**: `EventType`, `UtcTime`, `ProcessGuid`, `ProcessId`, `Image`, `TargetObject`

- **EventID 13**: **Registry Value Set**
  - Logs when a registry value is set.
  - **Fields**: `EventType`, `UtcTime`, `ProcessGuid`, `ProcessId`, `Image`, `TargetObject`, `Details`

- **EventID 14**: **Registry Object Renamed**
  - Logs when a registry key or value is renamed.
  - **Fields**: `EventType`, `UtcTime`, `ProcessGuid`, `ProcessId`, `Image`, `TargetObject`, `NewName`

### File and Directory Events
- **EventID 15**: **File Create Stream Hash**
  - Logs when a file stream is created.
  - **Fields**: `Hashes`, `FileName`, `StreamName`

- **EventID 16**: **Sysmon Configuration Change**
  - Logs when the Sysmon configuration is changed.
  - **Fields**: `Configuration`, `UtcTime`

- **EventID 17**: **Pipe Created**
  - Logs when a named pipe is created.
  - **Fields**: `PipeName`, `UtcTime`

- **EventID 18**: **Pipe Connected**
  - Logs when a named pipe connection is made.
  - **Fields**: `PipeName`, `UtcTime`

### WMI Events
- **EventID 19**: **WMI Filter**
  - Logs when a WMI filter is registered.
  - **Fields**: `FilterName`, `Query`

- **EventID 20**: **WMI Consumer**
  - Logs when a WMI consumer is registered.
  - **Fields**: `ConsumerName`, `FilterName`

- **EventID 21**: **WMI Consumer to Filter**
  - Logs when a WMI consumer is bound to a filter.
  - **Fields**: `ConsumerName`, `FilterName`

### DNS Events
- **EventID 22**: **DNS Query**
  - Logs when a DNS query is performed.
  - **Fields**: `QueryName`, `QueryStatus`, `QueryResults`

### File Delete Events
- **EventID 23**: **File Delete (Pending)**
  - Logs when a file is marked for deletion.
  - **Fields**: `FileName`, `UtcTime`

- **EventID 24**: **File Deleted**
  - Logs when a file deletion is completed.
  - **Fields**: `FileName`, `UtcTime`

### Clipboard Events
- **EventID 25**: **Clipboard Change**
  - Logs when the clipboard content changes.
  - **Fields**: `ProcessId`, `Image`, `QueryName`, `QueryResults`

### Additional Events
- **EventID 255**: **Error**
  - Logs errors encountered by Sysmon.
  - **Fields**: `ErrorCode`, `Message`

## Notes
- **UtcTime**: All event timestamps are in UTC time.
- **Hashes**: Include multiple hash types such as MD5, SHA256, and IMPHASH.
- **Signed/Signature**: Indicates if the file or driver is signed and provides signature details.
