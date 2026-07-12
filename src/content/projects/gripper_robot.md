---
title: "Gripper Robot"
summary: "A pick-and-place manipulator designed, built, and tested for a University of Waterloo mechanical design course."
period: "2022"
image: "/images/projects/gripper.JPG"
tags: ["Mechanical Design", "Manufacturing", "Robotics"]
order: 3
download: "/downloads/gripper-robot-final-design-report.pdf"
downloadLabel: "Download report"
---

## Overview

The Gripper Robot was a third year mechanical design project at the University of Waterloo. The goal was to design, build, test, and verify a fixed base pick-and-place manipulator capable of moving objects between specified pickup and drop off areas.

The design prioritized strength-to-weight ratio and repeatable placement accuracy. The final robot combined a rotating base, vertical lift, horizontal boom, and power screw actuated claw into a compact manipulator that could lift, translate, and place objects without direct user contact.

<div class="article-video">
  <iframe
    src="https://www.youtube.com/embed/IQVaHT-7uSQ"
    title="Gripper Robot preprogrammed hockey puck pick-and-place timelapse"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
  ></iframe>
</div>

## Requirements

The manipulator had to operate from a fixed base, move objects without direct user contact, and avoid mobile, flying, launching, or throwing designs. The original design objective was to achieve at least a 1:1 strength-to-weight ratio while precisely moving a 3D Benchy across a 2D surface and placing it into a container with 2 inch high walls.

Key requirements and design targets included:

- Lift the object at least 2 inches vertically.
- Translate an object at least 12 inches diagonally.
- Keep the total component cost below $300 CAD.
- Maximize strength-to-weight ratio.
- Maximize placement accuracy and repeatability.
- Place a hockey puck with less than 2% radius error during precision testing.
- Run multiple pick-and-place cycles without recalibration.

## Design

The final design used four primary subsystems: a rotating base, a vertical lift, a horizontal boom, and a mechanical gripper.

The gripper used a power screw actuation concept selected after comparing several claw concepts, including rack-and-pinion, hydraulic, three finger, spring loaded, and power screw approaches. Early prototype testing showed that the power screw claw offered high grip strength, simple construction, and straightforward integration with the boom.

The gripper arms were driven by a stepper motor through a threaded rod and sled. As the rod rotated, the sled translated vertically and forced the gripper arms open or closed. A fine thread rod was used to increase mechanical advantage, giving the claw the grip force needed for the strength-to-weight objective.

The boom was simplified from an early design with horizontal positioning to a fixed gripper mount. This reduced cost, reduced weight, and allowed the gripper structure to be integrated directly into the boom. With the gripper mounted 10 inches from the center of rotation, the manipulator could still translate objects roughly 20 inches through a 180 degree base rotation.

The vertical lift evolved from guide post concepts to an off-the-shelf linear rail and linear actuator. This choice reduced binding under load and improved vertical positioning repeatability. The rotating base used a machined aluminum bearing housing, aluminum shaft, mounting plate, ring gear, and pinion gear to keep the base stiff and accurate under cantilevered load.

## Manufacturing

The rotating base used machined aluminum for stiffness, low weight, and precise bearing alignment. The boom structure used laser-cut acrylic panels, while complex gripper and mounting components were 3D printed in PLA. Several high load printed components were increased to 100% infill after testing revealed fatigue or deflection concerns.

Important manufacturing and integration work included:

- Machining an aluminum shaft coupler to replace an early printed coupler.
- Printing enlarged gripper arms at 100% infill after early fatigue failures.
- Laser cutting acrylic chassis panels and gears.
- Machining the aluminum bearing housing and rotating shaft to reduce play.
- Integrating stepper motors, an Arduino controller, motor drivers, a linear actuator, and wiring onto the rotating structure.

The final material cost was $265.57, which was $34.43 below the $300 budget limit.

## Results

The finished robot passed the required 3D Benchy pick-and-place test by lifting the object over the required 2 inch height and placing it in the target location. It also completed repeated hockey puck placement tests, including a preprogrammed sequence that stacked six pucks into a pyramid and returned them to the original stack.

For the strength-to-weight test, the robot weighed approximately 6.8 lb and successfully lifted a 16.6 lb load before failing at the next test increment. This produced a final strength-to-weight ratio of 2.44:1, well above the original 1:1 target.

<div class="article-video">
  <iframe
    src="https://www.youtube.com/embed/x0Dt4iPqRhA"
    title="Gripper Robot lifting a barbell for strength-to-weight testing"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
  ></iframe>
</div>

Precision testing used a 3 inch diameter hockey puck and printed circular targets. Across repeated runs, the puck stayed within or on the 3 inch target line, keeping placement error well below the 2% radius requirement.

Out of 23 competing teams, the project won 1st place for design.

<figure class="article-figure">
  <img src="/images/projects/group_photo.JPG" alt="Gripper Robot team after winning first place for design" />
  <figcaption>Team photo with the completed gripper robot after winning 1st place for design.</figcaption>
</figure>

## Future Improvements

The final report identified several possible improvements. The vertical lift could be strengthened by adding a second linear actuator and a second linear slide to reduce rail deflection under heavier loads. Accuracy could also be improved by adding smoother acceleration and deceleration profiles to the rotating stepper motor, reducing jerk when starting or stopping under load.
