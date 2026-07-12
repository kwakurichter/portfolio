---
title: "External SSD Enclosure"
summary: "A custom external storage design focused on reliability, transfer speed, portability, and thermal performance."
period: "2022"
image: "/images/projects/final_render.png"
tags: ["CFD", "Thermal", "Product Design"]
order: 7
download: "/downloads/external-ssd-enclosure-report.pdf"
downloadLabel: "Download report"
---

## Overview

This project began after a commercial Seagate external hard drive failed and caused complete data loss. I wanted to design a replacement that improved on the weak points of a mechanical drive while staying practical for everyday portable use.

The target device needed to be rugged, compact enough to carry in a backpack, usable without an internet connection, compatible with modern transfer standards, and capable of storing at least 1 TB of data. The design also needed to maximize read/write speed and cooling performance while keeping cost reasonable.

## Design Direction

The selected architecture used a Samsung 980 Pro 1 TB NVMe SSD paired with a Trebleet 2-in-1 Thunderbolt 3 / USB 3.2 enclosure. SSD storage was chosen over a mechanical hard drive because it avoids fragile spinning platters and read heads, making it better suited to portable use.

The Samsung 980 Pro was selected for its price-to-performance balance. In the report comparison, it offered 7000 MB/s sequential read and 5100 MB/s sequential write performance at a substantially lower cost than the fastest competing 1 TB drive considered. The Trebleet enclosure was selected because it packaged Thunderbolt 3 and USB 3.1/3.2 support into a compact aluminum shell measuring approximately 98 mm x 49 mm x 14.5 mm.

The enclosure also used an Intel JL7440 controller for Thunderbolt and USB support, plus a JMicron JMS583 controller for dedicated USB operation. That combination helped satisfy the compatibility requirement across my desktop PC, Dell XPS 15, and iPad Pro.

## Thermal Analysis

Modern NVMe SSDs can generate enough heat under sustained transfers to trigger thermal throttling. Since the enclosure had to remain compact and rugged, fans or other active cooling methods were not a good fit. The design therefore focused on improving passive heat transfer through conduction into the enclosure and natural convection from the exterior surface to ambient air.

The report modeled the heat path as a thermal resistance network from the SSD, through the thermal pad and enclosure shell, and into the surrounding air. That analysis showed two practical ways to improve cooling without compromising the enclosure: increase the exterior surface area and increase the thermal conductivity of the shell material.

Several fin concepts were modeled on the top cover, including plate fins, cylindrical fins, and pin fins. Each concept increased the enclosure height by about 5 mm while preserving the compact form factor. The shell material was also compared between the factory aluminum design and a copper replacement, since copper has a higher thermal conductivity.

For the CFD work, I simplified the enclosure geometry, modeled the SSD as the main heat-generating body, and used symmetry to simulate half of the enclosure. The SSD was initialized at 35 degrees C and modeled as a 4.5 W heat source in the half domain, corresponding to a 9 W peak heat output for the full SSD. The ambient air was set near room temperature at 19.85 degrees C, with a small 0.05 m/s airflow domain around the enclosure.

<div class="article-grid three">
  <figure class="article-figure">
    <img src="/images/projects/copper_10s.png" alt="Copper fin thermal simulation at 10 seconds" />
    <figcaption>Copper plate-fin simulation at 10 seconds. Heat begins conducting from the SSD contact region into the cover.</figcaption>
  </figure>
  <figure class="article-figure">
    <img src="/images/projects/copper_30s.png" alt="Copper fin thermal simulation at 30 seconds" />
    <figcaption>Copper plate-fin simulation at 30 seconds. The elevated conductivity spreads heat through the fin field.</figcaption>
  </figure>
  <figure class="article-figure">
    <img src="/images/projects/copper_60s.png" alt="Copper fin thermal simulation at 60 seconds" />
    <figcaption>Copper plate-fin simulation at 60 seconds. The cover distributes heat across a larger convective surface area.</figcaption>
  </figure>
</div>

## Results

The CFD results supported the expected design direction. In the aluminum simulations, the factory cover reached the highest approximate steady-state SSD temperature at about 100 degrees C. Adding fins reduced that temperature, with the pin fin design at about 95 degrees C, the cylindrical fin design at about 90 degrees C, and the plate fin design performing best at about 85 degrees C.

The rate of temperature change plot also favored the plate fin geometry, indicating a higher effective heat transfer rate than the other cover options. A separate copper plate-fin simulation showed the copper shell heating faster than the aluminum shell under the same initial and boundary conditions, which aligned with the higher thermal conductivity of copper and suggested better heat removal from the SSD.

The final recommended design combined the Samsung 980 Pro SSD, Trebleet Thunderbolt/USB enclosure, a machined copper top cover, and plate fins to increase the external surface area.

## Outcome

The final report concluded that a compact external SSD could meet the reliability, speed, portability, and offline use requirements while improving thermal performance through passive enclosure changes. The main remaining work was to machine the custom copper cover from precise enclosure measurements, assemble the drive, and validate the design with real sustained transfer testing.
