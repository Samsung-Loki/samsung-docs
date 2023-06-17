---
layout: default
title: PIT
description: PIT flashing and dumping 
parent: Odin
nav_order: 6
---

## How-to guide
### Flash
1) Send Request PIT flash \
2) Send Begin PIT flash \
3) Send the entire PIT in one go \
4) Send End PIT flash
### Dump
1) Send Begin PIT dump \
2) Split the filesize into blocks \
3) Send Dump PIT data block
* Repeat until end

5) Send End PIT dump

## Packets
### Flashing
#### Request PIT flash
Request:

| Value        | Argument Type     | Information                 |
|:-------------|:------------------|:----------------------------|
| 0x65         | 32-bit integer    | Packet type                 |
| 0x00         | 32-bit integer    | Packet's command            |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x65         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

#### Begin PIT flash
Request:

| Value        | Argument Type     | Information                 |
|:-------------|:------------------|:----------------------------|
| 0x65         | 32-bit integer    | Packet type                 |
| 0x02         | 32-bit integer    | Packet's command            |
| dynamic      | 32-bit integer    | PIT size, in bytes          |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x65         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

#### Send PIT data
Request:

| Value        | Argument Type     | Information                 |
|:-------------|:------------------|:----------------------------|
| dynamic      | Raw byte buffer   | PIT file data buffer        |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x65         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

#### End PIT Flash
Request:

| Value        | Argument Type     | Information                 |
|:-------------|:------------------|:----------------------------|
| 0x65         | 32-bit integer    | Packet type                 |
| 0x03         | 32-bit integer    | Packet's command            |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x65         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

### Dumping
#### Request PIT data dump
Request:

| Value        | Argument Type     | Information                 |
|:-------------|:------------------|:----------------------------|
| 0x65         | 32-bit integer    | Packet type                 |
| 0x01         | 32-bit integer    | Packet's command            |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x65         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| dynamic      | 32-bit integer    | PIT file size. Ususally it is 0x4000        |

#### Dump PIT data block
One block is 500 bytes. Send an empty packet after last block. \
Request:

| Value        | Argument Type     | Information                 |
|:-------------|:------------------|:----------------------------|
| 0x65         | 32-bit integer    | Packet type                 |
| 0x02         | 32-bit integer    | Packet's command            |
| dynamic      | 32-bit integer    | Current block index         |

Response:

| Value        | Argument Type     | Information                       |
|:-------------|:------------------|:----------------------------------|
| dynamic      | Raw byte buffer   | The requested block's data buffer |

#### End PIT dump
Identical to End PIT flash. \
Request:

| Value        | Argument Type     | Information                 |
|:-------------|:------------------|:----------------------------|
| 0x65         | 32-bit integer    | Packet type                 |
| 0x03         | 32-bit integer    | Packet's command            |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x65         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero                 |
