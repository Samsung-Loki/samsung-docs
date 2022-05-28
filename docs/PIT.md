---
layout: default
title: PIT
description: Samsung Partition Table Format
nav_order: 4
---

## Header

| Value        | Argument Type     | Information                   |
|:-------------|:------------------|:------------------------------|
| 0x12349876   | 32-bit integer    | Magic Number                  |
| dynamic      | 32-bit integer    | Count of partitions           |
| COM_TAR2     | String, 8 bytes   | GANG Name                     |
| dynamic      | String, 8 bytes   | Project Name                  |
| dynamic      | 32-bit integer    | Protocol Version or LUN count |
| dynamic      | Array of entries  | Self-explanatory              |

## Entries
Entries begin after first 28 bytes. \
One entry is 132 bytes long.

| Argument Type     | Information                               |
|:------------------|:------------------------------------------|
| 32-bit integer    | Binary Type (more info below)             |
| 32-bit integer    | Device Type (more info below)             |
| 32-bit integer    | Partition Identifier                       |
| 32-bit integer    | Attributes (flags)                         |
| 32-bit integer    | Update attibutes (flags)                   |
| 32-bit integer    | Block size or Start Block                 |
| 32-bit integer    | Block Count or Block Number               |
| 32-bit integer    | File Offset (also seems to be obsolete)    |
| 32-bit integer    | File Size (is obsolete, nowadays is zero) |
| String, 32 bytes  | Partition Name                            |
| String, 32 bytes  | File Name                                 |
| String, 32 bytes  | Delta (FOTA) Name (only for 'remained')   |

### Binary Types
* AP/Phone = 0
* CP/Modem = 1

### Version 1
Old PIT version, the way to detect is not very accurate. \
It is v2 if all block sizes are the same, v1 otherwise.

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
New PIT version, the way to detect is not very accurate. \
It is v2 if all block sizes are the same, v1 otherwise.

#### Device Types
* ONENAND = 0
* NAND = 1
* EMMC = 2
* SPI = 3
* IDE = 4
* NAND X16 = 5

#### Attributes
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

#### Update Attributes
* NONE = 0
* BASIC = 1
* ENHANCED = 2
* EXT2 = 3
* YAFFS2 = 4
* EXT4 = 5
