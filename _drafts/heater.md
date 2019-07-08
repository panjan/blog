---
layout: post
title:  "Making Your Electric Heater Smarter"
---

![smart heating](/assets/heating/heating_finished.png)

This article will show you how to use the €15 power switch [Sonoff Pow](https://www.itead.cc/sonoff-pow-r2.html) to connect an ordinary electric heater to your WiFi network. You'll be able to control it with your phone, set timers and measure power consumption. Sonoff also supports Amazon Alexa, Google Assistant and Google Nest. No more getting up when you feel cosy and want to turn on the heating!

Optionally, you can integrate your smart heating to the home automation hub [Home Assistant](https://www.home-assistant.io/) as described later.

### Parts and Materials

- electric heater
- Sonoff POW <a href="https://www.itead.cc/sonoff-pow-r2.html" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- power cord with a free end (for testing the Sonoff) <a href="http://s.click.aliexpress.com/e/iyEbSzS" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- 0.5m of 1.5 mm<sup>2</sup> 3 core wire (they'll cut it for you at the hardware store so you don't have to buy the whole spool) <a href="http://s.click.aliexpress.com/e/KlWNjKY" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- 2 pcs 3.5 mm self tapping screws (with wall plugs if needed) <a href="http://s.click.aliexpress.com/e/Ub30HNA" target="_blank"><i class="fa fa-shopping-cart"></i></a>

### Tools

- wire stripper <a href="http://s.click.aliexpress.com/e/buZfHtLa" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- utility knife <a href="http://s.click.aliexpress.com/e/c339dPyk" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- pencil <a href="http://s.click.aliexpress.com/e/cDr0fBhI" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- electric drill <a href="http://s.click.aliexpress.com/e/b1gkDRWg" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- screwdriver <a href="http://s.click.aliexpress.com/e/HYTgl4M" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- multimeter <a href="http://s.click.aliexpress.com/e/YaaRHOs" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- headlamp (optional) <a href="http://s.click.aliexpress.com/e/lPtwlbE" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- needle nose pliers for manipulating cables (optional) <a href="http://s.click.aliexpress.com/e/banEej84" target="_blank"><i class="fa fa-shopping-cart"></i></a>
- USB to serial converter and jumpers for flashing the Sonoff (optional) <a href="http://s.click.aliexpress.com/e/ISUlAnW" target="_blank"><i class="fa fa-shopping-cart"></i></a>


### Check the Power Rating

Before we start, we need to check the power rating of our heater. It must be lower than the one of our Sonoff. Otherwise, we would be risking fire and electrocution.

Take a look at the Sonoff. Depending on which version you have, it should say something like `Maxload: 250V 16A` on the box. Use a multimeter to check the voltage of your wall outlet. There are plenty of [tutorials on YouTube](https://www.youtube.com/results?search_query=multimeter+wall+outlet). In my country, we should get 230 V and I measured 232 V. That's within the 250 V limit.

Now let's check the current. It must be under 16 A. My heater has a sticker on the side which says 3000 W. But how do we know how many amps? We'll use [the power law](https://en.wikipedia.org/wiki/Electric_power) `P = IV` (or `power = current * voltage`).

```
I = P/V = 3000 (W) / 230 (V) = 13,04 A < 16 A
```

We're within the 16 A limit. The last thing we need to calculate to avoid serious danger is wire size. In the [table of wire sizes](https://en.wikipedia.org/wiki/American_wire_gauge) we can see that for our 13 A current, a 16 AWG wire (1.31mm<sup>2</sup>) is sufficient. However, it is a good practice to use thicker wire, so we'll go with 15 AWG (or 1.5mm<sup>2</sup> for us Europeans).

If you are unsure about any of the calculations, ask in the electronics store or call an electrician to help you.

### Test the Sonoff

While it's disconnected, connect the power chord to your Sonoff. Follow the diagram on the box. L stands for line, N is neutral and E is earth ground. In my case, the wires are brown, blue and yellow-green respectively. The colours might vary in different areas.

![sonoff](/assets/heating/sonoff_diagram.png)

Make sure that the Sonoff works by pairing it to [the app](https://sonoff.itead.cc/en/ewelink). While you're at it, have some fun. Explore the app, connect a lamp (with the plug disconnected!), measure the power consumption, etc.

### Flashing the Sonoff (optional)

This step is only required if you want integration with [Home Assistant](https://www.home-assistant.io). Homekit and Google Home users can skip this as Sonoff supports both out of the box.

Home Assistant doesn't support the original Sonoff firmware. Luckily, Sonoff devices are relatively easy to flash. I've flashed mine with an alternative [firmware called Sonoff Tasmota](https://github.com/arendst/Sonoff-Tasmota). It is open-source and easy to integrate with Home Assistant. Some soldering and a USB-to-serial converter are required. There are [tutorials on flashing the Sonoff](https://www.google.com/search?q=flashing+sonoff+pow) available.

> Never try to flash the device while connected to mains power.

If you decide to flash the firmware, make sure you test the device with the original firmware first.

### Wiring

We are going to connect the Sonoff according to the diagram on the box. For that, we'll need a hole in the skirting board to run the cables through. First, place the Sonoff against the skirting board at the right angle. Mark the hole for the cables and cut it as in the picture below.

![cutting skirting board](/assets/heating/cutting_skirting_board.png)

Drill holes in the wall and attach the Sonoff. Measure how much cable you're going to need to connect the Sonoff to the socket. Leave some extra length and cut your 3 core wire. Remove a bit of the outer insulation and use pliers to pull out the three inner wires.

![cutting cables](/assets/heating/cutting_cables.jpg)

Before we start fiddling with your electrical wiring, put on your headlamp. Make sure your girlfriend doesn't need the heating badly in case anything goes wrong. Take a picture of the original wiring. You might want to refer to it later.

> Before you continue, turn off the circuit breaker to avoid injuries. Use a multimeter or a tester screwdriver to check that the power is off.

Connect the cables according to the diagram on the Sonoff. Remember you'll need to screw the cover back to the Sonoff. It's a tight fit so make sure your cables aren't crossing at the point of contact. Test your connections for [continuity with a multimeter](https://www.google.com/search?q=test+for+continuity+with+a+multimeter). A loose connection could cause a fire.

![cutting cables](/assets/heating/wiring.png)

Put the skirting board and the socket cover back on and voilà! You have a smart heater. If you'd like to connect it to Home Assistant, keep reading.

## Integration with Home Assistant (optional)

<img alt="home assistant logo" src="/assets/heating/ha_logo.png" style="display: block; margin-left: auto; margin-right: auto; vertical-align: middle; width: 25%;"/>

Unlike Google Home and Apple Homekit, Home Assistant is highly customizable, local and open-source. I run my instance on a Raspberry Pi. Pick your hardware and install it according to [these instructions](https://www.home-assistant.io/getting-started/).

I assume you've flashed your Sonoff with the alternative firmware Sonoff Tasmota. If not, go back to the section <a href="#flashing-the-sonoff-optional">Flashing the Sonoff</a>.

### MQTT

To connect Home Assistant to Tasmota, we'll need an [MQTT](https://www.home-assistant.io/components/mqtt/) message broker. We'll use Mosquitto as the embedded broker is deprecated. To set it up on Hassbian, follow [Bruh's video tutorial](https://youtu.be/AsDHEDbyLfg). Hass.io users, use the [Mosquitto Addon](https://www.home-assistant.io/addons/mosquitto/).

> Make sure you set a strong and unique password for your MQTT user. A successful attacker would gain full access to your Sonoff and other MQTT devices.

You can publish MQTT messages from [Home Assistant](https://www.home-assistant.io/docs/mqtt/service/), from the command line ([mosquitto_pub](https://mosquitto.org/man/mosquitto_pub-1.html)) or using a graphical client ([MQTT.fx](https://mqttfx.jensd.de) works for me on Mac). I'll leave the choice to you.

### Configuring Tasmota

Configure WiFi and MQTT according to [Tasmota's documentation](https://github.com/arendst/Sonoff-Tasmota/wiki/Initial-Configuration).

Set the `topic` setting to `sonoff1`. Your MQTT configuration should look like this:

<img alt="mqtt config page" src="/assets/heating/mqtt_config.png" style="display: block; margin-left: auto; margin-right: auto; vertical-align: middle; max-width: 300px"/>

Now you can send commands to Tasmota in the form `cmnd/sonoff1/<command> <parameter>`. Available commands are [listed here](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands###mqtt). Let's try one now:

```
cmnd/sonoff1/POWER ON
```

Subscribe to `stat/sonoff1/RESULT`  for command results.

```
{ "POWER": "ON" }
```

### Adding switches to Home Assistant

<img alt="HASS switches" src="/assets/heating/heating_switches.png" style="display: block; margin-left: auto; margin-right: auto; vertical-align: middle; max-width: 400px"/>

To be able to control our heating from Home Assistant, we'll create a software switch as shown in the picture above. Add the following to your `configuration.yaml` and restart Home Assistant.

``` yaml
switch:
  - platform: mqtt
    name: "Bedroom Heating"
    state_topic: "stat/sonoff1/POWER"
    command_topic: "cmnd/sonoff1/POWER"
    optimistic: true
    retain: true
```

### Dealing with Power Outages

My heaters (and boiler) have a special [load control switch](https://en.wikipedia.org/wiki/Demand_response) which turns off the power at certain times. In turn, I get a cheaper tariff but it also means my Sonoff might be offline when I want to control it. In cases like this, when we flip the switch, we don't want to wait for confirmation from the Sonoff. In the config above, we use the `optimistic` option which makes the switch change states immediately.

After the Sonoff goes online again, we want it to pull the state instantly (not the next time we flip the switch). By setting `retain` to `true`, we tell Home Assistant to publish MQTT messages with the `retain` flag. The flag tells newly-subscribed clients this is the "last good state". To make Tasmota pull the state after startup, publish the following MQTT commands:

```
cmnd/sonoff1/ButtonTopic 1
cmnd/sonoff1/ButtonRetain 1
```

Now you should be able to use the switch in Home Assistant even when your Sonoff is offline.

### Securing Sonoff Tasmota

Tasmota is incredibly easy to set up but the default settings are rather insecure. We'll tweak them using [Tasmota's MQTT commands](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#mqtt).

#### Secure the Web Server

Anyone in your local network could access the web interface and take complete control of the device. Set a password using the following command:

```
cmnd/sonoff1/WebPassword aStrongAndUniquePassword
```

Disable the web server and enable it only for administrative purposes.

```
cmnd/sonoff1/WebServer 0
```

#### Disable AP mode

By default, Tasmota goes to the [access point](https://en.wikipedia.org/wiki/Wireless_access_point) mode every time connection to WiFi is lost. So, when your router is offline, Tasmota serves its own unsecured WiFi network for everyone to join. Publish the following command to disable this behaviour (5 means wait).

```
cmnd/sonoff1/WifiConfig 5
```

#### Read the Docs

Read more about securing your devices [here](https://github.com/arendst/Sonoff-Tasmota/wiki/Securing-your-IoT-from-hacking).

## Conclusion

We made a regular electric heater a lot smarter for about €15. Then we flashed it with open-source software and secured it from hacking. Nice! Now it's time to take it further. How about making a Nest-like thermostat using Home Assistant and a thermometer? I'd love to hear about your projects in the comments section.
