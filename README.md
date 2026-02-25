# Switch-4-Node

## Overview

**Switch-4-Node** is a professional-grade ESP32-based 4-channel relay
controller designed for IoT, Home Automation, and industrial switching
applications.\
It supports WiFi provisioning, MQTT (Home Assistant compatible), a
secure web interface, and per-relay topic control.

This project is built using:

-   ESP32 (Arduino Framework)
-   PlatformIO
-   Async Web Server
-   MQTT (PubSubClient)
-   LittleFS filesystem
-   HTTP Basic Authentication (STA Mode)

------------------------------------------------------------------------

# Key Features

-   Control **4 independent relays**
-   Individual MQTT command/state topics per relay
-   Web-based configuration interface
-   WiFi provisioning with AP fallback mode
-   HTTP Basic Authentication (in STA mode)
-   LittleFS-based frontend storage
-   Compatible with Home Assistant MQTT Switch
-   Clean modular firmware architecture
-   Suitable for industrial dry-contact applications

------------------------------------------------------------------------

# Hardware Requirements

## Recommended Board

-   ESP32 DevKit v1\
-   ESP32-C3 SuperMini (adjust pins accordingly)

## Relay Module

-   4-Channel Relay Board (Active HIGH recommended)
-   External 5V power supply (do NOT power relays directly from ESP32 5V
    pin if high current load)

## Inputs (Optional)

-   4 digital input dry-contact switches
-   Connected to GND (using INPUT_PULLUP configuration)

------------------------------------------------------------------------

# GPIO Mapping (Example)

## Relay Outputs
```
  Relay     GPIO
  --------- --------
  Relay 1   GPIO16
  Relay 2   GPIO17
  Relay 3   GPIO18
  Relay 4   GPIO19
```
## Digital Inputs
```
  Input   GPIO
  ------- --------
  D1      GPIO25
  D2      GPIO26
  D3      GPIO27
  D4      GPIO14
```
Modify according to your hardware configuration.

------------------------------------------------------------------------

# WiFi Behavior

## First Boot

1.  Device starts in **Access Point mode**
2.  Connect to the AP (SSID example: Switch4Node-Setup)
3.  Open captive portal
4.  Enter your WiFi credentials
5.  Device reboots and connects to your network

## STA Mode

-   Web UI protected with HTTP Basic Auth
-   AP provisioning portal disabled

------------------------------------------------------------------------

# Web Interface

The device serves UI files from LittleFS.

## Pages

-   `/` → Main control dashboard
-   `/settings` → WiFi & MQTT configuration
-   `/api/*` → JSON API endpoints

Frontend files stored in:

    /data
      ├── index.html
      ├── settings.html
      ├── app.js
      └── style.css

Upload using:

    PlatformIO → Upload Filesystem Image

------------------------------------------------------------------------

# MQTT Integration

## Base Topic for cammand and state

    switch4node

The mqtt lines in Home Assistant will become:
Command:

    switch4node/relay/1/set

State:

    switch4node/relay/1/state

------------------------------------------------------------------------

# Home Assistant Configuration Example

Add to `configuration.yaml`:

    mqtt:
      switch:
        - name: "Switch4Node Relay 1"
          command_topic: "switch4node/relay/1/set"
          state_topic: "switch4node/relay/1/state"
          payload_on: "ON"
          payload_off: "OFF"
          retain: false

        - name: "Switch4Node Relay 2"
          command_topic: "switch4node/relay/2/set"
          state_topic: "switch4node/relay/2/state"
          payload_on: "ON"
          payload_off: "OFF"
          retain: false

Repeat for relay 3 and 4.

------------------------------------------------------------------------

# Project Structure

    Switch-4-Node/
    │
    ├── src/
    │   └── main.cpp
    │
    ├── include/
    │
    ├── lib/
    │
    ├── data/
    │   └── (Web UI Files)
    │
    ├── platformio.ini
    └── README.md

------------------------------------------------------------------------

# PlatformIO Configuration Example

    [env:esp32dev]
    platform = espressif32
    board = esp32dev
    framework = arduino
    monitor_speed = 115200

    lib_deps =
      bblanchon/ArduinoJson
      knolleary/PubSubClient
      https://github.com/esphome/ESPAsyncWebServer.git
      https://github.com/esphome/AsyncTCP.git

------------------------------------------------------------------------

# Build & Flash Instructions

1.  Install VSCode
2.  Install PlatformIO extension
3.  Clone repository
4.  Open project folder
5.  Connect ESP32 via USB
6.  Click **Build**
7.  Click **Upload**
8.  Upload LittleFS image if using web UI

------------------------------------------------------------------------

# Security Notes

-   Always enable HTTP Basic Authentication in STA mode
-   Do not expose device directly to internet without firewall
-   Use strong MQTT credentials
-   Prefer static IP reservation from router

------------------------------------------------------------------------

# Use Cases

-   Home automation switching
-   Pump control systems
-   Industrial dry contact relays
-   Remote power cycling
-   IoT lab experiments
-   Smart greenhouse control


------------------------------------------------------------------------

# Maintainer

Dr. Zainal Abidin Arsat\
Universiti Malaysia Perlis (UniMAP)

------------------------------------------------------------------------
