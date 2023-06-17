---
layout: default
title: Commands
description: Pre-handshake commands
parent: Odin
nav_order: 2
---

## Rooting
It is unknown what it does. \
Write: `ROOTING` \
Read: `DDI`

## ATQ0
It is unknown what it does. \
Write: `ATQ0` \
Read: `OKAY`

## RESET
Shuts down the device. \
Write: `RESET` \
Read: `+RESET: OK\n`

## PROMPT
Write: `PROMPT<command>` \
Read: `<response>`

### Commands
#### Get environment data
This is not an actual, but "virtual" command that is only usable by using the PROMT.
Would return all environment variables, stated below. \
Command format: `getenv`. No arguments required. \
Response format: `[Name]: [Value]\n` for each.
#### Set reboot mode
Command format: `setenv REBOOT_MODE [value]` \
Response format: `Unknown`
##### Reboot values

| Name                          | Value | Information               |
|:------------------------------|:------|:--------------------------|
| REBOOT_MODE_NONE              | 0x00  | Model's Name              |
| REBOOT_MODE_DOWNLOAD          | 0x01  | Serial Code               |
| REBOOT_MODE_UPLOAD            | 0x02  | Used, purpose unknown     |
| REBOOT_MODE_CHARGING          | 0x03  | Unused?                   |
| REBOOT_MODE_FOTA              | 0x04  | FOTA Updating Process     |
| REBOOT_MODE_FOTA_BL           | 0x05  | BOTA Update               |
| REBOOT_MODE_SECURE            | 0x06  | Modem Secure Error        |
| REBOOT_MODE_NORMAL            | 0x07  | Default Reboot Mode       |
| REBOOT_MODE_FWUP              | 0x08  | Emergency Firmware Update |
| REBOOT_MODE_EM_FUSE           | 0x09  | Unused?                   |
| REBOOT_MODE_FACTORY_MD        | 0xXA  | Unused?                   |
| REBOOT_MODE_FOTA_UP           | 0x0B  | FOTA Setting Up           |
| REBOOT_MODE_BOOTLOADER        | 0xXC  | Download (Odin) Mode      |
| REBOOT_MODE_WIRELESSD_BL      | 0xXD  | Unused?                   |
| REBOOT_MODE_RECOVERY_WD       | 0x0E  | Skip AVB Main             |
| REBOOT_MODE_FACTORY           | 0x0F  | Samsung Factory Mode      |
| REBOOT_MODE_POWEROFF_WATCH    | 0xFD  | Unused?                   |
| REBOOT_MODE_WATCH_REBOOT_MODE | 0x10  | Unused?                   |
| REBOOT_MODE_CHARGING          | 0x11  | Unused?                   |
| REBOOT_MODE_POWEROFF_BYKEY    | 0x12  | Unused?                   |

#### Enable/disable upload
Used mainly to do ramdumps, the way is unknown. \
Command format: `setenv FORCE_UPLOAD [value]` \
Response format: `Unknown`

##### Values

| Name                            | Value |
|:--------------------------------|:------|
| KERNEL_SEC_FORCE_UPLOAD_DISABLE | 0x00  |
| KERNEL_SEC_FORCE_UPLOAD_ENABLE  | 0x05  |

#### Set debug level
Command format: `setenv DEBUG_LEVEL [value]` \
Response format: `Unknown`

##### Debug Levels

| Name                        | Value  |
|:----------------------------|:-------|
| KERNEL_SEC_DEBUG_LEVEL_LOW  | 0x4F4C |
| KERNEL_SEC_DEBUG_LEVEL_MID  | 0x494D |
| KERNEL_SEC_DEBUG_LEVEL_HIGH | 0x4948 |
| KERNEL_SEC_DEBUG_LEVEL_AUTO | 0x5541 |

#### Set default cmdline
Command format: `setenv CMDLINE [value]` \
Tabs and quotes are not allowed. \
Response format: `Unknown`

##### Values observed

| Name            | Value                                                                |
|:----------------|:---------------------------------------------------------------------|
| Default Value   | `console=ram loglevel=7`                                             |
| Non-Exynos UART | `console=ttySAC0,115200n8 loglevel=7`                                |
| Exynos UART     | `earlycon=exynos4210,0x10540000 console=ttySAC0,115200n8 loglevel=7` |

#### Set power margin
Command format: `setenv POWER_MARGIN [value]` \
Values are unknown (not found in bootloader)

#### Save environment data
Response format: `Unknown` \
Command format: `saveenv`

#### Restart the device
Response format: `Unknown` \
Command format: `reset`

## SECCMD
Commands:
```
0x01 - Forcefully set the warranty bit
0x02 - Clear EMC token
```
Write: `SECCMD<command>` \
Read: `<response>`

## DVIF
Write: `DVIF` \
Read: 
```
General: @#MODEL=<Product Name>;VENDOR=<Chip Vendor>;FWVER=<Revision>;SALES=<Region Code>;VER=<Build Version>;DID=<DID>;RAND=<Base64 Encoded random string>;TMU_TEMP=<CPU Temperature>@#
For UFS: @#UN=<UFS ID>;CAPA=<UFS Size>;PRODUCT=<UFS name>@#
For MMC: @#UN=<MMC ID>;CAPA=<GB Size>PRODUCT=<MMC name>@#
```
