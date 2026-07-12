---
title: "Google Summer of Code Contributor"
role: "Contributor"
organization: "ArduPilot"
period: "May 2026-present"
location: "Remote"
summary: "Developing AP_SwarmMesh, a decentralized MAVLink peer state exchange framework for the ArduPilot ecosystem."
image: "/images/experience/GSoC_banner.png"
tags: ["C++", "Python", "ArduPilot", "MAVLink"]
order: 1
---

## Role

I am developing AP_SwarmMesh, a C++ ArduPilot communication middleware library for decentralized MAVLink peer state exchange over a dedicated peer-to-peer serial interface. If you are interested, please visit the [detailed write-up](https://discuss.ardupilot.org/t/gsoc-2026-ap-swarmmesh-resilient-mavlink-ad-hoc-swarm-networking-for-ardupilot/144105)!

## Work

The work includes an object-oriented mesh architecture with stream builders, frame and deframe logic, bounded peer state tables, duplicate suppression, TTL and staleness handling, forwarding rules, and logging hooks.

I am also implementing a compact peer-routing header that wraps standard MAVLink frames without changing payload definitions, preserving compatibility with existing ArduPilot and MAVLink abstractions.

## Validation

The protocol is being validated using UDP SITL backends with multi-vehicle relay scenarios. The goal is to quantify packet correctness, latency, loss, and CPU/memory usage before hardware deployment.

The project is developed in collaboration with the upstream ArduPilot community through code reviews, technical design discussions, documentation, and incremental integration with existing firmware architecture.
