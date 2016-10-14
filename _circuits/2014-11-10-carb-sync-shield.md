---
layout: circuits
title: Carb Sync Shield
subcat: hardware 
image: /img/thumbs/carbsync.png
description: Carb sync Arduino shield for motorcycles. 
num: 22
status: Complete
---

I worked with Tom Hogue (<a href="http://www.venturerider.org/forum/showthread.php?73158-Built-a-digital-carb-sync">tz89 from the Venture Rider forum</a>) to translate his prototyped design onto a printed circuit board. He's done all of the software; my contributions were entirely in hardware. 

The Carb Sync Shield monitors the up to six carburetors and displays the vacuum pressure in realtime on an LCD display so the user can make adjustments to the air/fuel mix in each cylinder.

There's an additional RPM feature that will be useful to set and monitor the idle speed on bikes that don't have a tachometer.

<img src="https://jenner.smugmug.com/Carb-Sync-Shield/i-CnTJMHR/0/L/IMG_2190-L.jpg">

<img src="https://jenner.smugmug.com/Carb-Sync-Shield/i-JPjRQ3X/0/L/IMG_2193-L.jpg">

<img src="https://jenner.smugmug.com/Carb-Sync-Shield/i-895ngdv/0/L/IMG_2433-L.jpg">

The kit can be assembled by a novice using a basic soldering iron. The pressure sensors have extra-large surface mount pads and all other components are through-hole. 

<img src="https://jenner.smugmug.com/Carb-Sync-Shield/i-NcsHnpf/0/M/IMG_2304-M.jpg">

The shield is compatible with both the <a href="http://store.arduino.cc/product/A000066">Arduino Uno v3</a> and the Bluetooth-capable <a href="http://redbearlab.com/blend/">RedBear Blend v1</a>. It displays the RPM calculated from each of up to six simultaneous 3.3V or 5V pressure sensors on an LCD display which can be mounted directly or connected with the rainbow jumpers as shown above. 

<img src="https://jenner.smugmug.com/Carb-Sync-Shield/i-PCbRQZg/0/M/ardblend-M.png">

The board draws power from the carrier Uno or Blend board, which in turn can run off a 9V battery or a USB plug. The blue trim potentiometer controls the brightness of the LCD so it's visible indoors or outdoors. The large switch sets the analog pressure sensor reference voltage to 3.3V or 5V. In the final version of the board, the switch is replaced with a 3-pin header and jumper cap.  

Three digital I/O lines are broken out along with dedicated 5V or 3.3V pins to support extra sensors. It's up to the user to match the sensor voltage on the digital line to the expected operating voltage of the Uno or Blend.

We had the boards fabricated through <a href="http://oshpark.com">OSHPark</a>, a local batch PCB service based in Oregon, and did a successful test in early November on Tom's bike. 

<img src="https://jenner.smugmug.com/Carb-Sync-Shield/i-BJjknMv/0/L/IMG_2436-L.jpg">

<img src="https://jenner.smugmug.com/Carb-Sync-Shield/i-t9DcvPN/0/L/IMG_2438-L.jpg">

In retrospect, this was the first project where I sought out the client, negotiated, did the work, and generally was very pleased with the final prouct. I learned a lot that helped me with future projects. 

Also, it turned out that Tom knew Dean Su, so this indirectly led me to the Pothole Project in 2015 and 2016.
