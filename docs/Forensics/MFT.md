## Understanding the Master File Table (MFT) in NTFS

### Introduction
The Master File Table (MFT) in the NTFS filesystem is a "database" that contains information about every file and directory on the volume.
### Types of Data in the MFT

1. **File and Directory Metadata**:
    - **File Name**: The name of the file or directory.
    - **File Attributes**: System, hidden, read-only, archive flags, etc.
    - **Timestamps**: Creation time, modification time, access time, and MFT entry modification time.
2. **File Size**:
    - The size of the file in bytes.
3. **File Permissions**:
    - Access control lists (ACLs) and permission settings.
4. **Data Location**:
    - **Data Runs**: Pointers to the clusters on the disk where the actual file data is stored.
    - **Extent Information**: Details about the contiguous or fragmented extents of the file.
5. **File Content (for small files)**:
    - For small files, the actual content might be stored directly within the MFT entry, referred to as resident data.
### MFT Entry Structure

An MFT entry is typically 1 KB in size and consists of several attributes, each describing different aspects of the file or directory:

1. **$STANDARD_INFORMATION**:
    - Contains basic metadata like timestamps and file attributes.
2. **$FILE_NAME**:
    - Stores the file name and the parent directory reference.
3. **$DATA**:
    - Describes the location of the file’s data on the disk. This attribute can be either resident (stored within the MFT entry) or non-resident (stored in external clusters).
4. **$ATTRIBUTE_LIST** (if present):
    - Lists additional attributes if they do not fit within the main MFT entry.
5. **$OBJECT_ID** (optional):
    - Used by distributed link tracking services.
6. **$SECURITY_DESCRIPTOR**:
    - Contains security information, including ownership and permissions.
7. **$VOLUME_INFORMATION**:
    - Specific to volume metadata, not individual files.

Each MFT record is 1024 bytes in size. If a file on disk is smaller than 1024 bytes, it can be stored directly in the MFT entry. These are called MFT Resident files.

## Recovering Information from the MFT

### Step 1: Parsing

Using Eric Zimmerman's tool [MFTECmd](https://github.com/EricZimmerman/MFTECmd), you can parse the MFT:

```shell
.\MFTECmd.exe -f .\files\MFT --csv ".\output\" --csvf mft_output.csv
```

Note: It is important to output the data in CSV format because the following tool can only extract data from CSV files.
### Step 2: Reading

Opening the parsed file with VS Code reveals it contains around 388,000 lines, making it difficult to read directly. 

To streamline the process, use [Timeline Explorer](https://f001.backblazeb2.com/file/EricZimmermanTools/net6/TimelineExplorer.zip). This tool helps display all the files and information needed:

![[Pasted image 20240610110609.png]]
### Key Informations Available in Timeline Explorer:
- Filename
- File path
- Extension
- Last Modified Time, Creation Time, Last Access Time
- Whether the file was in use

A variety of sorting options are available, making it very useful for investigations. You can sort by extension, time, and more.

### Step 3: Recovering file from Hex 
### Step 3: Recovering a File from Hex

Now that you've found an interesting file, you may want to extract it. Here’s how you can do that:

Note: As mentioned earlier, each MFT record is 1024 bytes in size. If a file on disk is smaller than 1024 bytes, it can be recovered directly from the MFT.

1. **Find the Entry Number**: Identify the entry number of the file you want to recover.
2. **Calculate the Hex Offset of the File**:
    - Multiply the entry number by 1024.
    - Example: For entry number 23436, the calculation is 1024 x 23436 = 23,998,464 (decimal) or 0x16E3000 (hex offset).
3. **Use HxD to Extract the File**:
    - Download and install [HxD](https://mh-nexus.de/en/downloads.php?product=HxD20).
    - Open the MFT table in HxD.
    - Press `Ctrl + G` (or `Win + G`) to go to the specified offset.
    - Export the content of the file from this offset.
![[Pasted image 20240610143555.png]]