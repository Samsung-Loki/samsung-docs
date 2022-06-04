---
layout: default
title: Handshake
description: Odin Handshake
parent: Odin
nav_order: 3
---

## Handshakes
Before any upload or download can take place, a handshake is done.
<h3>ODIN <p class="label label-yellow">Old knowledge</p></h3>
Write: `ODIN` \
Read: `LOKE`

<h3>FPGM <p class="label label-green">Reverse-Engineered</p></h3>
Does the same thing as the usual ODIN handshake. \
Write: `FPGM` \
Read: `OK`

<h3>THOR <p class="label label-green">Reverse-Engineered</p></h3>
It used by the Thor flashing software (newer Odin). \
Does the same as the usual `ODIN` handshake. \
Write: `THOR` \
Read: `LOKE`