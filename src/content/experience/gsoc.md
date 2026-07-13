---
title: "Google Summer of Code Contributor"
role: "Contributor"
organization: "ArduPilot"
period: "May 2026-present"
location: "Remote"
summary: "Developing AP_SwarmMesh, a resilient MAVLink ad-hoc peer networking layer for decentralized swarm coordination in ArduPilot."
image: "/images/experience/swarm.jpg"
tags: ["C++", "ArduPilot", "MAVLink", "Swarm Robotics"]
order: 1
source: "https://discuss.ardupilot.org/t/gsoc-2026-ap-swarmmesh-resilient-mavlink-ad-hoc-swarm-networking-for-ardupilot/144105"
---

## Overview

For Google Summer of Code 2026, I am developing `AP_SwarmMesh`, a native ArduPilot communication backend for resilient MAVLink peer-to-peer swarm networking. The project is mentored by Nate Mailhot and Asif Khan and runs from May to September 2026.

The goal is to move swarm coordination away from a purely centralized Ground Control Station model and toward onboard peer coordination. Each vehicle should be able to broadcast selected state, receive and forward peer packets, maintain a local peer state table, and expose that information to future swarm behaviors inside ArduPilot.

<div class="article-link-row">
  <a class="button secondary" href="https://discuss.ardupilot.org/t/gsoc-2026-ap-swarmmesh-resilient-mavlink-ad-hoc-swarm-networking-for-ardupilot/144105">Read project post</a>
  <a class="button secondary" href="https://github.com/kwakurichter/ardupilot_gsoc/tree/feature/AP_SwarmMesh">View working repository</a>
</div>

## Context

The project builds on my graduate research platform, [ArduSwarm](https://github.com/kwakurichter/ArduSwarm), which combines Bitcraze Crazyflie micro-UAV hardware with a custom ArduPilot build for decentralized indoor swarm experiments. That work gave me a practical testbed for MAVLink routing, peer-to-peer links, onboard autonomy, and multi-vehicle validation.

The broader ArduPilot ecosystem already supports multi-vehicle workflows, but most swarm coordination is still routed through a central GCS or through custom vehicle-to-vehicle logic. That creates three main limitations:

- Connection loss can become a single point of failure.
- Coordination often relies on external computation rather than onboard autonomy.
- Existing workflows are not naturally scalable to larger ad-hoc peer networks.

## Project Proposal

`AP_SwarmMesh` adds infrastructure for decentralized peer state exchange inside ArduPilot. The proposed system gives each vehicle a dedicated peer stream, a routing header for MAVLink peer packets, forwarding and duplicate rejection rules, persistent peer snapshots, and logging hooks through the existing ArduPilot logging backend.

The project introduces:

- **Logging integration:** peer messages that pass initial screening are parsed and logged through existing ArduPilot infrastructure.
- **Peer state table:** each vehicle maintains a bounded table of peer identity in memory; link quality, vehicle state, and coordination fields.
- **Persistent snapshots:** peer state can be periodically written under `APM/PEERS/` to recover useful state after a reboot.
- **Peer stream:** user-configurable MAVLink streams disseminate state messages at rates appropriate to the selected hardware backend.
- **Peer routing header:** a compact header carries routing, forwarding, duplicate suppression, timeout, and payload length information without modifying MAVLink payload definitions.

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

On the receive side, bytes from the peer serial port are fed through a fixed header state machine. Once the header is validated, the packet is screened for version compatibility, duplicates, flags, staleness, and TTL. If the packet is still useful and should be relayed, the forwarding path rewrites `prev_id`, decrements TTL, and sends a copy onward. Valid MAVLink payloads are then parsed and used to update logging and peer state records.

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

The peer state table stores the minimum state needed for future onboard swarm coordination. The current proposal tracks peer identity, liveness, sequence history, link quality, local/global position, velocity, acceleration, vehicle mode, arming/landing state, health flags, role, task ID, formation slot, target position/velocity/acceleration, and priority.

The initial design keeps memory bounded by limiting the maximum swarm size to up to 254 peers (depending on the users configured flight controller hardware). At roughly 100 bytes per peer entry, the table is small enough for ArduPilot targets while still preserving the information needed for formation control, leader/follower behaviors, and future distributed autonomy.

## Validation Plan

The first milestone is a SITL backend, `AP_SwarmMesh_SITL`, where the physical serial link is replaced with UDP sockets. Each simulated vehicle binds to a known port and sends or receives the same packets used by the real transport. That allows the framing, parsing, duplicate rejection, TTL handling, and forwarding rules to be tested in multi-vehicle simulation before hardware deployment.

The interactive replay below shows an initial SITL leader-follower formation experiment. One leader vehicle moves in a square trajectory while four follower vehicles attempt to hold fixed offsets around it, matching the planned hardware demonstration in simulation before moving to real radios and flight tests.

<div class="article-embed">
  <iframe src="/images/experience/formation.html" title="Interactive SITL leader-follower formation replay" loading="lazy"></iframe>
</div>

<div class="article-link-row">
  <a class="button secondary" href="/images/experience/formation.html">Open interactive replay</a>
</div>

The hardware demonstration target is a leader-follower formation flight. A small fleet of ArduCopter vehicles exchange heartbeat and position messages until each vehicle has a populated peer table. An onboard Lua script will then read the peer state table, compute fixed NED offsets from the leader, and issue position targets so followers maintain formation while the leader is manually commanded.

## Community Development

The project is being developed in the open with ArduPilot community feedback. The main design questions involve the boundary between swarm behavior and networking, the contents of the peer state table, which MAVLink messages should be forwarded, how filtering should work, and how much of the mesh behavior belongs inside ArduPilot versus transport-specific radio systems.
