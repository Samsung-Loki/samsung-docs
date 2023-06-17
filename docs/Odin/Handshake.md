---
layout: default
title: Handshake
description: Odin Handshake
parent: Odin
nav_order: 3
---

## Handshakes
Before any upload or download can take place, a handshake is done.

### ODIN
Write: `ODIN` \
Read: `LOKE`

### FPGM
Does the same thing as the usual ODIN handshake. \
Write: `FPGM` \
Read: `OK`

### THOR
It used by the Thor flashing software (newer Odin). \
Does the same as the usual `ODIN` handshake. \
Write: `THOR` \
Read: `LOKE`
