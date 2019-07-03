---
layout: post
title:  "Making Your Electric Heater Smarter"
---

![smart heating](/assets/heating/heating_finished.png)

This article will show you how to use the €15 power switch [Sonoff Pow](https://www.itead.cc/sonoff-pow-r2.html) to connect your ordinary electric heater to your WiFi. You'll be able to control it with your phone, set timers and measure power consumption. Sonoff also supports Amazon Alexa, Google Assistant and Google Nest. No more getting up when you feel cosy and want to turn on the heating!

Optionally, you can integrate your smart heating to the home automation hub [Home Assistant](https://www.home-assistant.io/) as described later.

## Parts and Materials

- electric heater
- [Sonoff POW](https://www.itead.cc/sonoff-pow-r2.html)
- 1.5mm2 3 core wire (about 0.5m)
- 1.5mm2 3 core wire with a plug (for testing)
- 2 screws (TODO: size)
- wall plugs (only for masonry)

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

## Check the Power Rating

Before we start, we need to check the power rating of our heater. It must be lower than the one of the Sonoff. Otherwise, we would be risking fire and electrocution.

Take a look at the Sonoff. Depending on which Sonoff version you have, it should say something like `Maxload: 250V 16A` on the box. Use a multimeter to check the voltage of your wall outlet. There are plenty of [tutorials on YouTube](https://www.youtube.com/results?search_query=multimeter+wall+outlet). In my country, we should get 230V and I measured 232V. That's within the 250V limit.

Now let's check the current. It must be under 16A. My heater has a sticker on the side which says 3000W. But how do we know how many amps? Use [the power law `P = IV`](https://en.wikipedia.org/wiki/Electric_power).

```
I = P/V = 3000W / 230V = 13,04A < 16A
```

We're within the 16A limit. The last thing we need to calculate to avoid serious danger is wire size. In the [table of wire sizes](https://en.wikipedia.org/wiki/American_wire_gauge) we can see that for our 13A, a 16AWG wire (1.31mm2) is sufficient. However, it is a good practice to use a thicker wire, so we'll use a 15AWG (or 1.5mm2 for us Europians). Don't hesitate to use a thick wire even for less powerful heatings. As our wire is very short, it won't cause a substantial power loss.

If you are unsure about any of the calculations, ask in the electronics store or call an electrician to help you.

## Test the Sonoff

Connect your 3 core wire with a plug to the Sonoff (with the plug disconnected of course). Follow the diagram on the box. L stands for line, N is neutral and E is earth ground. In my case, the wires are brown, blue and yellow-green respectively. The colours might vary in different areas.

![sonoff](/assets/heating/sonoff_diagram.png)

Make sure that the Sonoff works by pairing it to [the app](https://sonoff.itead.cc/en/ewelink). While you're at it, have some fun. Explore the app, connect a lamp (with the plug disconnected!), measure the power consumption, etc.

## Flashing the Sonoff (optional)

This step is only required if you want integration with [Home Assistant](https://www.home-assistant.io). Homekit and Google Assistant users can skip this as the original firmware supports both.

Home Assistant doesn't support the original firmware. Luckily, Sonoff devices are relatively easy to flash. I've flashed mine with an alternative [firmware called Sonoff Tasmota](https://github.com/arendst/Sonoff-Tasmota). It is open-source and easy to integrate with Home Assistant. Some soldering and a USB-to-serial converter are required. There are [tutorials on flashing the Sonoff](https://www.google.com/search?q=flashing+sonoff+pow) available.

> Never try to flash the device while connected to power.

If you decide to flash the firmware, make sure you test the device with the original firmware first.

## Wiring

We are going to connect the Sonoff according to the diagram on the box. For that, we'll need a hole in the skirting board to run the cables through. First, place the Sonoff against the skirting board in the right angle. Mark the hole for the cables and cut it as in the picture below.

![cutting skirting board](/assets/heating/cutting_skirting_board.png)

Drill holes in the wall and attach the Sonoff. Measure how much cable you're going to need to connect the Sonoff to the socket. Leave some extra length and cut your 3 core wire. Remove a bit of the outer insulation and use pliers to pull out the three inner wires.

![cutting cables](/assets/heating/cutting_cables.jpg)

Before we start fiddling with your electrical wiring, put on your headlamp. Make sure your girlfriend doesn't need the heating badly in case anything goes wrong. Take a picture of the original wiring. You might want to refer to it later.

> Before you continue, turn off the circuit breaker to avoid injuries. Use a multimeter or a tester screwdriver to check that the power is off.

Connect the cables according to the diagram on the Sonoff. Remember you'll need to screw the cover back to the Sonoff. It's a tight fit so make sure your cables aren't crossing at the point of contact. Test your connections for [continuity with a multimeter](https://www.google.com/search?q=test+for+continuity+with+a+multimeter). A bad connection could cause a fire.

![cutting cables](/assets/heating/wiring.png)

Put the skirting board and the socket cover back on and voilà! You have a smart heating. Congratulations! Now it's time to lie down and turn the heating on and off senselessly.

## Integration with Home Assistant (optional)

<img alt="home assistant logo" src="/assets/heating/ha_logo.png" style="display: block; margin-left: auto; margin-right: auto; vertical-align: middle; width: 25%;"/>

Unlike Google Home and Apple Homekit, Home Assistant is highly customizable, local and open-source. I run my instance on a Raspberry Pi. Pick your hardware and install it according to [these instructions](https://www.home-assistant.io/getting-started/).

I assume you've flashed your Sonoff with the alternative firmware Sonoff Tasmota. If not, go back to the section "Flashing the Sonoff".

#### MQTT

To connect Home Assistant to Tasmota, we'll need an [MQTT](https://www.home-assistant.io/components/mqtt/) message broker. We'll use Mosquitto as the embedded broker is deprecated. To set it up on Hassbian, follow [Bruh's video tutorial](https://youtu.be/AsDHEDbyLfg). Hass.io users, use the [Mosquitto Addon](https://www.home-assistant.io/addons/mosquitto/).

> Make sure you set a strong and unique password for your MQTT user. A successful attacker would gain full access to your Sonoff.

You can publish MQTT messages from [Home Assistant](https://www.home-assistant.io/docs/mqtt/service/), from the command line ([mosquitto_pub](https://mosquitto.org/man/mosquitto_pub-1.html)) or using a graphical client ([MQTT.fx](https://mqttfx.jensd.de) works for me on Mac). I'll leave the choice to you.

#### Configuring Tasmota

Configure WiFi and MQTT according to [Tasmota's documentation](https://github.com/arendst/Sonoff-Tasmota/wiki/Initial-Configuration).

Set the `topic` setting to `sonoff1`. Your MQTT configuration should look like this:

<img alt="mqtt config page" src="/assets/heating/mqtt_config.png" style="display: block; margin-left: auto; margin-right: auto; vertical-align: middle; max-width: 300px"/>

Now you can send commands to Tasmota in the form `cmnd/sonoff1/<command> <parameter>`. Subscribe to `stat/sonoff1/RESULT` for command results. Available commands are [listed here](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#mqtt).

TODO: example command

#### Adding switches to Home Assistant

<img alt="HASS switches" src="/assets/heating/heating_switches.png" style="display: block; margin-left: auto; margin-right: auto; vertical-align: middle; max-width: 400px"/>

To be able to control our heating from Home Assistant, we'll create an software switch as shown in the picture above. Add the following to your `configuration.yaml` and restart Home Assistant.

``` yaml
switch:
  - platform: mqtt
    name: "Bedroom Heating"
    state_topic: "stat/sonoff1/POWER"
    command_topic: "cmnd/sonoff1/POWER"
    optimistic: true
    retain: true
```

#### Dealing with Power Outages

My heaters (and boiler) have a special [load control switch](https://en.wikipedia.org/wiki/Demand_response) which turns off the power at certain times. This gives me a cheaper tariff but it also means the Sonoff might be offline when I want to control it. In cases like this, when we flip the switch, we don't want to wait for confirmation from the Sonoff. In the config above, we use the `optimistic` option which makes the switch change states immediately.

After the Sonoff goes online again, we want it to pull the state instantly (not the next time we flip the switch). By setting the `retain` setting to `true`, we tell Home Assistant to publish MQTT messages with the `retain` flag. The flag tells newly-subscribed clients this is the "last good state". To make Tasmota pull the state after startup, publish the following MQTT commands:

```
cmnd/sonoff1/ButtonTopic 1
cmnd/sonoff1/ButtonRetain 1
```

#### Securing Sonoff Tasmota

Tasmota is incredibly easy to setup but the default settings are rather insecure. We'll tweak them using [Tasmota's MQTT features](https://github.com/arendst/Sonoff-Tasmota/wiki/MQTT-Features).

##### Secure the Web Server

Anyone in your local network could access the web interface and take complete control of the device. Set a password using the following command:

`cmnd/sonoff1/WebPassword aStrongAndUniquePassword`

Disable the web server and enable it only for administrative purposes.

`cmnd/sonoff1/WebServer 0`

##### Disable AP mode

By default, Tasmota goes to the [access point](https://en.wikipedia.org/wiki/Wireless_access_point) mode every time connection to WiFi is lost. So, when your router is offline, Tasmota serves it's own unsecure WiFi network for everyone to join. Publish the following command to disable this behaviour:

`cmnd/sonoff1/WifiConfig 5 # 5 means Wait`

##### Read the Docs

Read more about securing your devices [here](https://github.com/arendst/Sonoff-Tasmota/wiki/Securing-your-IoT-from-hacking).

## Conclusion

We made a regular electric heater a lot smarter for about €15. Then we flashed it with open-source software and secured it from hacking. Nice! Now it's time to take it further. How about making a Nest-like thermostat using Home Assistant and a thermometer? Or what about geofences? Let me know about your projects in the comments section.
