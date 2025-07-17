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
