---
layout: default
title: PIT
description: Samsung Partition Table Format
nav_order: 4
---

# Warning
The PIT format is not yet fully documented and most of it's parts are unknown. \
Also things vary from device to device.

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
| 32-bit integer    | Block size (usually 512 bytes)            |
| 32-bit integer    | Count of blocks                           |
| 32-bit integer    | File Offset (also seems to be obsolete)    |
| 32-bit integer    | File Size (is obsolete, nowadays is zero) |
| String, 8 bytes   | Partition Name                            |
| String, 8 bytes   | File Name                                 |
| String, 8 bytes   | FOTA Name (only for 'remainder')          |

### Binary Types
* AP/Phone = 0
* CP/Modem = 1

### Device Types
* NAND = 1
* EMMC = 2
* SPI = 3
* IDE = 4
* NANDX16 = 5
* NOR = 6
* NANDWB1 = 7
* UFS = 8

### Attributes
* Read-Only = 0
* Read-Write = 1
* STL = 2

### Update Attributes
* None = 0
* FOTA = 1
* Secure = 2
* FOTA Secure = 3
