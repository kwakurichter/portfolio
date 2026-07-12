---
title: "PackerPro"
summary: "A low-cost automated seed packaging machine built for Richters Herbs as a University of Waterloo capstone project."
period: "2023-2024"
image: "/images/projects/packerpro.JPG"
tags: ["Mechanical Design", "Automation", "Capstone"]
order: 2
download: "/downloads/packerpro-final-design-report.pdf"
downloadLabel: "Download report"
---

## Overview

PackerPro was my 5th year mechanical engineering design capstone project at the University of Waterloo. The project was sponsored by Richters Herbs, and the goal was to automate a manual seed packing process used across more than 1000 seed varieties.

The machine accepts bulk seeds and empty packets, portions the seeds, fills each packet, seals it, and ejects the finished packet into a collection area.

<div class="article-video">
  <iframe
    src="https://www.youtube.com/embed/_lHGYC00hmM"
    title="PackerPro system operating"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
  ></iframe>
</div>

<div class="article-video">
  <iframe
    src="https://www.youtube.com/embed/loAZpzNOaXY"
    title="PackerPro filled packets"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
  ></iframe>
</div>

## Problem

Richters Herbs relied on manual labour for a production process that had become difficult to scale. Operators had to retrieve a seed variety, measure seeds by weight or volume, fill plastic packets, and seal batches with a heat press.

The design requirements included:

- Repeatable seed measuring within 10 wt. percent of target.
- A production rate at least matching the existing manual rate of 15 packets per minute.
- Variable batch sizes, with 20 to 100 packets produced in a run.
- Hopper capacity of roughly 1.5 L of seeds.
- Packet filling, heat sealing, and finished packet collection.
- Minimal human interaction during production.
- A user-friendly operating process with safe access to power, pneumatics, and hot components.
- Cleanout between seed varieties to avoid cross-contamination.

## Design

The final concept used a rotary table with four packet clamps moving around a stationary plate. Four major operations were mounted 90 degrees apart so the system could process packets in sequence:

- Packet injection.
- Seed portioning.
- Heat sealing.
- Packet ejection.

<figure class="article-figure">
  <img src="/images/projects/packerpro_solidworks.png" alt="SolidWorks model of the PackerPro seed packaging machine" />
  <figcaption>SolidWorks model of the final PackerPro assembly, including the rotary base, seed hopper, packet feeder, and electrical enclosure.</figcaption>
</figure>

The rotary base transported packets between stations using a NEMA 17 stepper motor and a 4:1 gear reduction. This made the motion predictable: one complete motor revolution advanced the packet clamps by a quarter turn to the next subsystem.

The packet injection system used pneumatic suction cups to pull a single packet from a spring-loaded packet feeder and place it into a clamp. The feeder was redesigned from an early clear plastic concept into a stiffer aluminum structure with a linear slide, UHMW low-friction surfaces, and a top-loading lid. The final feeder held approximately 225 empty packets.

The seed portioning tower used an auger driven by a stepper motor to meter seeds from the hopper. A servo-actuated ball valve was added so the system could pre-portion seeds and release them quickly once the packet was opened, improving production rate compared with dispensing directly during the fill step.

The heat sealer used a pneumatic clamp and resistive heating element to melt an airtight seal into the packet. Thermal simulation of the sealer was used to target a 150 C steady-state heating element temperature and roughly 70 W of heat output.

The full machine also included a pneumatic circuit, a 12 V electrical distribution system, stepper motor drivers, MOSFET boards for solenoid control, a servo-controlled seed valve, a heated sealing subsystem, and an Arduino Mega microcontroller.

## Manufacturing

The project combined machined aluminum, waterjet-cut structural components, 3D-printed parts, pneumatic actuators, electrical boards, fasteners, and off-the-shelf controls hardware. My individual contributions included stakeholder relations, mechanical design and manufacturing, and electrical system design.

To keep the design manufacturable in the student shop, many structural parts were designed around 6061 aluminum, black PLA, waterjet-cut plates, and standard metric fasteners. More complex non-structural geometry was 3D printed, while larger precision plates were machined externally through the engineering shop.

The electrical tower organized the power distribution board, MOSFET boards, TB6600 stepper drivers, step-down converter, and Arduino. The system used a single external power input, user-accessible fuse protection, and a master switch for simple operation and emergency shutoff.

The final material cost was $1358.19, with the largest cost increase coming from controls components and externally machined stationary and rotating plates.

## Results

PackerPro produced packets at a rate of 15 packets per minute. In a steady-state production test, the machine processed 20 packets in 1 minute and 20 seconds, equivalent to one packet every 4 seconds. Seed portioning verification used a 40 repetition proxy test and measured a standard deviation of 0.963 with a range of 4.

The final system could produce roughly 150 to 200 filled packets in a run after the user loaded seeds and empty packets, connected compressed air and electrical power, and pressed start.

<figure class="article-figure">
  <img src="/images/projects/best_prototype_award.JPG" alt="PackerPro team receiving the Best Prototype Award at the University of Waterloo capstone symposium" />
  <figcaption>PackerPro received the Best Prototype Award at the University of Waterloo Mechanical Engineering capstone symposium.</figcaption>
</figure>

The project won the Best Prototype Award and was nominated for Best Overall. Future improvements identified with Richters Herbs included stronger and more precise augers for smaller seed varieties, interchangeable auger pitches, and continued refinement before full scale seed production.
