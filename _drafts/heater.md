---
layout: post
title:  "Making Your Electric Heater Smarter"
---

![smart heating](/assets/heating/heating.jpg)

This article will show you how to make your ordinary electric heater smarter. You'll be able to set timers, measure power consumption and control it with your phone, Alexa and Google Home. Finally, we'll integrate it to the home automation hub [Home Assistant](https://www.home-assistant.io/). No more getting up when you feel cosy and want to turn on the heating!

All this is possible just with the €15 WiFi power switch [Sonoff Pow](https://www.itead.cc/sonoff-pow-r2.html).

## Parts and Materials

- electric heater
- Sonoff POW
- 1.5mm2 3 core wire (about 0.5m)
- 2 screws (TODO: size) and wall plugs (for masonry)
- cable with a plug for testing (optional)

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

This step is only required if you want integration with [Home Assistant](https://www.home-assistant.io). Homekit and Google Assistant users can skip this as the original firmware supports both.

Home Assistant doesn't support the original firmware. Luckily, Sonoff devices are relatively easy to flash. I've flashed mine with an alternative [firmware called Sonoff Tasmota](https://github.com/arendst/Sonoff-Tasmota). It is open-source and easy to integrate with Home Assistant. Some soldering and a USB-to-serial converter are required. There are tutorials on flashing the Sonoff available.

> If you decide to flash the firmware, make sure you test the device with the original firmware first.

## Wiring

We are going to connect the Sonoff according to the diagram on the box. For that, we'll need some cables and a hole in the skirting board to run the cables through.

![sonoff](/assets/heating/sonoff_diagram.png)

First, place the Sonoff against the skirting board in the right angle. Mark the hole for the cables and cut it as in the picture below.

![cutting skirting board](/assets/heating/cutting_skirting_board.png)

Now drill holes in the wall and screw the Sonoff to it. Measure how much cable you're going to need to connect the Sonoff to the socket. Leave some extra length and cut your 3 core wire. Remove a bit of the outer insulation and use pliers to pull out the three inner wires.

![cutting cables](/assets/heating/cutting_cables.jpg)

> Before you continue, turn off the circuit breaker to avoid injuries. Use a multimeter or a tester screwdriver to check that the power is off. There are plenty of tutorials on how to test sockets on YouTube.

Take a picture of the wiring now (and thank me later). Connect the cables according to the diagram on the Sonoff. L stands for line, N is neutral and E is earth ground. In my case, the wires are brown, blue and yellow-green respectively. Be advised that your colours might be off. If in doubt, use a multimeter or a tester screwdriver.

When you do the wiring, remember you'll need to put on the cap of the Sonoff. It's a tight fit so make sure your cables aren't crossing (TODO: crossing?).

![cutting cables](/assets/heating/wiring.png)
*TODO: make a picture of final wiring*

Test your connections using a multimeter (TODO: how). A bad connection could impose fire hazard. Put the skirting board and the socket cover back on and voilà! We have a smart heating.

TODO: photo of the result

## Integration with Home Assistant (optional)


![home assistant logo](/assets/heating/ha_logo.png)

Unlike Google Home and Apple Homekit, Home Assistant is highly customizable, local and open-source. I run my instance on a Raspberry Pi. Pick your hardware and install it according to [these instructions](https://www.home-assistant.io/getting-started/).

#### MQTT

To connect Home Assistant to Tasmota, we'll need an [MQTT](https://www.home-assistant.io/components/mqtt/) message broker. We'll use Mosquitto as the embedded broker is deprecated. To set it up on Hassbian, follow [Bruh's video tutorial](https://youtu.be/AsDHEDbyLfg). Hass.io users, use the [Mosquitto Addon](https://www.home-assistant.io/addons/mosquitto/).

> Make sure you set a strong and unique password for your MQTT user. A successful attacker would gain full access to your Sonoff.

You can publish MQTT messages from [Home Assistant](https://www.home-assistant.io/docs/mqtt/service/), from the command line ([mosquitto_pub](https://mosquitto.org/man/mosquitto_pub-1.html)) or using a graphical client ([MQTT.fx](https://mqttfx.jensd.de) works for me on Mac). I'll leave the choice to you.

Subscribe to `stat/sonoff1/RESULT` for command results.

#### Configuring Tasmota

TODO: Tasmota screenshot

#### Adding switches to Home Assistant

![HASS switches](/assets/heating/heating_switches.png)

Add the following to your `configuration.yaml`.

``` yaml
switch:
  - platform: mqtt
    name: "Bedroom Heating"
    state_topic: "stat/sonoff1/POWER"
    command_topic: "cmnd/sonoff1/POWER"
    payload_on: "ON" # TODO: default?
    payload_off: "OFF" # TODO: default?
    optimistic: true
    retain: true
```

#### Dealing with Power Outages

My heaters (and boiler) have a special [load control switch](https://en.wikipedia.org/wiki/Demand_response) which turns off the power at certain times. This gives me a cheaper tariff but it also means the Sonoff might be offline when I want to control it. In cases like this, when we flip the switch, we don't want to wait for confirmation from the Sonoff. In the config above, we use the `optimistic` option which makes the switch change states immediately.

After the Sonoff goes online again, we want it to pull the state instantly (not the next time we flip the switch). By setting the `retain` setting to `true`, we tell Home Assistant to publish MQTT messages with the `retain` flag. It tells newly-subscribed clients that this is the "last good state". To make Tasmota pull this state after startup, publish the following MQTT commands:

```
cmnd/sonoff1/ButtonTopic 1
cmnd/sonoff1/ButtonRetain 1
```

#### Securing Sonoff Tasmota

Tasmota is incredibly easy to setup but the default settings are rather insecure. We'll tweak them using [Tasmota's MQTT features](https://github.com/arendst/Sonoff-Tasmota/wiki/MQTT-Features). The available commands are [listed here](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#mqtt).

##### Secure the Web Server

Anyone in your local network could access the web interface and take complete control of the device. Set a password using the following command:

`cmnd/sonoff1/WebPassword aStrongAndUniquePassword`

Disable the web server and enable it only for administrative purposes.

`cmnd/sonoff1/WebServer 0`

##### Disable AP mode

By default, Tasmota goes to the [access point](https://en.wikipedia.org/wiki/Wireless_access_point) mode every time connection to WiFi fails. So when your router is offline, Tasmota serves it's own unsecure WiFi network for everyone to join. Publish the following command to disable this behaviour:

`cmnd/sonoff1/WifiConfig 5 # 5 means Wait`

##### Read the Docs

Read more about securing your devices [here](https://github.com/arendst/Sonoff-Tasmota/wiki/Securing-your-IoT-from-hacking).

## Conclusion

We made a regular electric heater a lot smarter for about €15. Then we flashed it with open-source software and secured it from hacking. Nice! Now it's time to take it further. How about making a Nest-like thermostat using Home Assistant and a thermometer?
