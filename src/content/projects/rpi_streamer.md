---
title: "Raspberry Pi Audio Streamer"
summary: "A compact headless Raspberry Pi Zero 2 W audio receiver with Spotify Connect, Bluetooth fallback, and a custom SolidWorks enclosure."
period: "2026"
image: "/images/projects/rpi-audio-streamer.png"
tags: ["Linux", "Audio", "CAD"]
order: 4
source: "https://github.com/kwakurichter"
---

## Overview

This personal project turns a Raspberry Pi Zero 2 W into a compact headless audio receiver. Spotify Connect is the default source, Bluetooth A2DP acts as a fallback, and audio is routed through an I2S DAC HAT to powered speakers, an amplifier, or an older stereo receiver.

## Architecture

In normal operation, the device appears as a Spotify Connect speaker through Raspotify/librespot. When a Bluetooth device connects, a watcher service stops Raspotify and starts Bluetooth playback. When Bluetooth disconnects, Spotify Connect resumes.

The audio path is:

- Spotify or Bluetooth source.
- Raspberry Pi Zero 2 W.
- Raspotify/librespot or BlueZ/BlueALSA.
- ALSA.
- RPi DAC+ / I2S DAC HAT.
- Amplifier or powered speakers.

## Hardware

The build uses a Raspberry Pi Zero 2 W, microSD card, 5 V power, an RPi DAC+ / I2S DAC HAT, and analog output cabling. The enclosure was designed in SolidWorks and sized around a compact footprint of roughly 75 mm by 39 mm by 38 mm.

![SolidWorks enclosure render](/images/projects/rpi-enclosure-solidworks.png)

## Result

The result is a small Wi-Fi audio endpoint with automatic source switching and a printable enclosure. It is designed to be used without a monitor, keyboard, or regular manual maintenance.
