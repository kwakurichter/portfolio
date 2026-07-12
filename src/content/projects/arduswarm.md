---
title: "ArduSwarm"
summary: "An open-source micro-UAV testbed for decentralized swarm robotics using Crazyflie hardware and the ArduPilot flight stack."
period: "2025-present"
image: "/images/projects/arduswarm-hardware.jpg"
tags: ["Robotics", "Embedded Systems", "Decentralized Swarming", "Research"]
order: 1
source: "https://github.com/kwakurichter/ArduSwarm"
---

## Overview

ArduSwarm is the focus of my master's thesis research at the University of Ottawa. It is an open-source micro-UAV research platform for testing decentralized swarm flight control algorithms on real hardware rather, than only in simulation.

The platform uses Bitcraze Crazyflie 2.1 and Crazyflie 2.1 Brushless drones running the ArduPilot flight stack, with onboard localization, peer-to-peer communication, scriptable mission execution, and logging for repeatable experiments.

<div class="article-video">
  <iframe
    src="https://www.youtube.com/embed/67zJn1JK91k"
    title="ArduSwarm experiment video"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
  ></iframe>
</div>

## Motivation

Swarm algorithms are often developed in simulation, where communication latency, packet loss, sensor drift, actuator limits, and hardware variation are difficult to model faithfully. ArduSwarm is meant to close that gap by giving researchers a compact indoor testbed for validating decentralized algorithms under real physical constraints.

The project is designed to have small, inexpensive micro-UAVs support decentralized swarm experiments without relying on motion capture, GPS, or a centralized ground station during flight.

## Conference Paper And Award

This work was published as "ArduSwarm: A Self-Contained Micro-Aerial Vehicle Testbed for Experimental Swarm Flight Control" for the CSME-CFDSC-CSR 2026 International Congress in Vancouver, British Columbia. The paper received Second Place in the Best Student Papers Competition.

<figure class="article-figure">
  <img src="/images/projects/arduswarm/best-student-paper-certificate.png" alt="Second place best student paper certificate for ArduSwarm at the 2026 CSME International Congress" />
  <figcaption>Second Place - Best Student Papers Competition at the 2026 CSME International Congress.</figcaption>
</figure>

<div class="article-link-row">
  <a class="button" href="/downloads/arduswarm-csme-2026.pdf">Read the paper</a>
  <a class="button secondary" href="/downloads/arduswarm-poster.pdf">View the poster</a>
  <a class="button secondary" href="/downloads/arduswarm-best-student-paper-certificate.pdf">View certificate</a>
</div>

## Hardware Architecture

The platform combines the Crazyflie STM32F4-based flight controller with modular expansion decks and an additional companion computer:

- Crazyflie 2.1 and Crazyflie 2.1 Brushless vehicles for compact indoor flight testing.
- Flow Deck v2 for PMW3901 optical flow sensing and VL53L1x Time-of-Flight ranging.
- AI Deck companion computer for high level coordintion, WiFi telemetry, and future edge AI experiments.
- MicroSD deck for onboard flight data capture and post flight analysis.
- Custom 3D-printed guards with motion capture marker mounts for OptiTrack benchmarking.

## Technical Work

The main engineering challenge was adapting ArduPilot (ArduCopter) to a very small drone platform while preserving enough autonomy, sensing, and communication capability for swarm research. This required work across embedded software, hardware integration, test infrastructure, and experimental validation.

Key areas included:

- Porting and optimizing ArduPilot for the Crazyflie STM32F4 flight controller.
- Adding and tuning drivers for optical flow, Time-of-Flight ranging, and non-GPS EKF state estimation.
- Building a communication architecture that supports both telemetry and low-latency peer-to-peer drone communication.
- Routing MAVLink and custom messages between the flight controller, AI Deck companion computer, nRF51 radio path, WiFi telemetry path, and ground tools.
- Implementing scriptable high-level control through Lua, Python bridges, and custom mission actions.
- Creating repeatable validation workflows using onboard SD logs, OptiTrack ground truth, and post-flight analysis scripts.

The software architecture separates low-level stabilization and state estimation from higher-level coordination. The STM32F4 flight controller handles real-time flight control, while the AI Deck can support mission logic, communication handling, and future distributed coordination algorithms.

## Experimental Validation

ArduSwarm has been validated through a series of indoor experiments covering onboard localization, peer-to-peer communication, leader-follower control, and collision avoidance behavior.

<div class="article-grid">
  <figure class="article-figure">
    <img src="/images/projects/arduswarm/localization-position.png" alt="EKF position estimate compared against OptiTrack benchmark" />
    <figcaption>The onboard EKF combines optical flow, Time-of-Flight range sensing, and IMU data. Against OptiTrack ground truth, the paper reports a 0.35 m position RMSE for the indoor validation flight.</figcaption>
  </figure>
  <figure class="article-figure">
    <img src="/images/projects/arduswarm/p2p-latency.png" alt="Peer-to-peer echo latency, RSSI, and echo rate plot" />
    <figcaption>The P2P echo test measured radio link latency with median = 4.77 ms, mean = 5.55 ms, p95 = 10.48 ms, and max = 45.85 ms. Most packets remained below roughly 10.5 ms, with occasional outliers during the test window.</figcaption>
  </figure>
</div>

<figure class="article-figure">
  <img src="/images/projects/arduswarm/voodoo-overview.png" alt="Leader-follower Voodoo experiment showing leader angle, RC input, and follower velocity" />
  <figcaption>The leader-follower experiment used one Crazyflie as a handheld leader and another as an autonomous follower. The leader broadcast attitude data over the P2P link, and the follower mapped pitch and roll commands into discrete velocity targets while maintaining onboard stabilization.</figcaption>
</figure>

In a separate decentralized command experiment, the full control loop reported a median command latency of 333 ms. That value includes mission logic, message routing, and vehicle response, while the P2P echo test above isolates the lower-level packet exchange behavior.

Additional collision avoidance experiments used RSSI as a lightweight proximity signal. A flying drone listened for peer packets from a second drone, filtered the RSSI readings, and climbed when the other vehicle entered a close-proximity threshold.

## Results

The project demonstrates a self-contained micro-UAV swarm testbed with onboard localization, peer-to-peer communication, repeatable logging, and early decentralized behavior. The CSME 2026 paper reports 0.35 m position RMSE against motion capture ground truth and a median decentralized command latency of 333 ms for the leader-follower experiment.

The testbed also establishes the infrastructure for future work: a hardware platform, software stack, logging pipeline, and validation workflow that can support more advanced decentralized algorithms.

## Future Thesis Work

My future thesis work builds on ArduSwarm by developing lightweight decentralized state estimation for indoor MAV swarms. The goal is to reduce drift and improve relative awareness without relying on external motion capture systems or centralized localization.

The planned direction is a distributed graph optimization approach that fuses onboard odometry with peer-to-peer relative measurements, UWB ranging, loop closure events, and uncertainty-aware filtering. A major focus will be the practical tradeoff between accuracy, bandwidth, onboard compute load, solver frequency, and communication reliability on resource-constrained aerial robots.
