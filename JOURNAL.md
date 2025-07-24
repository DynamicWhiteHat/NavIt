---
title: "NavIt!"
author: "DynamicWhiteHat"
description: "An ESP-32 Powered Bicycle Navigation Display"
created_at: "2025-07-17"
---


## JUNE 17th: Selected Parts And Worked On PCB

To start, I decided to select a powerful MCU with wireless connectivity, as I wanted to show map information from both an SD card and from an API. I chose the ESP-32 S3 Wroom since it has lots of GPIO pins and 
lots of PSRAM. It was my first time designing an ESP-32 based board, so I followed [this](https://www.instructables.com/Build-Custom-ESP32-Boards-From-Scratch-the-Complet/) tutorial, which served as a good starting
point for my design. This is what I had after I finished the tutorial:

| ESP-32 | USB | BOOT & RESET |
|--------|---------|---------|
| <img width="680" height="493" alt="image" src="https://github.com/user-attachments/assets/47ce4ffd-d84a-4518-b726-ecf3db93e929" /> | <img width="451" height="259" alt="image" src="https://github.com/user-attachments/assets/1ba60c97-79e7-41de-b9db-c6a76733708d" /> | <img width="229" height="260" alt="image" src="https://github.com/user-attachments/assets/ace614c3-d7cf-4e42-b578-9c02a41a1dd8" /> |

After that, I looked on AliExpress for a good and cheap eink display. I found [this](https://www.aliexpress.us/item/3256807363354164.html?pdp_npi=4%40dis%21USD%21US%20%2426.37%21US%20%2426.37%21%21%2126.37%2126.37%21%402101ec1f17507091063604232ed362%2112000045639522238%21sh%21US%216238534509%21X&spm=a2g0o.store_pc_allItems_or_groupList.new_all_items_2007445615883.1005007549668916&gatewayAdapt=glo2usa)
eink from Goodisplay that was $24 and used ffc cables for connection. This display also has touchscreen. However, I do need to buy an [adapter](https://www.aliexpress.us/item/3256804446769469.html?spm=a2g0o.detail.pcDetailTopMoreOtherSeller.3.77354F1V4F1Vs7&gps-id=pcDetailTopMoreOtherSeller&scm=1007.40050.354490.0&scm_id=1007.40050.354490.0&scm-url=1007.40050.354490.0&pvid=4d04d64b-d86e-4113-bec2-9f569b87db08&_t=gps-id:pcDetailTopMoreOtherSeller,scm-url:1007.40050.354490.0,pvid:4d04d64b-d86e-4113-bec2-9f569b87db08,tpp_buckets:668%232846%238108%231977&pdp_ext_f=%7B%22order%22%3A%22277%22%2C%22eval%22%3A%221%22%2C%22sceneId%22%3A%2230050%22%7D&pdp_npi=4%40dis%21USD%2111.03%210.99%21%21%2111.03%210.99%21%402103201917507107117687867eb072%2112000029911597842%21rec%21US%216238534509%21ABXZ&utparam-url=scene%3ApcDetailTopMoreOtherSeller%7Cquery_from%3A)
that allows the ESP-32 to connect to the display using header pins. I found a footprint online and added this for the display:

<img width="453" height="524" alt="image" src="https://github.com/user-attachments/assets/3fcc81ec-0b7f-43ca-855a-0389fec0f728" />

Next, I added in a MEMS I2S microphone using a custom footprint I designed and found a NEO-6M GPS for cheap on AliExpress and added that too. The GPS will allow me to add map functionality using IceNav, a library for ESP-32 based navigation. I can also get the current time, date, and calculate speed.
Wiring those to the ESP-32 looks like this:

<img width="393" height="401" alt="image" src="https://github.com/user-attachments/assets/262d4fb7-353e-4cd4-ba35-6168ea64c9d1" />

Finally, I attempted to add a speaker to the ESP-32. One major problem that I had a difficult time with was that the ESP-32 doesn't have a built in DAC, so I would need to add a DAC before sending audio data to an amp.
I initially decided to use a PCM5102A for DAC and a PAM8403D amp, but I just couldn't figure out how to wire the DAC, especially since it had analog gnd and digital gnd signals and multiple different setting pins.
I am also new to I2S communication, so that also didn't make much sense. This was as far as I got before I gave up using the two chips:

<img width="1199" height="582" alt="Screenshot 2025-07-16 080923" src="https://github.com/user-attachments/assets/1d5d8d78-a229-4240-abf3-c15adf49a096" />

Afterwards, I searched online for other projects where people used an ESP-32 to play audio and I found that many people used a MAX98375 breakout to play audio, as it included a DAC and amp. The reason I didn't use this earleir was
becuase I was looking for easier to solder tht ics to avoid reflow soldering. I later realized that since I'm using an ESP-32 and TP4056 IC, I will eventually need to reflow solder, so I decided to switch. Once I was done wiring, the speaker
portion looked like this:

<img width="635" height="384" alt="image" src="https://github.com/user-attachments/assets/5c59758f-b3e9-4972-b857-0ee40d049c01" />

I added in a quick TP4056 charging circuit and SPI SD card reader and now this is my final schematic:

<img width="1318" height="638" alt="image" src="https://github.com/user-attachments/assets/9e636b43-5d3a-46e9-8a23-f80c6f63d095" />

## Time Spent: 8 Hours (I didn't know what I was doing at all)

## JUNE 20th: Worked On PCB

Today I worked on the PCB. I imported the component footprints from the schematic and started placing them in good spots. I checked the size of the E-ink display, which had an active size of 99x53 mm. I decided to make the PCB a bit smaller than that, at 90x50 mm. I started by placing the ESP-32 down and the corresponding buttons. Then I placed the larger modules on the back, such as the GPS, E-Ink adapter, and the microphone, although I did place the sd card reader on the front as I thought it would be better there. I then placed the various capacitors and resistors around the board, trying to keep the trace lengths to a minimum to avoid difficult routing. After I placed all the components, this is what my layout looked like:
<img width="1392" height="715" alt="image" src="https://github.com/user-attachments/assets/fc36530a-abf3-4300-ad9d-615164959f6e" />

I first started routing my USB C connector, as those have specific requirements like short trace lengths that must be routed as differential pairs. After a couple attempts and some moving of surrounding components, I was able to get a routing that I liked:

| <img width="860" height="602" alt="image" src="https://github.com/user-attachments/assets/b3a88ea6-b55b-44b9-a4c9-495d7266f046" /> |
|------------------------------------------------------------------------------------------------------------------------------------|
| The highlighted traces are for USB                                                                                                 |

Next, I went on to route various resistors, capacitors, and other components like the AMS 1117. I made sure to keep the traces on the front side of the board at most times to make it easier to route the components that are on the back side of the board. This was relatively easy at first, but then I started running into space constraints and had to move components around.

Then, I finally routed the larger components and modules on the back. Once again, at first, this was relatively easy, but as I started adding more traces and more vias, space began getting constrained again. I had to move components around and place lots of vias for the ground pour, but I eventually routed all the traces. I ran DRC and found this error: "rear solder mask aperture bridges items with different nets"
for this footprint:

<img width="269" height="480" alt="image" src="https://github.com/user-attachments/assets/b283d33f-5291-415b-9c5a-69f549033b07" />

I decided to change the footprint and reroute, which fixed the issue. This is my final PCB:

| <img width="1058" height="591" alt="image" src="https://github.com/user-attachments/assets/94be7f55-0e77-477b-a9cf-56c41384c8d1" /> | <img width="1126" height="625" alt="image" src="https://github.com/user-attachments/assets/9a86076f-0947-4d8d-a2f2-89843595692c" /> |
|-------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|

## Time Spent: 3 Hours
