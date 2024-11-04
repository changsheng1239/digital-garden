---
title: Flashing Sonoff Zigbee Dongle-E to Ember
tags:
  - home-assistant
  - zigbee
date: 2024-08-12
---

I've been running [Home Assistant](https://www.home-assistant.io/) for over a year with incredible stability. Recently, I took a leap to update my Home Assistant & [Zigbee2MQTT](https://www.zigbee2mqtt.io/) as I thought it's been outdated for a year (i probably shouldn't upgrade since everything is working fine but YOLOOOOOO). 

My initial setup was Docker based which consist of 4 containers:
1. HASS
2. z2m
3. mosquitto MQTT broker
4. Vscode server 

I decided to convert this stack to VM based HAOS so I can just scheduled a daily VM snapshot and import back when something went wrong. HAOS addon is also a breeze to work with.

Migration went well and I just had to repair some automation trigger and everything worked. And now I had a fully working HASS + latest z2m. 

Was happy for about a day(?), then I started having issue about z2m add-on randomly shutting down & a particular Aqara switch always response slower than others. After bit of research, seems like z2m now recommend [Ember](https://www.zigbee2mqtt.io/guide/adapters/emberznet.html) adapter, and my dongle was bit outdated too.

I am using the Sonoff ZBDongle-E and running a firmware of 6.x.x. So maybe an upgrade would help? 

So next move: 
1. flash the dongle to latest v7.4.3 firmware
2. switch to ember adapter in z2m
3. pray that it won't break 

Flashing process is simple, just download the latest firmware from [github](https://github.com/darkxst/silabs-firmware-builder/tree/main/firmware_builds/zbdonglee)and flash using this [web flasher](https://darkxst.github.io/silabs-firmware-builder/) (had issue connecting the dongle initially, but installing the [driver](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers?tab=downloads) helped). 

And ... BAM! 

Nothing break, and the particular switch is now acting normal again. Overall speed feels the same but the z2m add-on had been working normally for a week now. 
