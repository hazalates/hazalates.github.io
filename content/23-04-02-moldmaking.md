---
title: "Moldmaking"
date: 2023-04-02T09:51:46+02:00
draft: false
tags: ['exterior']
toc: true
---

## Mycelium update

Last time i was in the biolab my mycelium baby's were growing nicely. Both my pot with spawn and my substrates are mature enough to use in a prototype. Only my small test piece, made from another, older, batch of substrate didn't grow well. Probably because the substrate is too old to be used. 

{{< rawhtml >}}
<div class="row">
  <div class="column-2">
    <img src="/images/23-04-02-moldmaking/moldmaking2.jpeg" alt="substrate" style="width:100%">
  </div>
  <div class="column-2">
    <img src="/images/23-04-02-moldmaking/moldmaking3.jpeg" alt="failedtest" style="width:100%">
  </div>
</div>
{{< /rawhtml >}}

I can now use my bags of substrate to make a prototype for the exterior.

## Making a mold

But before i can make a prototype I'll need a mold for the mycelium to grow in. I have a few thoughts on how to do this. 

First i have to make a prototype of the exterior from another material, such as wood. Than, i'll take this positive and use a plastic former to make a negative. Than i'll drill holes in the negative to give the mycelium room to breathe. 

To make the positive, i'll design the exterior in Fusion360. It would be nice to use the mold for both the box and the lid, instead of two separate molds. I talked with Ronald and he told me this is probably called point- or mirror symmetrical.

## Designing the positive in Fusion360

I recycled my design of a box i made a while back. The design is parametric, thus easily changeable. I added an extra panel on each side with a lower and higher height. 

{{< rawhtml >}}
<div class="row">
  <div class="column-3">
    <img src="/images/23-04-02-moldmaking/moldmaking5.jpeg" alt="boxv1" style="width:100%">
  </div>
  <div class="column-3">
    <img src="/images/23-04-02-moldmaking/moldmaking4.jpeg" alt="boxv2" style="width:100%">
  </div>
  <div class="column-3">
    <img src="/images/23-04-02-moldmaking/moldmaking8.jpeg" alt="parametricvalues" style="width:100%">
  </div>
</div>
{{< /rawhtml >}}

For the size i'd like to use the standard size of kitchen "gastronome container". This is standard equipment in professional kitchens but also easy to come by as a amateur cook. There are also perforated gastronome containers, perfect for making miso, since miso needs oxygen flow, also at its bottom. I choose the 1/2 size, because the 1/1 sized containers are quite big and the 1/2 is close to the side i initially wanted to use (240mm x 360mm). I also added 5mm extra on each side for some allowance.

{{< rawhtml >}}
<div class="row">
  <div class="column-2">
    <img src="/images/23-04-02-moldmaking/moldmaking06.jpeg" alt="gastronomesizes" style="width:100%">
  </div>
  <div class="column-2">
    <img src="/images/23-04-02-moldmaking/moldmaking7.jpeg" alt="gastronomecontainer" style="width:100%">
  </div>
</div>
{{< /rawhtml >}}

Next steps in making the positive: 

- Find a nice material
- Measure material thickness
- Enter material thickness in Fusion design
- Prepare DXF files
    - New sketch -> Select face -> create DXF from sketch
- Mill all the faces
- Glue them together

## Building the positive

I found a nice material of 12mm thick. I changed the material thickness in Fusion360 and exported each panel as a DXF file. Next I opened the files in VCarve on the Shopbot computer (the computer connected to the CNC). 

Then i set up the CNC machine. I used a 5mm 2 flute end mill because the milling doesn't need to be extremely precise. I added -.5mm allowance to make sure the panels wouldn't be to tight.

{{< rawhtml >}}
<video width="320" height="240" controls>
  <source src="/images/23-04-02-moldmaking/moldmaking01.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
{{< /rawhtml >}}

{{< rawhtml >}}
<div class="row">
  <div class="column-3">
    <img src="/images/23-04-02-moldmaking/moldmaking12.jpeg" alt="boxv1" style="width:100%">
  </div>
  <div class="column-3">
    <img src="/images/23-04-02-moldmaking/moldmaking13.jpeg" alt="boxv2" style="width:100%">
  </div>
  <div class="column-3">
    <img src="/images/23-04-02-moldmaking/moldmaking14.jpeg" alt="parametricvalues" style="width:100%">
  </div>
</div>
{{< /rawhtml >}}

The milling went well and i'm happy with the result. 

{{< rawhtml >}}
<div class="row">
  <div class="column-2">
    <img src="/images/23-04-02-moldmaking/moldmaking9.jpeg" alt="gastronomesizes" style="width:100%">
  </div>
  <div class="column-2">
    <img src="/images/23-04-02-moldmaking/moldmaking10.jpeg" alt="gastronomecontainer" style="width:100%">
  </div>
</div>
{{< /rawhtml >}}