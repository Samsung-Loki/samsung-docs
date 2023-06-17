---
layout: default
title: Device Information
description: Information about the device
parent: Odin
nav_order: 1
---

## Format
### Header

| Argument Type      | Value      | Information           |
|:-------------------|:-----------|:----------------------|
| Magic Number       | 0x12345678 | Self-explanatory      |
| 32-bit integer     | dynamic    | Count of items        |
| Array of Location  | dynamic    | Self-explanatory      |
| Array of Data      | dynamic    | Self-explanatory      |

### Location Structure

| Argument Type     | Information           |
|:------------------|:----------------------|
| 32-bit integer    | DevInfo type (below)  |
| 32-bit integer    | Offset, in bytes      |
| 32-bit integer    | Size, in bytes        |

### Data Structure

| Argument Type     | Information           |
|:------------------|:----------------------|
| 32-bit integer    | DevInfo type (below)  |
| 32-bit integer    | Size, in bytes        |
| Raw byte buffer   | DevInfo data          |

### DevInfo Types

| Name                      | Value | Information  |
|:--------------------------|:------|:-------------|
| DEVINFO_TYPE_MODEL_NAME   | 0x00  | Model's Name |
| DEVINFO_TYPE_SERIAL       | 0x01  | Serial Code  |
| DEVINFO_TYPE_OMCSALESCODE | 0x02  | Region Code  |
| DEVINFO_TYPE_CARRIERID    | 0x03  | Carrier ID   |

## Packets
### Dump Device Info
Request:

| Value        | Argument Type     | Information        |
|:-------------|:------------------|:-------------------|
| 0x69         | 32-bit integer    | Packet type        |
| 0x00         | 32-bit integer    | Packet's command   |

Response:

| Value        | Argument Type     | Information                                    |
|:-------------|:------------------|:-----------------------------------------------|
| 0x69         | 32-bit integer    | Packet type, would be 0xFF on failure          |
| dynamic      | 32-bit integer    | Size of the DevInfo. Usually it is 500 bytes   |

### Dump a block
A block is 500 bytes. \
Request:

| Value        | Argument Type     | Information        |
|:-------------|:------------------|:-------------------|
| 0x69         | 32-bit integer    | Packet type        |
| 0x01         | 32-bit integer    | Packet's command   |
| dynamic      | 32-bit integer    | Block Index        |

Response:

| Value        | Argument Type     | Information          |
|:-------------|:------------------|:---------------------|
| dynamic      | Raw byte buffer   | Block's data buffer  |

### End dump
Request:

| Value        | Argument Type     | Information        |
|:-------------|:------------------|:-------------------|
| 0x69         | 32-bit integer    | Packet type        |
| 0x02         | 32-bit integer    | Packet's command   |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x69         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |
