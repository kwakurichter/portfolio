---
title: "Defensive Drone Swarming Graduate Researcher"
role: "MASc Researcher"
organization: "University of Ottawa"
period: "Jan 2025-present"
location: "Ottawa, ON"
summary: "Researching decentralized micro-UAV swarms through ArduSwarm, an open-source Crazyflie and ArduPilot testbed."
image: "/images/projects/arduswarm-hardware.jpg"
tags: ["Research", "Robotics", "Embedded", "Decentralized Swarming"]
order: 2
source: "https://github.com/kwakurichter/ArduSwarm"
---

## Research Overview

I am a Master of Applied Science candidate in Mechanical Engineering at the University of Ottawa and a member of the D06 Robotics Lab research group. My graduate research focuses on defensive drone swarming, embedded robotics, and infrastructure-free indoor flight testing.

The core platform for this work is ArduSwarm, an open-source micro-UAV testbed for decentralized swarm flight control research. The system combines Bitcraze Crazyflie 2.1 and Crazyflie 2.1 Brushless hardware with the ArduPilot flight stack, onboard localization, peer-to-peer communication, scriptable mission execution, and repeatable data logging.

<div class="article-video">
  <iframe
    src="https://www.youtube.com/embed/67zJn1JK91k"
    title="ArduSwarm experiment video"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
  ></iframe>
</div>

## Motivation

Defensive swarm algorithms are difficult to evaluate using simulation alone. Real micro-UAVs introduce communication delay, packet loss, sensor drift, actuator limits, battery constraints, ground effect behavior, and hardware variation that are easy to simplify away in strictly software studies.

My research work is aimed at building a compact testbed that can evaluate decentralized behaviors under those real constraints without depending on GPS, motion capture, or a centralized ground station during normal operation.

## Conference Paper And Award

This research was published as "ArduSwarm: A Self-Contained Micro-Aerial Vehicle Testbed for Experimental Swarm Flight Control" for the CSME-CFDSC-CSR 2026 International Congress in Vancouver, British Columbia. The paper received Second Place in the Best Student Papers Competition.

<figure class="article-figure">
  <img src="/images/projects/arduswarm/best-student-paper-certificate.png" alt="Second place best student paper certificate for ArduSwarm at the 2026 CSME International Congress" />
  <figcaption>Second Place - Best Student Papers Competition at the 2026 CSME International Congress.</figcaption>
</figure>

<div class="article-link-row">
  <a class="button" href="/downloads/arduswarm-csme-2026.pdf">Read the paper</a>
  <a class="button secondary" href="/downloads/arduswarm-poster.pdf">View the poster</a>
  <a class="button secondary" href="/downloads/arduswarm-best-student-paper-certificate.pdf">View certificate</a>
</div>

## Research Platform

The experimental platform combines flight hardware, onboard sensing, embedded firmware, companion-computer logic, and validation infrastructure:

- Crazyflie 2.1 and Crazyflie 2.1 Brushless vehicles for compact indoor flight testing.
- Flow Deck v2 for PMW3901 optical flow sensing and VL53L1x Time-of-Flight ranging.
- AI Deck companion computer for high-level coordination, WiFi telemetry, and future edge-AI experiments.
- MicroSD deck for onboard flight data capture and post-flight analysis.
- Custom 3D-printed guards with motion-capture marker mounts for OptiTrack benchmarking.

## Technical Work

My technical work spans embedded firmware, communication routing, experiment design, hardware integration, and validation:

- Ported and optimized ArduPilot ArduCopter for the Crazyflie STM32F4 flight controller.
- Wrote and integrated ArduPilot HAL drivers for PMW3901 optical flow over SPI and VL53L1x Time-of-Flight ranging over I2C.
- Tuned non-GPS EKF localization for indoor flight using optical flow, range sensing, and IMU data.
- Built a multi-processor architecture that routes MAVLink and custom messages between the STM32F4 flight controller, AI Deck companion computer, nRF51 radio path, WiFi telemetry path, and ground tools.
- Implemented scriptable high-level control through Lua, Python bridges, custom mission actions, and peer-to-peer command exchange.
- Developed repeatable experiment workflows using onboard SD logs, OptiTrack ground truth, and post-flight analysis scripts.

The architecture separates low-level stabilization and state estimation from higher-level coordination. The flight controller handles real-time control, while the AI Deck and supporting tools handle mission logic, communication, and future decentralized coordination algorithms.

## Experimental Validation

The current research platform has been validated through indoor experiments covering onboard localization, peer-to-peer communication, leader-follower control, and collision avoidance behavior.

<div class="article-grid">
  <figure class="article-figure">
    <img src="/images/projects/arduswarm/localization-position.png" alt="EKF position estimate compared against OptiTrack benchmark" />
    <figcaption>The onboard EKF combines optical flow, Time-of-Flight range sensing, and IMU data. Against OptiTrack ground truth, the CSME paper reports a 0.35 m position RMSE for the indoor validation flight.</figcaption>
  </figure>
  <figure class="article-figure">
    <img src="/images/projects/arduswarm/p2p-latency.png" alt="Peer-to-peer echo latency, RSSI, and echo rate plot" />
    <figcaption>The P2P echo test measured radio link latency with median = 4.77 ms, mean = 5.55 ms, p95 = 10.48 ms, and max = 45.85 ms. The test separates packet exchange behavior from full vehicle-response latency.</figcaption>
  </figure>
</div>

<figure class="article-figure">
  <img src="/images/projects/arduswarm/voodoo-overview.png" alt="Leader-follower Voodoo experiment showing leader angle, RC input, and follower velocity" />
  <figcaption>The leader-follower experiment used one Crazyflie as a handheld leader and another as an autonomous follower. The follower mapped P2P attitude packets into discrete velocity targets while maintaining onboard stabilization.</figcaption>
</figure>

In a separate decentralized command experiment, the full control loop reported a median end-to-end command latency of 333 ms. That result includes mission logic, message routing, and vehicle response, while the radio link echo plot isolates lower-level packet timing.

## Results

The research has produced a self-contained experimental swarm platform with onboard localization, peer-to-peer communication, repeatable logging, and early decentralized behaviors. The CSME 2026 paper reports 0.35 m position RMSE against motion capture ground truth and a median decentralized command latency of 333 ms for the leader-follower experiment.

This work also establishes the infrastructure needed for the next stage of the thesis: a working flight platform, communication stack, analysis workflow, and validation method for testing more advanced distributed algorithms.

## Future Work

The next phase of my thesis will focus on lightweight decentralized state estimation for indoor MAV swarms. The goal is to reduce drift and improve relative awareness without relying on motion capture, GPS, or a centralized localization server.

Planned work includes:

- Developing a reduced-order distributed factor graph for indoor micro-UAV localization, likely using SE(2) or SE(2)+z state representations rather than full 6-DOF optimization.
- Fusing onboard EKF odometry with peer-to-peer relative measurements, UWB range factors, loop closure events, and outlier rejection.
- Designing efficient keyframe and update strategies so each vehicle can share useful localization information without saturating the P2P link.
- Evaluating consensus or distributed optimization methods that can run within the bandwidth, CPU, memory, and timing limits of the Crazyflie and AI Deck hardware.
- Benchmarking localization accuracy, solver frequency, communication rate, packet loss sensitivity, and onboard compute load against OptiTrack ground truth.
- Extending the testbed toward multi-drone demonstrations involving formation keeping, relative positioning, collision avoidance, and decentralized defensive swarm behaviors.

The intended contribution is a practical localization and coordination layer that lets small indoor drones reason about their own motion and their neighbors motion while staying within the limits of real embedded hardware.

## Awards And Funding

- Second Place - Best Student Papers Competition at the 2026 CSME International Congress.
- Special Merit Scholarship - Engineering (2025).
- Research Assistantship (2025).
- Engineering Admission Scholarship (2025).
