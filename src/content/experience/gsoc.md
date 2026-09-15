---
title: "Google Summer of Code Contributor"
role: "Contributor"
organization: "ArduPilot"
period: "May-August 2026"
location: "Remote"
summary: "Developed AP_SwarmMesh, a resilient MAVLink peer networking layer for ArduPilot, validated in simulation and hardware flight demos."
image: "/images/experience/swarm.jpg"
tags: ["C++", "ArduPilot", "MAVLink", "Swarm Robotics"]
order: 1
source: "https://discuss.ardupilot.org/t/gsoc-2026-ap-swarmmesh-resilient-mavlink-ad-hoc-swarm-networking-for-ardupilot/144105"
---

## Overview

For Google Summer of Code 2026, I developed `AP_SwarmMesh`, a native ArduPilot communication backend for resilient MAVLink peer-to-peer swarm networking. The project was mentored by Nate Mailhot and Asif Khan and ran from May to August 2026.

The project moves swarm coordination away from a purely centralized Ground Control Station model and toward onboard peer coordination. Each vehicle can broadcast selected state, receive and forward peer packets, maintain a local peer state table, and expose that information to onboard scripts and companion computers. The completed work includes large-scale simulation and hardware flight demos.

<div class="article-link-row">
  <a class="button secondary" href="https://discuss.ardupilot.org/t/gsoc-2026-ap-swarmmesh-resilient-mavlink-ad-hoc-swarm-networking-for-ardupilot/144105">Read project post</a>
  <a class="button secondary" href="https://github.com/ArduPilot/ardupilot/pull/33881">View ArduPilot PR</a>
</div>

## Context

The project builds on my graduate research platform, [ArduSwarm](https://github.com/kwakurichter/ArduSwarm), which combines Bitcraze Crazyflie micro-UAV hardware with a custom ArduPilot build for decentralized indoor swarm experiments. That work gave me a practical testbed for MAVLink routing, peer-to-peer links, onboard autonomy, and multi-vehicle validation.

The broader ArduPilot ecosystem already supports multi-vehicle workflows, but most swarm coordination is still routed through a central GCS or through custom vehicle-to-vehicle logic. That creates three main limitations:

- Connection loss can become a single point of failure.
- Coordination often relies on external computation rather than onboard autonomy.
- Existing workflows are not naturally scalable to larger ad-hoc peer networks.

## Project Implementation

`AP_SwarmMesh` adds infrastructure for decentralized peer state exchange inside ArduPilot. The library gives each vehicle a dedicated peer stream, a routing header for MAVLink peer packets, forwarding and duplicate rejection rules, persistent peer snapshots, and logging hooks through the existing ArduPilot logging backend.

The project introduces:

- **Logging integration:** peer messages that pass initial screening are parsed and logged through existing ArduPilot infrastructure.
- **Peer state table:** each vehicle maintains a bounded table of peer identity, link quality, vehicle state, and coordination fields, with freshness tracked per message type.
- **Persistent snapshots:** peer state can be periodically written under `APM/PEERS/` to recover useful state after a reboot.
- **Peer stream:** user-configurable MAVLink streams disseminate state messages at rates appropriate to the selected hardware backend.
- **Peer routing header:** a compact header carries routing, forwarding, duplicate suppression, timeout, and payload length information without modifying MAVLink payload definitions.

The final update adds a coordination hook for Lua scripts and companion computers through MAVLink `TUNNEL` messages, peer telemetry forwarding to a companion port, and `AP_LocationDB` integration for peer position, velocity, and heading.

## Wire Format

The peer packet design prepends a routing header to a standard MAVLink frame. The receiver can parse the header first, reject bad or stale packets early, and only pass valid payloads into the MAVLink parser. This reduces work in the receive path and keeps MAVLink compatibility intact.

| Field | Purpose |
| --- | --- |
| `magic/sync` | Marks the start of a peer packet. |
| `version` and `flags` | Support future header changes and special packet behavior. |
| `type` | Identifies the payload type, such as MAVLink or future ACK/NACK messages. |
| `ttl` | Limits forwarding hops and prevents endless packet loops. |
| `origin_id`, `prev_id`, `dest_id` | Track source, previous relay, and destination or broadcast routing. |
| `seq` | Enables duplicate suppression, packet tracking, and loss statistics. |
| `origin_time_us` and `deadline_ms` | Support synchronization and freshness filtering. |
| `payload_len` and `crc` | Bound the payload and validate header integrity. |

## TX and RX Paths

On the transmit side, `AP_SwarmMesh::update()` runs from the ArduPilot scheduler at a reduced frequency to limit compute overhead. If a serial port is bound to the peer stream, `AP_SwarmMesh_Serial::update()` iterates through enabled MAVLink streams such as position, attitude, and status. When a stream interval elapses, the outgoing MAVLink frame is wrapped with the peer routing header and written to the bound UART if there is enough `txspace()`.

On the receive side, bytes from the peer serial port are fed through a fixed header state machine. Once the header is validated, the packet is screened for version compatibility, duplicates, flags, staleness, and TTL. Broadcasts are single hop whereas directed packets addressed to another vehicle can be relayed with `prev_id` rewritten and TTL decremented. Valid MAVLink payloads are then parsed and used to update logging and peer state records.

<figure class="article-figure">
  <img src="/images/experience/GSOC_v2.png" alt="High-level overview of the AP_SwarmMesh software and hardware structure" />
  <figcaption>
    High-level overview of the AP_SwarmMesh software and hardware structure. Each vehicle connects to onboard radios through two UART ports: one for standard telemetry and one for the P2P mesh. AP_SwarmMesh serializes peer data into MAVLink packets, encapsulates them in a peer routing header, broadcasts them to neighboring vehicles, and stores limited peer state after parsing received packets.
  </figcaption>
</figure>

<div class="article-link-row">
  <a class="button secondary" href="/images/experience/gsoc_tx_v2.pdf">View TX path</a>
  <a class="button secondary" href="/images/experience/gsoc_rx_v3.pdf">View RX path</a>
</div>

## Peer State Table

The peer state table stores the minimum state needed for onboard swarm coordination. It tracks peer identity, liveness, sequence history, link quality, local/global position, velocity, acceleration, vehicle mode, arming/landing state, health flags, role, task ID, formation slot, target position/velocity/acceleration, and priority.

Memory remains bounded by a capacity of 8 to 255 peer entries at roughly 168 bytes each depending on your hardware. A neighbourhood allowlist controls which peers are tracked, preserving the state needed for formation control while accommodating flight controllers with limited memory.

## Simulation and Hardware Demos

The SITL backend, `AP_SwarmMesh_SITL`, uses UDP multicast to exercise the same framing and routing logic in simulation. Automated tests cover forwarding, TTL rejection, simulated packet loss, and `AP_LocationDB` publishing and expiry.

**Simulation:** the final glyph demo uses one leader and 56 vehicles to spell `GSoC`, then morph into `CoSG`. Each vehicle computes its own target from shared task state and applies local separation filtering. All 56 cells were occupied, with 0.02 m median cell error in the initial formation and no sampled separations below 1m in either run.

<div class="article-embed">
  <iframe src="https://kwakurichter.github.io/ardupilot_gsoc/gsoc-swarm-replay.html" title="Interactive SITL GSoC glyph formation replay with 57 vehicles" loading="lazy"></iframe>
</div>

<div class="article-link-row">
  <a class="button secondary" href="https://kwakurichter.github.io/ardupilot_gsoc/gsoc-swarm-replay.html">Open GSoC replay</a>
  <a class="button secondary" href="https://kwakurichter.github.io/ardupilot_gsoc/gsoc-to-cosg-replay.html">Open GSoC to CoSG replay</a>
</div>

**Hardware:** two demos flew on ArduSwarm using Crazyflie hardware and its onboard nRF51 peer radio through a custom Syslink backend. A hovering leader relayed movement commands to flying followers, and two drones exchanged coordination state to fly coordinated trajectories without a ground station in the coordination loop.

<div class="article-video">
  <iframe
    src="https://www.youtube.com/embed/LBa-KSG2SKs"
    title="AP_SwarmMesh hardware demos: leader-follower and two drone coordinated flight"
    loading="lazy"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
  ></iframe>
</div>

<div class="article-link-row">
  <a class="button secondary" href="https://youtu.be/LBa-KSG2SKs">Watch hardware demos</a>
</div>

## Community Development

The project was developed in the open with ArduPilot community feedback and submitted as [ArduPilot PR #33881](https://github.com/ArduPilot/ardupilot/pull/33881). The GSoC work is complete, with setup and usage documentation included. ACK/NACK synchronization, reference ESP32 radio firmware, and further upstream integration remain follow-up work.
