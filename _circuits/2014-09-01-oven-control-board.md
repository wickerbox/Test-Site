---
layout: post
title: Oven Control Board
subcat: hardware
image: /img/thumbs/ovenboard3.png
description: Oven controller to make the oven more like an applicance.
---

### First Version

An ME capstone team at Portland State is working on an oven so we can cure our own composites. It's awesome. This oven control board was a five-day project that came out of nowhere this week on Tuesday and which I delivered on Sunday.  

Their electrical team member already had the design figured out and working with breadboard components and an Arduino. What he really wanted was an Arduino shield. We met, looked at what he had, and figured out how we'd put it on the right footprint. This is the first time I'd been able to sit with someone who was new to Eagle and really explain what makes a decent schematic, so it's pretty cool to see how much I've learned in just a couple of years.

This is his schematic.

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-Grcj9cH/0/O/i-Grcj9cH.png">

So the very first thing I did was to redraw it to give a better sense of the flow of functionality through the circuit. 

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-d53LZHG/0/O/i-d53LZHG.png">

The layout was constrained by the shield but even moreso by our plan to use the LPKF router. It had to be a two-layer board that didn't have plated through-holes or vias. I managed to put all the traces except one on the bottom layer because all the components get soldered on the bottom side (they're all through-hole) and then I just made that a much wider polygon fill and dropped vias to get the top-trace signal connected down below.

Unfortunately, Arduino shields sit on top of the Arduino so I had to drop an extra via on each pad of the 0.1" headers so copper wire could be slid through and squashed, essentially plating them myself. 

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-Dx3v2DZ/0/O/i-Dx3v2DZ.png">

The final product doesn't look too shabby, but the isolation between the copper pads and the rest of the copper ground plane was so small that it made soldering a stressful chore. I electrically verified everything with a multimeter to make sure I hadn't accidentally bridged to cause a short, and I handed it off to the mechanical engineers for testing. 

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-nqnJtKL/0/O/i-nqnJtKL.png">

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-hPRVMpg/0/O/i-hPRVMpg.png">

### Version 2 

The team needed some changes after their testing, including adding a second relay, so I made some changes to the schematic. 

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-pNsZGPz/0/O/i-pNsZGPz.png">

I made much more even use of the top and bottom layers for traces this time around. The top layer copper is in red and the bottom layer copper is in blue. The white outline shows where the Arduino will sit beneath the board, so you can see the shield extends a bit beyond the Arduino shape.

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-26qbcQp/0/O/i-26qbcQp.png">

Because I was still making this one by hand in the lab, I still wasn't able to plate the drilled holes for the pins themselves, so I had to add in small vias on the pads. I would place the pin in the big hole and solder it to one side of the board, and then I would slide in a small diameterwire through the tiny via and crimp it down to make a good connection.

This is a closeup of how to call that out in the board layout. 

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-smzXxNS/0/O/i-smzXxNS.png">

Again, I did the minimal soldering and handed it off to the mechanical team to finish installing.

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-FFJGxp5/0/O/i-FFJGxp5.png">

### Final Version

The last step was to take the design from homemade board to a professionally fabricated board. 

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-H4Xw49D/0/O/i-H4Xw49D.png"> 

We figured out that we wanted to place the thermocouples on the board itself. I developed the final version of the oven controller, which ended up installed on a 4'x8'x4' curing oven at Portland State in the Mechanical Engineering department. 

The schematic became complicated enough that it was too big to easily fit on a screen, so you can click to open a larger view.

<a href="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-td5nbCr/0/O/i-td5nbCr.png"><img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-td5nbCr/0/O/i-td5nbCr.png"></a>

Because were were going to use OSH Park to fabricate the boards, all of the holes could be plated so there was no more need to place vias in pin pads to connect the top and bottom layers. 

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-gK7CdkD/0/O/i-gK7CdkD.png">

The electronics are a three-board stack with an Arduino Uno R3 on the bottom; the custom oven control board set up in the middle with screw-in Euroterminals for start switch, relay, and thermocouple wires; and a 1.8" TFT LCD Shield from Adafruit for the display on top. 

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-n9B3FwK/0/O/i-n9B3FwK.png">

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-TG92jSd/0/O/i-TG92jSd.png">

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-7dQkTvr/0/O/i-7dQkTvr.png">

The blue case is 3D printed, and it holds both the boards and two relays, with a switch on the front cover. 

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-hHcvxpm/0/O/i-hHcvxpm.png">

The box is attached to the side of the oven, with an air gap to help protect it from the heat.

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-Kd5vcK4/0/O/i-Kd5vcK4.png">

The display allows the user to program up to nine steps in a temperature profile. Using the joystick to navigate the menu, the user can specify a ramp rate, hold temperature, and hold time. Once all the steps are set, the user should set the relay enable switch to ON and then tell the program to begin running the profile. 

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-SWkDk3G/0/O/i-SWkDk3G.png">

<img src="https://jenner.smugmug.com/Oven-Controller/n-PdZDTM/i-cR8DdzH/0/O/i-cR8DdzH.png">

The program automatically detects which thermocouples are present and uses the temperature as input to a PID loop which controls ramp rate by turning on and off up to four relays which are, for our configuration, two heaters, one fan, and one vent.

The code and hardware is available <a href="https://github.com/psas/mme-capstone/">on Github in the 'oven-controller'</a> directory.
  


