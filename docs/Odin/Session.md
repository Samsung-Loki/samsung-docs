---
layout: default
title: Session
description: Manage the Session
parent: Odin
nav_order: 5
---

## Packets
### Begin Session
Request:

| Value        | Argument Type     | Information                                          |
|:-------------|:------------------|:-----------------------------------------------------|
| 0x64         | 32-bit integer    | Packet type                                          |
| 0x00         | 32-bit integer    | Packet's command                                     |
| dynamic      | 32-bit integer    | Protocol version (Known: 0x00, 0x03, 0x04 and 0x05)  |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x64         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| dynamic      | 32-bit integer    | The protocol version, modified               |

The modified version would be:
* If the `<Protocol Version>` is 0, would return `0x20000`
* If the `<Protocol Version>` is lower than bootloader's,
    * it would return `(<Protocol Version> << 16) | 0x0`
* In other cases it would return:
    * `(<Bootloader Protocol Version> << 16) | 0x0`

(In a nutshell, `4` would be `0x40000`)

Devices that do not support changing the packet size, send zero instead. \
Consider it as a worst case scenario - Odin protocol version 1. \
If it's not the case, we can safely change the values:
* Read/Write timeout for file transfer: 120000 (2 minutes)
* Packet size for file transfer: 1048576 (1 MiB)
* File transfer max sequence size: 30 (Packets)

Also, you can send raw LZ4 compressed data to the phone!
If it is supported, it would return 0x4**8**000 with the example above.
Basically, it is a bitwise-or with `0x8000`.

### Flash count reset
Request:

| Value        | Argument Type     | Information                |
|:-------------|:------------------|:---------------------------|
| 0x64         | 32-bit integer    | Packet type                |
| 0x01         | 32-bit integer    | Packet's command           |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x64         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| dynamic      | 32-bit integer    | Status code                                 |

### Set Total Bytes
Request:

| Value        | Argument Type           | Information                |
|:-------------|:------------------------|:---------------------------|
| 0x64         | 32-bit integer          | Packet type                |
| 0x02         | 32-bit integer          | Packet's command           |
| dynamic      | 64-bit integer          | Total length, in bytes     |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x64         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

### Set File Part size
Request:

| Value        | Argument Type     | Information                |
|:-------------|:------------------|:---------------------------|
| 0x64         | 32-bit integer    | Packet type                |
| 0x05         | 32-bit integer    | Packet's command           |
| dynamic      | 32-bit integer    | File part size (in bytes)  |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x64         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

### Erase userdata partition
**WARNING!** This will do a factory reset. \
Request:

| Value        | Argument Type     | Information                |
|:-------------|:------------------|:---------------------------|
| 0x64         | 32-bit integer    | Packet type                |
| 0x07         | 32-bit integer    | Packet's command           |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x64         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| dynamic      | 32-bit integer    | Status code of the Erase function           |

### Enable T-Flash
Request:

| Value        | Argument Type     | Information                |
|:-------------|:------------------|:---------------------------|
| 0x64         | 32-bit integer    | Packet type                |
| 0x08         | 32-bit integer    | Packet's command           |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x64         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

### Change device's region code
Request:

| Value        | Argument Type     | Information                |
|:-------------|:------------------|:---------------------------|
| 0x64         | 32-bit integer    | Packet type                |
| 0x09         | 32-bit integer    | Packet's command           |
| dynamic      | String, 3 bytes   | Region Code                |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x64         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

## End Session (0x67)
### End Session
Request:

| Value        | Argument Type     | Information                |
|:-------------|:------------------|:---------------------------|
| 0x67         | 32-bit integer    | Packet type                |
| 0x00         | 32-bit integer    | Packet's command           |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x67         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

### Reboot
Request:

| Value        | Argument Type     | Information                |
|:-------------|:------------------|:---------------------------|
| 0x67         | 32-bit integer    | Packet type                |
| 0x01         | 32-bit integer    | Packet's command           |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x67         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

### Reboot into Odin
Reboots and sends you back into download mode. \
Request:

| Value        | Argument Type     | Information                |
|:-------------|:------------------|:---------------------------|
| 0x67         | 32-bit integer    | Packet type                |
| 0x02         | 32-bit integer    | Packet's command           |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x67         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |

### Shutdown
This command doesn't work on some devices. \
Request:

| Value        | Argument Type     | Information                |
|:-------------|:------------------|:---------------------------|
| 0x67         | 32-bit integer    | Packet type                |
| 0x03         | 32-bit integer    | Packet's command           |

Response:

| Value        | Argument Type     | Information                                 |
|:-------------|:------------------|:--------------------------------------------|
| 0x67         | 32-bit integer    | Packet type, would be 0xFF on failure       |
| 0x00         | 32-bit integer    | Status code. Is always zero.                |
