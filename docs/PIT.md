---
layout: default
title: PIT
description: Partition Information Table
nav_order: 4
---

## Header

| Value        | Argument Type     | Information                   |
|:-------------|:------------------|:------------------------------|
| 0x12349876   | 32-bit integer    | Magic Number                  |
| dynamic      | 32-bit integer    | Count of partitions           |
| COM_TAR2     | String, 8 bytes   | GANG name                     |
| dynamic      | String, 8 bytes   | Project Name                  |
| dynamic      | 32-bit integer    | Reserved                      |

## Entries
PIT is V1 if the block sizes are equal, otherwise it's start block for V2. \
Entries begin after first 28 bytes, one entry is 132 bytes long.

| Argument Type     | Information                               |
|:------------------|:------------------------------------------|
| 32-bit integer    | Binary Type (more info below)             |
| 32-bit integer    | Device Type (more info below)             |
| 32-bit integer    | Partition Identifier                      |
| 32-bit integer    | Attributes (v1) or Partition Type (v2)    |
| 32-bit integer    | Update Attibutes (v1) or File System (v2) |
| 32-bit integer    | Block Size (v1) or Start Block (v2)       |
| 32-bit integer    | Block Count (v1) or Block Number (v2)     |
| 32-bit integer    | File Offset (also seems to be obsolete)   |
| 32-bit integer    | File Size (is obsolete, nowadays is zero) |
| String, 32 bytes  | Partition Name                            |
| String, 32 bytes  | File Name                                 |
| String, 32 bytes  | Delta (FOTA) Name (only for 'remained')   |

### Binary Types
* AP/Phone = 0
* CP/Modem = 1

## Version 1

#### Device Types
* ONENAND = 0
* NAND = 1
* MOVINAND = 2

#### Attributes
* Read-Only = 0
* Read-Write = 1
* STL = 2

#### Update Attributes
* None = 0
* FOTA = 1
* Secure = 2
* FOTA Secure = 3

### Version 2

#### Device Types
* ONENAND = 0
* NAND = 1
* EMMC = 2
* SPI = 3
* IDE = 4
* NAND X16 = 5

#### Partition Type
* NONE = 0
* BCT = 1
* BOOTLOADER = 2
* PARTITION TABLE = 3
* NVDATA = 4
* DATA = 5
* MBR = 6
* EBR = 7
* GP1 = 8
* GP1 = 9

#### Filesystem
* NONE = 0
* BASIC = 1
* ENHANCED = 2
* EXT2 = 3
* YAFFS2 = 4
* EXT4 = 5

## ImHex pattern
Use this to analyze PIT files in ImHex (we still recommend using Thor for that)
```cpp
#pragma description Samsung PIT file

enum BinaryType : u32 {
    AP_Phone = 0,
    CP_Modem = 1
};

enum DeviceTypeV1 : u32 {
    ONENAND = 0,
    NAND = 1,
    MOVINAND = 2
};

enum DeviceTypeV2 : u32 {
    ONENAND = 0,
    NAND = 1,
    EMMC = 2,
    SPI = 3,
    IDE = 4,
    NAND_X16 = 5
};

enum Attributes : u32 {
    ReadOnly = 0,
    ReadWrite = 1,
    STL = 2
};

enum UpdateAttributes : u32 {
    None = 0,
    FOTA = 1,
    Secure = 2,
    FOTA_Secure = 3
};

enum PartitionType : u32 {
    None = 0,
    BCT = 1,
    Bootloader = 2,
    PartitionTable = 3,
    NVData = 4,
    Data = 5,
    MBR = 6,
    EBR = 7,
    GP1 = 8,
    GP1 = 9,
};

enum Filesystem : u32 {
    None = 0,
    Basic = 1,
    Enhanced = 2,
    EXT2 = 3,
    YAFFS2 = 4,
    EXT4 = 5
};

struct partition_v1_t {
    DeviceTypeV1 device_type [[name("Device Type")]];
    u32 id [[name("Partition Identifier")]];
    Attributes attributes [[name("Attributes")]];
    UpdateAttributes update [[name("Update Attributes")]];
    u32 block_size [[name("Block Size")]];
    u32 block_count [[name("Block Count")]];
    u32 file_offset [[name("File Offset (obsolete)")]];
    u32 file_size [[name("File Size (obsolete)")]];
    char name[32] [[name("Partition Name")]];
    char filename[32] [[name("File Name")]];
    char delta[32] [[name("Delta (FOTA) name")]];
};

struct partition_v2_t {
    DeviceTypeV2 device_type [[name("Device Type")]];
    u32 id [[name("Partition Identifier")]];
    PartitionType part_type [[name("Partition Type")]];
    Filesystem filesystem [[name("File System")]];
    u32 start_block [[name("Start Block")]];
    u32 block_number [[name("Block Number")]];
    u32 file_offset [[name("File Offset (obsolete)")]];
    u32 file_size [[name("File Size (obsolete)")]];
    char name[32] [[name("Partition Name")]];
    char filename[32] [[name("File Name")]];
    char delta[32] [[name("Delta (FOTA) name")]];
};

struct partition_t {
    BinaryType binary_type [[name("Binary Type")]];

    if (isVersion2) partition_v2_t partition [[name("Partition V2")]];
    else partition_v1_t partition [[name("Partition V1")]];
};

struct signer_info_t {
	char SignSystemRevision[16] [[name("Signing System Revision")]];
	char QuickBuildId[16] [[name("Quick Build ID")]];
	char VersionName[32] [[name("Firmware Version")]];
	char BuildTime[16] [[name("Build Timestamp")]];
	char ModelName[32] [[name("Model Name")]];
	char SystemRPValue[16] [[name("System Secureboot Magic")]];
	char KernelRPValue[16] [[name("Kernel Secureboot Magic")]];
	char BuildVariant[4]  [[name("Engineering Build"), comment("usr = user, eng = engineering")]];
	char KillSwitchMagic[4] [[name("Killswitch Type"), comment("frp = factory reset protection, ral = reactivation lock")]];
	char FactoryBuild[4] [[name("Factory Build"), comment("fac = factory, mrk = user")]];
	char BinaryName[16] [[name("Binary Name")]];
	char Reserved[84] [[name("Reserved")]];
};

struct signature_t {
    signer_info_t info [[name("Signer Info")]];
    char signature[256] [[name("ECDSA Signature")]];
};

struct pit_t {
    u32 magic [[name("Magic Number")]];
    u32 count [[name("Number of partitions")]];
    char gang_name[8] [[name("GANG name")]];
    char project_name[8] [[name("Project Name")]];
    u32 reserved [[name("Reserved")]];
    partition_t partitions[count] [[name("Partitions")]];
    if (builtin::std::mem::size() > builtin::std::mem::base_address())
        signature_t signature [[name("Signature")]];
};


bool isVersion2 in;
pit_t file @ 0x00 [[name("PIT file")]];
```
