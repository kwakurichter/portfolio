---
title: "HVAC Analysis Project"
summary: "A greenhouse heat pump feasibility study comparing existing propane heating costs against a proposed hybrid heat pump system."
period: "2022"
image: "/images/projects/greenhouses_cad.png"
tags: ["HVAC", "Thermal Analysis", "Energy"]
order: 5
download: "/downloads/hvac-heat-pump-feasibility-study.pdf"
downloadLabel: "Download report"
---

## Overview

This project analyzed alternative heating options for Richters Herbs, a greenhouse operation in southern Ontario. The existing system used propane-fired unit heaters to maintain stable winter growing conditions, and rising propane prices, inflation, and carbon costs created a need to evaluate lower cost heating alternatives.

The study compared the existing propane system against a proposed hybrid system using electric heat pumps as the primary heating source, with the legacy propane heaters retained as backup during extreme cold conditions.

## Problem

Richters Herbs operates all year, and must maintain different minimum temperatures across the greenhouse facility to keep plants healthy. The site was divided into three heating zones:

- Greenhouse #3, a propagation area for young cuttings, requiring a 60 deg F minimum ambient temperature.
- The remaining greenhouse zones, requiring a 50 deg F minimum ambient temperature.
- A boiler room and support spaces, warmed mostly by adjacent greenhouse structures.

The existing heating system used 14 Modine PDP200 propane-fired unit heaters, each rated at approximately 166,000 BTU/hr. Two heaters served Greenhouse #3, while the remaining units were distributed across the larger greenhouse structures.

## Site Survey

The project began with a full survey of the greenhouse facilities. I documented dimensions, construction materials, door types, unit heater locations, heater nameplates, and the different temperature zones across the site.

<figure class="article-figure">
  <img src="/images/projects/hvac-greenhouse.jpg" alt="Exterior of one greenhouse bay during the site survey" />
  <figcaption>Exterior of one greenhouse bay documented during the site survey.</figcaption>
</figure>

The survey identified several construction types that affected the heating load model:

- Large polyethylene greenhouses built from hollow galvanized steel tube frames and air-filled double-layer polyethylene coverings.
- Greenhouse #1, built from a galvanized steel frame with tempered glass panes.
- Greenhouse #3 and Greenhouse #8, which used more rounded wall and roof geometries.
- A boiler room with a painted steel exterior shell and approximately four inches of insulation.
- Large exterior bay doors, smaller swinging doors, and a non-insulated vestibule entrance.

## Modeling

After the survey, I created a 2D AutoCAD floorplan and a 1:1 Fusion 360 model of the greenhouse structures. The AutoCAD floorplan captured heater locations, door dimensions, interior walls, facility names, and equipment notes. The Fusion 360 model was then used to estimate surface areas, volumes, construction boundaries, and material interfaces for later calculations.

Heating loads were calculated using ASHRAE Fundamentals Handbook methods for nonresidential heating and cooling load calculations. The simplified load model used the heat transfer coefficient, exterior surface area, and temperature difference for each structure.

The model evaluated three exterior temperature cases representing typical southern Ontario winter periods:

- 15 deg F for January and February.
- 23 deg F for December and March.
- 32 deg F for portions of November and April.

At the 15 deg F design case, the calculated heating loads were approximately:

- 1,333,633 BTU/hr for the remaining greenhouse zones.
- 124,343 BTU/hr for Greenhouse #1.
- 135,746 BTU/hr for Greenhouse #3.

The largest load came from the polyethylene greenhouse zones because they made up most of the facility surface area and had lower insulating performance than the glass greenhouse.

## Proposed System

The analysis selected Lennox ELP/ELS120 large split system heat pumps as a practical alternative because they offered acceptable cold weather heating performance, moderate installation complexity, and lower operating cost than propane.

The proposed system used 24 heat pumps:

- 20 units for the remaining greenhouse zones.
- 2 units for Greenhouse #1.
- 2 units for Greenhouse #3.

The model estimated the combined heat pump power draw at each exterior temperature case. At 15 deg F, the full heat pump system would draw approximately 142.8 kW across all zones. The total annual heat pump energy consumption was estimated at roughly 444,000 kWh.

## Findings

The existing propane system cost approximately $110,909 per year, or about $924 per day over a four month heating season.

The proposed heat pump system was estimated to cost approximately $51,060 per year in electricity. Because heat pump efficiency falls during very cold weather, the model also included 15 days of backup propane operation at the existing daily cost. This added about $13,860, bringing the proposed hybrid system operating cost to approximately $64,920 per year.

## Result

The analysis estimated annual operating savings of about $45,989 compared with the legacy propane system. Assuming a conservative installed cost of $240,000 for 24 heat pumps, the projected break-even period was about 5.5 years.

The study also recommended further work before implementation, including CFD analysis using the 3D greenhouse model, heat pump placement optimization, and investigation of government clean energy tax incentives, grants, or loans that could reduce the capital cost and shorten the payback period.
