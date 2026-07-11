---
title: "ArduSwarm"
summary: "An open-source micro-UAV testbed for decentralized swarm robotics using Crazyflie hardware and the ArduPilot flight stack."
period: "2025-present"
image: "/images/projects/arduswarm-hardware.jpg"
tags: ["Robotics", "Embedded Systems", "Research"]
order: 1
download: "/downloads/arduswarm-csme-2026.pdf"
source: "https://kwakurichter.wordpress.com/2025/07/29/arduswarm/"
---

## Overview

ArduSwarm is the focus of my master's thesis research at the University of Ottawa. It is an open-source research platform for testing decentralized, network-based swarming algorithms using Bitcraze Crazyflie micro-UAVs and the ArduPilot flight control stack.

The platform bridges simulation and real hardware by combining onboard localization, peer-to-peer communication, and scriptable mission execution on a small indoor drone platform.

## Motivation

Defensive swarm algorithms are often tested in simulation, where sensor noise, communication latency, packet loss, and hardware variation are difficult to model faithfully. ArduSwarm creates a physical testbed so researchers can validate decentralized algorithms under real constraints.

## Hardware

The platform uses the Bitcraze Crazyflie 2.1 and Crazyflie 2.1 Brushless hardware ecosystem, including:

- Flow Deck v2 for PMW3901 optical flow and VL53L1x Time-of-Flight ranging.
- AI Deck for companion-computer logic, Wi-Fi telemetry, and potential edge AI.
- MicroSD logging for flight data and post-flight analysis.
- Custom 3D-printed guards with marker mounts for motion-capture benchmarking.

## Software

The main challenge was porting ArduPilot ArduCopter to the Crazyflie STM32 flight controller while preserving enough functionality for autonomous indoor operation. This required custom driver work, memory optimization, sensor integration, communication routing, and extensive validation.

Major software elements include:

- Custom ArduPilot drivers for optical flow and range sensing.
- EKF-based non-GPS localization.
- MAVLink communication between the STM32 flight controller, companion computer, and monitoring station.
- Lua scripting support for rapid mission and algorithm testing.
- Python TCP/UDP bridging for telemetry and command routing.

## Results

Experimental work demonstrated self-contained indoor localization and decentralized command exchange. The CSME 2026 paper reports 0.35 m position RMSE against motion-capture ground truth and a median decentralized control latency of 333 ms over the proposed P2P link.

The project received a 2nd Best Written Paper Award at the CSME-CFDSC-CSR International Congress.
