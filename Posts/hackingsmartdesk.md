---
blog-title: I turned my desk into a Smart Desk
blog-published: 2022-05-12
blog-tags:
  - blog
  - EN
  - Python
  - SmartHome
blog-skip: true
---

Motorized standing desks are great... but only if you use them. So, I decided to automate mine. 

I have a list of requirements:
- It should move on its own, no buttons or hands required
- It should still be movable by a simple button press 
- It should be controllable from Homeassistant and Home Kit 

## Experimenting

As I do not have a lot of knowledge about electronics I am a bit hesitant to work on the desks motor controllers directly. That is why I first start to experiment with the desks buttons instead. They are glued to the underside of the table and pulling them off does not damage the wood. Underneath the glue there is wiring which connects the buttons to the controller of the table.

There are a total of three cables. The left (green) one is connected with the *"Raise"* button and the right (blue) one with the *"Lower"* button. A black wire connected to both buttons closes the circuit to the controller.

When I now connect the green and black wires with another small piece of wire it triggers the table to move upwards. Doing the same with the blue wire will lower the table.

![[Drawing 2023-11-29 18.07.12.excalidraw]]

## Automating a Button Click

Now that I somewhat understand the circuit, I can start automating it with a relay. In simple terms, relays can mimic button presses electrically. They receive data signals turning on or off an internal switch. 
For the above I need two relays so I bought [this two-relay module](https://www.az-delivery.de/products/2-relais-modul) which is compatible with most low-voltage control devices.

Each button is still connected to the tables controller and can thus function as usual. But now there is an additional way to close the circuit via the two relays.

![[Drawing 2023-11-29 18.32.44.excalidraw]]

### Controlling the Relays

Relays on themselves can't do anything so I need a device to control them. I am using a [ NodeMCU ESP8266 ESP-12F Wifi Development Board](https://www.az-delivery.de/products/nodemcu-lolin-v3-modul-mit-esp8266) which can of course be exchanged for any device you like.

The board has to provide power (and ground) and two data pins, one for each relay. Now the circuit is complete and we can start programming the ESP.

![[Drawing 2023-11-29 18.56.09.excalidraw]]

## HomeAssistant


 

## Raycast Scripts 

To control the real desk from my virtual desk (Mac) I am using the app Raycast which allows me to quickly add commands that execute simple python scripts.

This is a script which will rise the desk:

```python
#!path/to/bin/python

# Required parameters:
# @raycast.schemaVersion 1
# @raycast.title Tisch Hoch
# @raycast.mode compact

# Optional parameters:
# @raycast.icon 🤖
# @raycast.packageName Tisch

# Documentation:
# @raycast.description Fährt den Tisch hoch
# @raycast.author Marc Julian Schwarz
# @raycast.authorURL https://www.github.com/marcjulianschwarz

import paho.mqtt.publish as publish

publish.single("marc/desk", "up", hostname="homeassistant.local",
               auth={"username": "<your-mqtt-username>", "password": "<your-mqtt-password>"})


print("Tisch fährt hoch.")

```







