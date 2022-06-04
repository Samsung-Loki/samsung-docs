---
layout: default
title: Device Information
description: Information about the device
parent: Odin
nav_order: 1
---

## Format
<h3>Header <p style="font-size: 14px !important;" class="label label-red">From leak</p></h3>

| Argument Type      | Value      | Information           |
|:-------------------|:-----------|:----------------------|
| Magic Number       | 0x12345678 | Self-explanatory      |
| 32-bit integer     | dynamic    | Count of items        |
| Array of Location  | dynamic    | Self-explanatory      |
| Array of Data      | dynamic    | Self-explanatory      |

<h3>Location Structure <p style="font-size: 14px !important;" class="label label-red">From leak</p></h3>

| Argument Type     | Information           |
|:------------------|:----------------------|
| 32-bit integer    | DevInfo type (below)  |
| 32-bit integer    | Offset, in bytes      |
| 32-bit integer    | Size, in bytes        |

<h3>Data Structure <p style="font-size: 14px !important;" class="label label-red">From leak</p></h3>

| Argument Type     | Information           |
|:------------------|:----------------------|
| 32-bit integer    | DevInfo type (below)  |
| 32-bit integer    | Size, in bytes        |
| Raw byte buffer   | DevInfo data          |

<h3>DevInfo Types <p style="font-size: 14px !important;" class="label label-red">From leak</p></h3>

| Name                      | Value | Information  |
|:--------------------------|:------|:-------------|
| DEVINFO_TYPE_MODEL_NAME   | 0x00  | Model's Name |
| DEVINFO_TYPE_SERIAL       | 0x01  | Serial Code  |
| DEVINFO_TYPE_OMCSALESCODE | 0x02  | Region Code  |
| DEVINFO_TYPE_CARRIERID    | 0x03  | Carrier ID   |


## Packets

<h3>Dump Device Info <p style="font-size: 14px !important;" class="label label-red">From leak</p></h3>
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

<h3>Dump a block <p style="font-size: 14px !important;" class="label label-red">From leak</p></h3>
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

<h3>End dump <p style="font-size: 14px !important;" class="label label-red">From leak</p></h3>
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
