---
layout: post
title:  "Making Your Electric Heater Smarter"
---

![smart heating](/assets/heating/heating.jpg)

This article will show you how to make your ordinary electric heater smarter and connect it to Home Assistant. You'll be able to control it remotely over WiFi, set timers and go crazy with automations in Home Assistant.

## Parts and Materials

- electric heater
- Sonoff POW
- 1.5mm2 3 core wire (about 0.5m)
- 2 screws (TODO: size) and wall plugs (for masonry)
- cable with a plug for testing?

## Tools

- wire cutter
- utility knife
- pencil
- electric drill
- screw driver
- multimeter
- needle nose pliers for manipulating cables (optional)
- USB to serial converter and jumpers for flashing the Sonoff (optional)

## Flashing the Sonoff (optional)

This step is optional if you don't need integration with Home Assistant.

Sonoff devices are relatively easy to flash. I've flashed mine with an alternative [firmware called Tasmota](https://github.com/arendst/Sonoff-Tasmota). I wanted something open-source and easy to integrate with Home Assistant. Some soldering and a USB-to-serial converter are required. There are tutorials on flashing the Sonoff available.

> If you decide to flash the firmware, make sure to test the device with the original firmware first.

## Wiring

We are going to connect the Sonoff according to the following diagram. For that, we're going to need extra cables and a hole in the skirting board to run the cables through.

TODO: make diagram

First, place the Sonoff against the skirting board in the right angle. Mark the hole for the cables and cut it as in the picture below.

![cutting skirting board](/assets/heating/cutting_skirting_board.png)

Now mark holes for the screws and drill the holes. Attach the Sonoff to the wall.

Measure how much cable you're going to need to connect the Sonoff to the socket. Leave some extra length and cut your 3 core wire. Remove a bit of the outer insulation and use pliers to pull out the three wires.

![cutting cables](/assets/heating/cutting_cables.jpg)

> Before you continue, turn off the circuit breaker to avoid injuries. Make sure you understand the risks of working with high voltage. If in doubt, call a licensed electrician to help you.

Take the skirting board and the socket cover off. Before we start fiddling with the wiring, take out your phone and take a picture of the connections. Line is brown, neutral is blue and ground is yellow and green. Be advised that the colours may be incorrect. Use a multimeter to check your wiring. There are plenty of tutorials on how to test sockets with a multimeter on YouTube.

![cutting cables](/assets/heating/wiring.png)
*TODO: make a picture of final wiring*

Connect the cables according to the diagram and test your connections using a multimeter. Put the skirting board and the socket cover back on.

TODO: photo of the result

## TODO: Securing the Sonoff

## TODO: Integration with Home Assistant
