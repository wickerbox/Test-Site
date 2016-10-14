---
layout: post
title: STM32F4 Discovery Breakout
image: /img/thumbs/stm32breakout.png
---

The STM32F4-Discovery development board has columns of male pin headers. I made a breakout board since you can't plug the dev board into a breadboard, since the two columns on each side will short, and I've found the female-to-male jumpers to be unreliable. I made up a breakout board but it's sadly cost prohibitive at $40 for three boards. Still a quick, fun project.

<img src="https://jenner.smugmug.com/STM32-Discovery-Breakout/i-TV7kmhd/0/L/stm32-v2-side-L.png">

I've open sourced and <a href="https://oshpark.com/shared_projects/LSHOUKpw">shared the project at <span class="caps">OSH</span> Park</a>.

I started with Jason Lopez's <a href="http://www.electro-tech-online.com/threads/stm32f4-discovery-eagle.123047/">STM32F4-Discovery Board Eagle schematic and footprint</a>.

For the first test, I placed all the traces on the bottom of the board. Bottom in the layout here is blue. This made it possible to route a breakout board on the <span class="caps">PCB</span> router at Portland State's Lab for Interconnected Devices. I didn't want to have to plate all the via holes by hand to solder on the bottom and use traces on the top. Been there, done that, not interested. There are 200 vias on this board!

<img src="https://jenner.smugmug.com/STM32-Discovery-Breakout/i-gTFtSPm/0/L/stm32f4-v1-layout-L.png">

<img src="https://jenner.smugmug.com/STM32-Discovery-Breakout/i-nD3cv57/0/L/stm32-top-separate-L.png">

<img src="https://jenner.smugmug.com/STM32-Discovery-Breakout/i-55T6mm6/0/L/stm32-v1-top-L.png">

<img src="https://jenner.smugmug.com/STM32-Discovery-Breakout/i-k5PXQ8G/0/L/stm32-side-L.png">

It worked fine, but there's no silk and I'd really like a better looking board. I uploaded the now-verified Eagle .brd file to <span class="caps">OSH</span> Park. Of course, since <span class="caps">OSH</span> Park charges $5/square inch for three boards, it was $53.05 for three! Way out of my price budget. Luckily, you can submit designs where two completely separate boards are sitting next to each other on one .brd file.

<img src="https://jenner.smugmug.com/STM32-Discovery-Breakout/i-HQp3R2h/1/L/stm32-v2-layout-L.png">

This is the <span class="caps">OSH</span> preview with a cost of $33.95 for three. <span class="caps">OSH</span> Park charges for the smallest rectangle that encompasses your design, and you have to leave 100 mils between boards so the fab can mill it out.

<img src="https://jenner.smugmug.com/STM32-Discovery-Breakout/i-KgQF7gt/0/L/osh-preview-L.png">

It's still significant, at about $10/board, but I can live with that. The 2&#215;25 and 1&#215;25 female headers also added up. Looks great, though.

<img src="https://jenner.smugmug.com/STM32-Discovery-Breakout/i-JKpjxKg/0/L/stm32-v2-closeup-L.png">

The design is licensed under CERN's Open Hardware License v1.2.

The design files are available at the <a href="https://github.com/wicker/stm32f4-discovery-breakout-board">Github repository</a>, and the boards can be ordered <a href="https://oshpark.com/shared_projects/GJyFucOY">for $33.95 for a set of three from OSH Park</a>.

I used these Sullins female headers: <br />

Qty 4 of <a href="https://www.digikey.com/product-detail/en/PPTC251LFBN-RC/S7023-ND/810163">PPTC251LFBN-RC</a> 1&#215;25 0.1&quot; for $1.41 each<br />Qty 2 of <a href="https://www.digikey.com/product-detail/en/SFH11-PBPC-D25-ST-BK/S9201-ND/1990094">SFH11-<span class="caps">PBPC</span>-D25-ST-BK</a> 2&#215;25 0.1&quot; for $2.89 each.

I'd bet you could search on Digikey or Mouser and find a cheaper equivalent.

