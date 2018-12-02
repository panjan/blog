---
layout: post
title:  "Making a Glowing Alien Mask"
date:   2018-11-27 21:46:18 +0100
---

![green alien](/blog/assets/alien_green3.gif)

The awesome [Glowing Mirror Mask tutorial](https://learn.adafruit.com/glowing-mirror-mask/introduction) by Adafruit inspired me to make this Halloween mask. The main differences are that I used a cool alien design and cheaper parts. Also the image is printed (not cut into vinyl) which allows us to use any shapes and even colours.

## Required Skills

You should be able to do the following. There are plenty tutorials on these topics.

- using a breadboard
- soldering
- sewing
- programming the Wemos board using Arduino IDE (at least blink a led)

  > A common pitfall in flashing the Wemos board is not having [drivers for the USB to serial converter](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers).

## Parts and Materials

All of the parts can be ordered from eBay or AliExpress.

- [alien vector image](https://www.istockphoto.com/vector/sign-of-space-aliens-gm906014358-249808396) (€9)
- 5V RGB individually addressable LED strip
- wemos d1 mini (I recommend to order at least two)
- wemos battery shield
- 3.7V lithium battery with connector
- male-male jumper cables
- bread board
- shrink tubes
- holographic foil or paper
- one-way mirror foil
- silicone glue
- craft felt
- flexible strip

## Tools

- hot air gun, hair drier or lighter (for shrink tubes)
- hot glue gun
- needle and thread
- soldering iron
- utility knife
- wire cutter

> Before you start with this project make sure you understand the risks of working with electronics and lithium batteries (they blow up if shorted!). The mask is not waterproof.

## Flashing Wemos

Flash the Wemos using Arduino IDE. Use the [Adafruit Neopixel library](https://github.com/adafruit/Adafruit_NeoPixel) to control the LEDs. Start with [this example](https://github.com/adafruit/Adafruit_NeoPixel/blob/master/examples/strandtest/strandtest.ino).

## Light Up the LEDs

First solder headers to the Wemos to make it breadboard-compatible. Then connect the LED strip according to the following diagram. You can power the Wemos directly through USB to save battery.
![wiring diagram](/blog/assets/wemos_led_strip_wiring.png)

![first test](/blog/assets/alien_first_test.png)
*First test of the infinity mirror effect. Wow!*

There might be 2 extra cables running from the LED strip. They are meant for external power supply. You don't need them as Wemos has a 5V output. Make sure to isolate them though.

## Choosing Design

I've chosen [this vector image](https://www.istockphoto.com/vector/sign-of-space-aliens-gm906014358-249808396) because it has wide edges that perfectly hide the LEDs so they don't shine directly in your eyes. The €9 price seems reasonable for the time it saved me. If you're adventurous, make or buy a different design. I used the free SVG editor Inkscape to erase the background.

TIP: If you're buying an image, use Google image search to see if there's a better deal on another site.

## Making the Front Layer

Print the mask on an A4 transparent self-adhesive foil. You can do this at home if you have a suitable printer and transparency foil. I had it done at a local copy centre. Make sure you crop the image as much as possible so that there are no margins on the sides. For an adult-sized mask the A4 format is just wide enough. First try printing the image on an ordinary paper if you don't want to risk wasting a transparency foil.

![alien printed on transparency foil](/blog/assets/alien_foil.jpeg)
*This is how the image looks printed on a transparency foil.*

Stick the transparency foil to the one-way mirror foil. First peel the thin strip on the edge of the transparency foil. Align it with the edge of your one-way mirror foil. Carefully peel the rest of the sticker while TODO: vyhlazovani bublinek.

Use a utility knife to cut out the mask.

![transparency foil on one-way mirror foil](/blog/assets/alien_foil_cut.jpeg)
*Transparency foil on a one-way mirror foil.*

## Making the Back Layer

Use the front layer as a template to cut out the felt and holographic foil layers. Keep the felt left-overs. You'll use them to cover the Wemos board later.

- make the front layer slightly wider so that it bends better

![holographic foil and felt](/blog/assets/alien_holographic_felt.jpeg)

Use your hot glue gun to glue the two layers together.

## Cut Out the Eyes

I cut out the eyes above the "eyebrows" of the alien. I was afraid that the holes would look bad so I made them quite small. It doesn't look bad at all and I should have made them bigger as  it's quite hard to see with the mask on.

## Attaching the LED Strip

Sew the LED strip to the edge of the back layer. Leave a small margin between the LED strip and the edge so that the LED strip doesn't slide off of the back layer.

When you're about to close the loop, cut off the excess of the LED strip using scissors (TODO: link to LED strip cuttin tutorial). Cut a hole for the connector and pull it through. Use jumper cables to connect your Wemos and check that everything is still working.

![led strip test](/blog/assets/alien_led_strip_test.jpeg)
*LED strip attached to the back layer*

Isolate the end of the LED strip using a shrink tube. Squeeze the end with pliers as in the picture below.

![led strip isolation](/blog/assets/alien_led_strip_ending.jpeg)

## Attaching the Front Layer

Glue the front layer to the LED strip using silicone glue. Use tape to hold everything together. Wait until the next day to let the glue dry completely.

![glueing front layer](/blog/assets/alien_silicone_glue.jpeg)
*You can see tape holding the mask together while the silicon glue dries.*

## Connecting the Board

Solder connections according to the Fritzing scheme. Strengthen the connections using a hot glue gun. Sew Wemos to the inside of the mask. Isolate the unused power cables.

![wemos connected](/blog/assets/alien_wemos.jpeg)

## Battery Holder and Charging

- TODO

## Final Touches

Cover the Wemos with some felt (use hot glue gun). Allow for access to the micro USB port.

![wemos covered with felt](/blog/assets/alien_wemos_covered.jpeg)

Sew a flexible strip to the mask and enjoy!

![green alien](/blog/assets/alien_green.gif)

----------
- prototyping and coding
- glowing mirror effect
- keep the mask bent
- why wemos - can be powered by 3.7V lipo, supplies 5V to the strip, wifi
- why arduino ide
