---
title: "Raspberry Pi Audio Streamer"
summary: "A compact headless Raspberry Pi Zero 2 W audio receiver with Spotify Connect, Bluetooth fallback, and a custom SolidWorks enclosure."
period: "2026"
image: "/images/projects/rpi-audio-streamer.png"
tags: ["Linux", "Audio", "CAD"]
order: 4
source: "https://github.com/kwakurichter/rpi-audio"
---

## Overview

This personal project turns a Raspberry Pi Zero 2 W into a compact headless audio receiver. The goal was to create an alternative to the discontinued Chromecast Audio from Google. Spotify Connect is the default source, Bluetooth A2DP acts as a fallback, and audio is routed through an I2S DAC HAT to powered speakers, an amplifier, or an older stereo receiver.

The finished device is meant to modernize any existing audio analog setup: plug in power, connect it to WiFi, choose it from Spotify, and let the software handle the audio routing in the background.

<div class="article-video">
  <iframe
    src="https://www.youtube.com/embed/aisrnJWEon0"
    title="Raspberry Pi audio streamer demo"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
  ></iframe>
</div>

## Architecture

In normal operation, the device appears as a Spotify Connect speaker through Raspotify/librespot. When a Bluetooth device connects, a watcher service stops Raspotify and starts Bluetooth playback. When Bluetooth disconnects, Spotify Connect resumes.

This gives the device a simple source-priority model: Spotify Connect is always available by default, but Bluetooth can temporarily take control when a phone, laptop, or tablet connects. Once that Bluetooth session ends, the system returns to Spotify Connect without requiring a reboot or manual service restart.

The audio path is:

- Spotify or Bluetooth source.
- Raspberry Pi Zero 2 W.
- Raspotify/librespot or BlueZ/BlueALSA.
- ALSA.
- RPi DAC+ / I2S DAC HAT.
- Amplifier or powered speakers.

The software stack uses Raspberry Pi OS Lite, NetworkManager for WiFi profiles, Raspotify/librespot for Spotify Connect, BlueZ and BlueALSA for Bluetooth A2DP, and ALSA for routing audio to the DAC HAT. The DAC is addressed through `plughw:1,0`, which keeps ALSA flexible with sample format and sample rate conversion.

## Hardware

The build uses a Raspberry Pi Zero 2 W, microSD card, 5 V power, an RPi DAC+ / I2S DAC HAT, and analog output cabling. The enclosure was designed in SolidWorks and sized around a compact footprint of roughly 75 mm by 39 mm by 38 mm.

The printed enclosure protects the Pi and DAC while exposing the RCA outputs, 3.5 mm jack, USB ports, and power input. The enclosure files include SolidWorks parts and a printable 3MF file, with standard PLA working well for the prototype.

<figure class="article-figure">
  <img src="/images/projects/rpi-enclosure-solidworks.png" alt="SolidWorks render of the Raspberry Pi audio streamer enclosure bottom" />
  <figcaption>SolidWorks render of the enclosure bottom, including internal supports and component clearance.</figcaption>
</figure>

<figure class="article-figure">
  <img src="/images/projects/rpi-enclosure-cover-solidworks.png" alt="SolidWorks render of the Raspberry Pi audio streamer enclosure cover" />
  <figcaption>SolidWorks render of the enclosure cover, with cutouts for the DAC outputs, USB access, and status openings.</figcaption>
</figure>

## Result

The result is a small WiFi audio endpoint with automatic source switching and a printable enclosure. It is designed to be used without a monitor, keyboard, or regular manual maintenance. The necessary components can be purchased for less than $60 CAD (as of July 2026), and the entire device can be built easily in a few hours.
