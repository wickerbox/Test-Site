---
layout: toolbox
title: Starting
permalink: /circuits/the-weekly-circuit/start.html
image: /img/thumbs/kicad.png
num: 1
---

## Why this was written

I started learning to use KiCad during my time at OSH Park. I had used other PCB design tools but I only had a vague idea of how to develop the processes and templates that would allow me to develop boards without repeating the same basic beginner steps every time. 

I read a lot of tutorials but didn't find a series of articles that walked someone through starting a project, setting up the libraries in an intuitive way, and generating gerbers that would help simplify things when it came time to place parts on the board.

Armed with a fairly bad memory, I decided to investigate and write down everything about my process to see if I could improve it.

## Introduction

I like to think of circuits as tools in a toolbox. This is a project to work through analyzing, building, and testing circuits while I read <em>Art of Electronics</em>. Also, I've forgotten how to write, and my new year's resolution included practicing. It's this or bad fanfiction.

## Circuit Analysis 

<img src="/img/circuits/weekly-circuit/simple-io.png">

I'm going to generate sine, square, and sawtooth waves and then look at what comes out of the circuit so I can understand the effect my circuit is having on the signal. 

For the input, I've made my own Arduino Uno pulse generator shield. I'm going to explain everything I do, with pictures, so you can play along. 

To monitor the signals, I'm using a Saleae Logic Pro 8. It cost me just under $500, but that's a lot less than a good oscilloscope and the thing only weighs a few ounces. The Logic Pro 4 costs $100 and has three digital channels, and one analog and digital channel. I hugely recommend it.

## Following Along

I'm using through-hole parts from Digikey and releasing verified printed circuit board designs on OSH Park. Following along is as easy as buying a soldering iron and some soldering tools, ordering the Digikey parts, ordering the boards, assembling the boards, using a pulse generator, and using a logic analyzer and multimeter.

The first post is how to assemble and test my pulse generator shield, and get used to using the Saleae Logic Pro. We're not going to worry about the filtering yet. The important thing is that we 

http://bradsduino.blogspot.com/2013/08/generating-sine-waves-with-arduino-uno.html

The second post is about building 9V -> 5V -> 3.3V boards and looking at the difference between voltage dividers, transistors, and important thoughtsa bout power, current, voltage, and heat. I may light something on fire. Also talking about copper weight and trace calculators.

The third post uses an Arduino Uno to demonstrate some basic resistor circuits.

## Tools

- The Saleae Logic Pro 8. It's amazing. 

- Simple pulsegen USB dongle.

- Arduino Uno R3

- Digikey for parts

- Breadboard or Breadboard Shield

## Arduino Signalgen Shield Design Considerations

Working with surface mount parts is way easier than you'd expect. The days of etching with chemicals is over. I wanted an example project that would demonstrate everything needed to get started in ordering boards, ordering parts, placing solder paste with a stencil, soldering surface mount parts with a reflow oven, and testing the board before using it in all the other projects in this series.

I want other people to be able to follow along, so I decided on the Arduino platform. The classic Arduino Uno will do fine. It's a good introduction to microcontrollers and we'll be back here in chapter 15 so buying one now is a good investment.

The Uno connects to the computer and is programmed with the Arduino IDE. In this case, I'll write some code and make it available. All you have to do is install the Arduino development environment, plug in the Arduino Uno board, copy and paste the code into a new sketch, and click 'compile' and 'upload'. That's all.

Having decided on an Arduino Uno, now I know what my form factor will look like. 

`The Arduino Uno shield is a very common first board, and I've created a KiCad template if you want to make your own boards.`

I'll create a board and save it to OSH Park as a shared project, so you'll be able to just order the board, the parts, and assemble it yourself. This will be a surface mount project, so I'll need a stencil (which is super easy) and some solder paste. 

How important is cost? The cost for getting started in electronics is a couple hundred dollars, but by the time you're done you'll have supplies that will make future boards much easier. 

## Functional Diagram

<img src="/img/circuits/ardusignalgen/functional-higher.png">

There's no need for an independent reference voltage source like 5V or 3.3V to go along with the output signal for now, but later on there will be, so using the Arduino Uno is keeping our options open. 

<img src="/img/circuits/ardusignalgen/functional.png">

This is from the point of view of the microcontroller, since that's how I'll code. Everything without a box around it is going to be on the shield itself.

I also bought one of these prototype shields, which has been worth its weight in gold. 

I prototyped an LCD display and the buttons to make sure I had everything right, and then I created the board in KiCad. I didn't expect a lot of current on this board and there aren't any high-speed traces or tight placement. 

I talk about looking at gerbers <a href="#">in this post</a>.

## Order the Parts

OSH Park minimum quantity is three boards, so I uploaded my files and ordered a set of boards from <a href="#">here</a>.

I used KiCad to generate a list of parts, also called a Bill of Materials.

I also made a list of parts that I recommend buying if you don't have any supplies whatsoever.

- WES51 or RadioShack soldering iron
- wick
- solder
- flux pen
- alcohol 
- toothbrush
- vise grip
- arduino uno 
- regular tip
- itty-bitty tip
- water or gold foil

No need for tip tinner, just put some solder on your tip after you're done to help prevent corrosion.

Also, you'll need a reflow oven or hot plate.

Then I ordered a stencil at OSH Stencils, and I recommend grabbing the jig as well. It'll come in handy.

I use blue tape, also called gaffer's or painter's tape, to tack down the stencil while I'm applying paste. 

## Assemble the Boards

## Pull-up and Pull-down Resistors

Ohm's law applies. At some point, the resistance is too high so the LED can't turn on, or the resistance is too low so you might as well not even have a resistor.

Look at the example of a microcontroller pin from an Arduino. Take into account the pin driver. Arduino allows digital pins to be set to INPUT, INPUT_PULLUP with 20K-100K resistors, and OUTPUT. 

OUTPUT can sink or source 40mA. Arduino recommends 470 ohm or 1K resistors to limit current sink/source. 1K are very common, and you get 1000 of <a href="http://www.digikey.com/scripts/DkSearch/dksus.dll?Detail&itemSeq=191148736&uq=635924623977724659">these</a> for $7.30. They're handy.

Because an Arduino Uno output pin can be damaged if you source more than 40mA, the minimum resistance to use is 125 ohms at 5V. 150 ohms at 5V gives you 33mA. 220 ohms gives you 22mA, which is about the max for LEDs. 

{% highlight c %}
int dataPin = 3;

// set the data pin as an output 
// and start the serial connection

void setup() {
  pinMode(dataPin, OUTPUT);
  Serial.begin(9600);
  Serial.println("begin");
}

// toggle the data pin every 200 ms

void loop() {
  digitalWrite(dataPin, HIGH);
  delay(200);
  digitalWrite(dataPin, LOW);
  delay(200);
}
{% endhighlight %}

The LED should blink on or off every 200ms, or about five times per second. You can verify by hooking up the Logic Pro to pin 3 and to ground.

<img src="/img/start/simple-resistor-led.png">

<img src="/img/start/logic-pro-example-blink.png">

The LED doesn't conduct until you apply voltage past the turn-on voltage threshold, which for this red LED is 2.1V. Therefore, the LED and resistor don't act as a pulldown resistor. The line is effectively floating until the Arduino pulls it high or or low. If I left the data pin 3 unconnected, it appears to stay low, but this is undefined behavior so you just don't want to do this. 

So the pin is back in its OUTPUT state. 

If I add a pulldown resistor to pull this line low, I would have to do it in parallel with the resistor and LED. That will provide another path to ground for current from the pin but again, we want to make sure we're not allowing a path that's going to source more than 40mA from the pin. A standard value is 1K, which 5V/1000ohm = 5mA. The LED should stay off until the digitalMode(dataPin, HIGH) call. 

<img src="/img/start/simple-resistor-led-pulldown.png">

The same thing is true with the pullup resistor. The pullup resistor will keep the output at TEST high if the Arduino is in a floating state, but will allow the Arduino to pull the output at TEST low with a digitalMode(dataPin, LOW) call.  The output at TEST will never float. 

<img src="/img/start/simple-resistor-led-pullup.png">

You certainly could pull the line high or low with a direct short to 5V or GND, but there are two problems. First, you'll override the Arduino pin entirely, so you might as well not have it at all. Second, the Arduino pin will source or sink current without limit, which will damage the pin. 

In the shorted pullup image, the LED will remain on forever, but there are two paths to ground from the direct 5V on the line: through the LED, and through the Arduino pin. 

<img src="/img/start/simple-resistor-led-pullup-short.png">

In the shorted pulldown example, the LED will remain off forever, and there is only one path: the Arduino pin to ground, without any limiting, so it will source as much current as it possibly can and be damaged. 

<img src="/img/start/simple-resistor-led-pulldown-short.png">


You can only bypass the LED path by proving another path that has no resistance or barely any relative resistance, and that's bad because it will source or sink more current than the Arduino pin can handle.

## Control of a Line by Button or Microcontroller, with an indicator LED

Now I want to control the data line either with a pushbutton or a microcontroller pin, but I want the microcontroller pin to be able to be used if the pushbutton isn't pushed. 

- You could build two separate circuits and have a slider switch. 
- You could make it so if the pushbutton isn't pushed, the microcontroller can be used.

We definitely want an LED to visually indicate the state of the line. 

First, do we want the data line pulled high or low by default? It makes sense to pull it low, so the LED isn't on all the time. We want the data line or the pushbutton being pressed to pull the line high. 

ot both at the same time.  if that line should be normally high or low. Let's say it's a data line and it's normally low. 

<img src="/img/start/data-line-button-led.png">

It's tempting to just swap the button and resistor and go the other way, where we could have a pullup resistor and use the button to pull the line low. The LED will be on and the line pulled high, unless the microcontroller sinks current to pull it low, or the button is pressed to pull it low. 

<img src="/img/start/data-line-button-led2.png">

The problem is obvious when you look at the current paths. When the button is open, everything should be OK. When the button is shorted, it's still probably okay, because the current limiting R2 doesn't stop the UNO pin from shorting to ground through the button. 

The best answer is this: 

<img src="/img/start/data-line-button-led3.png">

## Smoothing Button Bounce

But something else that's interesting happens when you push the button. Mechanical buttons don't just snap closed: they bounce.

Debounce is easy when you have a microcontroller pin input. 

<div style="text-align:center; background-color: #cfcfcf;">Then i swap out the IC with SN74HC595N by Texas Instruments, it works as expected even without any debounce circuit.

I have no idea why the STMicroelectronics of the 74HC595 version doesn't work with push buttons, maybe they don't like slow clock... (I haven't dig too deep into the datasheet anyway)

So for anyone tried and have the same problem, try different IC.

edit (2015-07-11):
I revisit this circuit with an oscilloscope, and everything came very clear to me now. STMicroelectronics' M74HC595B1 is very sensitive to switch bouncing, whereas Texas Instruments chip have higher tolerance level.
Conclusion, either chip works with a microcontroller, but for push button setup, use the SN74HC595N.

https://www.youtube.com/watch?v=d8V8SRsJRoI
</div>

Another consideration: total current the device can drive: http://www.protostack.com/blog/2010/05/introduction-to-74hc595-shift-register-controlling-16-leds/

There's also the ELM411.

http://electronics.stackexchange.com/questions/66764/how-the-capacitor-works-in-a-debouncing-circuit
## Debouncing buttons

## Final external and debounced button control of a data line
