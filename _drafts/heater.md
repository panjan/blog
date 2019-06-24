---
layout: post
title:  "Making Your Electric Heater Smarter"
---

![smart heating](/assets/heating/heating.jpg)

This article will show you how to make your ordinary electric heater smarter. You'll be able to set timers, control it with your phone, Alexa and Google Home. Finally, we'll integrate it to the home automation hub [Home Assistant](https://www.home-assistant.io/).

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
- screwdriver
- multimeter
- headlamp (optional)
- needle nose pliers for manipulating cables (optional)
- USB to serial converter and jumpers for flashing the Sonoff (optional)

## Flashing the Sonoff (optional)

This step is only required if you need integration with Home Assistant. Homekit and Google Assistant users can skip this as the original firmware supports both.

Home Assistant doesn't support the original firmware. Luckily, Sonoff devices are relatively easy to flash. I've flashed mine with an alternative [firmware called Sonoff Tasmota](https://github.com/arendst/Sonoff-Tasmota). It is open-source and easy to integrate with Home Assistant. Some soldering and a USB-to-serial converter are required. There are tutorials on flashing the Sonoff available.

> If you decide to flash the firmware, make sure to test the device with the original firmware first.

## Wiring

We are going to connect the Sonoff according to the diagram on the box. For that, we'll need extra cables and a hole in the skirting board to run the cables through.

![sonoff](/assets/heating/sonoff_diagram.png)

First, place the Sonoff against the skirting board in the right angle. Mark the hole for the cables and cut it as in the picture below.

![cutting skirting board](/assets/heating/cutting_skirting_board.png)

Now mark holes for the screws and drill the holes. Attach the Sonoff to the wall.

Measure how much cable you're going to need to connect the Sonoff to the socket. Leave some extra length and cut your 3 core wire. Remove a bit of the outer insulation and use pliers to pull out the three wires.

![cutting cables](/assets/heating/cutting_cables.jpg)

> Before you continue, turn off the circuit breaker to avoid injuries. Use a multimeter or a tester screwdriver to check that the power is off. There are plenty of tutorials on how to test sockets on YouTube.

Connect the cables according to the diagram on the Sonoff.

![cutting cables](/assets/heating/wiring.png)
*TODO: make a picture of final wiring*

Test your connections using a multimeter. Put the skirting board and the socket cover back on and voilà! We have a programmable, remotely controllable heating.

TODO: photo of the result

## Integration with Home Assistant (optional)

![HASS switches](/assets/heating/heating_switches.png)

Unlike Google Home and Apple Homekit, Home Assistant is highly customizable, local and open-source. I run mine on a Raspberry Pi. Pick your hardware and install it according to [these instructions](https://www.home-assistant.io/getting-started/).  

#### MQTT

To connect Home Assistant to Tasmota, we'll need an [MQTT](https://www.home-assistant.io/components/mqtt/) message broker. We'll use Mosquitto as the embedded broker is deprecated. To set it up on Hassbian, follow [Bruh's video tutorial](https://youtu.be/AsDHEDbyLfg). Hass.io users, use the [Mosquitto Addon](https://www.home-assistant.io/addons/mosquitto/).

#### Configuring Tasmota

#### Adding switches to Home Assistant

Add the following to your config.

``` yaml
switch:
  - platform: mqtt
    name: "Bedroom Heating"
    state_topic: "stat/sonoff1/POWER"
    command_topic: "cmnd/sonoff1/POWER"
    payload_on: "ON"
    payload_off: "OFF"
    retain: true
    optimistic: true
```

#### Securing Sonoff Tasmota

Sonoff Tasmota is incredibly easy to setup but the default settings are rather insecure. We'll tweak them using [MQTT features](https://github.com/arendst/Sonoff-Tasmota/wiki/MQTT-Features). The available commands are [listed here](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#mqtt).

TODO: mqtt ssl https://www.home-assistant.io/docs/mqtt/certificate/

##### Secure the Web Server

Set a password for the web UI.

`cmnd/sonoff/WebPassword aVeryStrongPassword`

Disable the web server and enable it only for administrative purposes.

`cmnd/sonoff/WebServer 0`

##### Disable AP mode

By default, the device goes to [AP mode](TODO: link) every time the connection to the router fails. Use the following command to disable this behaviour.

`cmnd/sonoff3/WifiConfig 5 # 5 means Wait`

##### Disable the HTTP API
TODO

You can read more about securing your devices [here](https://github.com/arendst/Sonoff-Tasmota/wiki/Securing-your-IoT-from-hacking).

#### Setting the Retain Flag
    - cmnd/sonoff3/ButtonTopic 1
    - cmnd/sonoff3/ButtonRetain 1
    - popsano zde:
        -
        -
    - toto ma chybu, ze kdyz neni pripojeni na mosquitto, tak tlacitko sice funguje, ale jakmile se znovu pripoji, tak se prepne do retained stavu
        - alespon blika modra ledka, kdyz je pripojeni dole
    - HA -> retain: truek
