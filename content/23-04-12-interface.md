---
title: "Interface"
date: 2023-04-12T11:03:25+02:00
draft: false
tags: ['electronics']
toc: true
---
<style>
* {
  box-sizing: border-box;
}

.column {
  float: left;
  width: 33.33%;
  padding: 5px;
}

/* Clearfix (clear floats) */
.row::after {
  content: "";
  clear: both;
  display: table;
}
</style>

## Components

What I need:

    - LCD 

    - Button(s) -> Rotary Encoder

    - Speaker

    - LED indicator

    - Plug to connect to main board

    - Housing

## Prototype #1 (without LED)

The prototype exists of two PCB's: one "main" board, and one board for the rotary encoder.

The housing of the prototype is made out of the main exterior and a knob.

### Main board

![interface_main_schema](/images/23-04-12-interface/interface04.jpeg)

{{< rawhtml >}}
<div class="row">
  <div class="column-2">
    <img src="/images/23-04-12-interface/interface05.jpeg" alt="interface_main_PCB" style="width:100%">
  </div>
  <div class="column-2">
    <img src="/images/23-04-12-interface/interface01.jpeg" alt="interface_main_3D" style="width:100%">
  </div>
</div>
{{< /rawhtml >}}

### Rotary encoder board

![interface_RE_schema](/images/23-04-12-interface/interface06.jpeg)

{{< rawhtml >}}
<div class="row">
  <div class="column-2">
    <img src="/images/23-04-12-interface/interface07.jpeg" alt="interface_RE_PCB" style="width:100%">
  </div>
  <div class="column-2">
    <img src="/images/23-04-12-interface/interface02.jpeg" alt="interface_RE_3D" style="width:100%">
  </div>
</div>
{{< /rawhtml >}}

### Casing

3D render of the exterior:

![interface_main](/images/23-04-12-interface/interface03.jpeg)

### Final product

{{< rawhtml >}}
<video width="320" height="240" controls>
  <source src="/images/23-04-12-interface/interface.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
{{< /rawhtml >}}






