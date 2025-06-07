---
blog-title: ESP8266 Height-Adjustable Desk Control with Home Assistant Integration
blog-published: 2022-05-12
blog-tags:
  - EN
  - Python
  - SmartHome
  - Hardware
  - Raycast
blog-skip: false
---
## Project Overview

The goal of this project is to control a height-adjustable desk from a Home Assistant button while still preserving the functionality of the manual button controls on the table itself.

## Prerequisites

These are the exact tools, hardware, and software I used. However, you can exchange most components with other similar ones.

### Hardware

- NodeMCU Lua Lolin V3 Module ESP8266 ESP-12F WIFI Development Board with CH340
- At least 8 jumper wires
- One _2-Channel Relay Module 5V with Optocoupler Low-Level Trigger_
- Three _WAGO 221 Terminal Block 3 Connectors_
- USB-A to Micro-USB cable for powering the board and flashing code

### Tools

- Scissors to cut the cable  
-  Cable insulation stripping tool

### Software

- [Arduino IDE](https://docs.arduino.cc/software/ide/#ide-v2)
- Running [Home Assistant](https://www.home-assistant.io/) instance

## Safety Warnings

The instructions contain steps that permanently alter parts of the desk. If not followed correctly, they **CAN damage** the desk controller, engine, and other parts on or around the table. Make sure to remove any items on, around, and under the desk before performing the steps.

The instructions are relatively safe to follow, as they only deal with low-voltage parts of the table. Nonetheless, be careful and do NOT make any changes to parts that run high voltages. Always check the components with a multimeter before working on them and unplug all cables.

## Step-by-Step Guide

### Understanding the Desk Controls

The desk controls are made up of three cables. The following figures show a schematic view of those cables. The colors are only for illustrative purposes and might be different for your specific desk.

The green (left) cable connects the controller with the `Raise` button, and the blue (right) one connects with the `Lower` button. One black cable connects both buttons back to the table to form a closed loop that is only interrupted by the open buttons themselves. Pressing a button closes the respective circuit, letting the desk controller know to start the engine for raising or lowering the table.

![](/images/desk-diag.jpg)

At this point, it is quite straightforward to intercept the button presses by directly connecting the green and blue wires with the orange wire, thereby creating a closed circuit before the button is even reached. The same connection can be made with the blue cable, lowering the table.

### Cut the Cable

To get access to the individual cables, the cable running from the button controls to the desk has to be cut, and the insulation around it and the individual smaller cables has to be removed for about 1-2 cm.

#### Automating a Button Click

![](/images/desk-relay-diag.jpg)

#### Controlling the Relays

Relays don't do anything on their own. A microcontroller is needed. For example, you can use the NodeMCU ESP8266 development board. The board has to provide power, ground, and two data pins. Each data pin controls one of the relays. Now the circuit is complete, and the ESP can be programmed.

![](/images/desk-controller-diag.jpg)

### Programming the Controller

Open [esp8266.ino](/esp8266.ino) in the Arduino IDE and set all configuration variables.

### Home Assistant Setup

#### 1. Install MQTT Broker

- Install [Mosquitto Broker Add-on](https://www.home-assistant.io/integrations/mqtt/)
- Start Add-on with default configuration
- Add new `mqtt` user under `Settings > People` and allow login

#### 2. Add MQTT Integration

- The MQTT integration should appear under `Settings > Devices & services`
-  Add the integration
-  Here you can try publishing packets to the `desk` topic. Set your payload to `down` or `up`.

#### 3. Add Switch in Home Assistant Configuration

Use the Text Editor add-on to edit the `configuration.yaml` file of your Home Assistant instance. Add the following entry to the file.

```yaml
mqtt:
  switch:
    - name: "Desk Switch"
      unique_id: desk_switch
      command_topic: "desk"
      payload_on: "up"
      payload_off: "down"
```

After restarting Home Assistant, this will add a new switch device that sends `up` and `down` payloads to the `desk` MQTT topic.

## Optional Raycast Integration

The repository includes [two python files](https://github.com/marcjulianschwarz/ha-desk-automation/blob/main/raycast-scripts) that can be included in a [script commands directory](https://github.com/raycast/script-commands) for [Raycast](https://www.raycast.com/). They will add the commands `⬆️ Raise Desk` and `⬇️ Lower Desk`.

![Screenshot of Raycast Script Commands in Raycast Search](/images/raycast.jpg)

### Install

To use the scripts, install the MQTT python client.

```bash
pip install paho-mqtt
```

Then change the shebang line to point to the Python environment where you installed the client, set the MQTT broker hostname, user, password, and add both Python scripts to your script commands directory. You should now be able to raise and lower your desk from Raycast 🎉.
