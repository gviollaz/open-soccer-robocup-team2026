---
title: "Complete Rules Analysis — Soccer 2026"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
status: completed
priority: high
tags: [rules, competition, 2026, analysis]
lang: en
translations:
  es: "./2026-02-21-rules-analysis-2026.es.md"
legacy-es: "./2026-02-21-analisis-reglas-2026.md"
---

# Complete Rules Analysis — RoboCupJunior Soccer 2026

Source: https://robocup-junior.github.io/soccer-rules/2026-soccer-draft-rules/rules.html
Rules date: 2026-01-21

## Name Changes

- **Soccer Open** is now called **Soccer Vision**
- **Soccer Lightweight** is now called **Soccer Infrared**
- Our team competes in **Soccer Vision**

## Robot Physical Restrictions (Soccer Vision)

| Parameter | Limit | Notes |
|-----------|-------|-------|
| Maximum diameter | **18.0 cm** | Must fit smoothly into a cylinder of this diameter |
| Maximum height | **18.0 cm** | Measured with all parts extended |
| Maximum weight | **No limit** | Soccer Vision has no weight restriction |
| Ball capture zone | **1.5 cm** | ⚠️ **CHANGED from 3.0 cm to 1.5 cm** — the ball cannot enter more than 1.5 cm into the convex hull of the robot |
| Maximum voltage | 48V DC / 25V AC RMS | Must be measurable during inspections |
| Inter-robot communication | 2.4 GHz, max 100 mW EIRP | Spectrum is not guaranteed |

**Official tip**: Being 3mm under the size limit and 50g under the weight limit makes inspections smoother.

## Ball

- **Soccer Vision uses a passive orange golf ball** (42mm diameter) — **NO CHANGE for our league**
- The ball size change from 74mm to 42mm is only for the Infrared league
- But the **capture zone changed from 3.0 to 1.5 cm** — this DOES affect us

## Ball Capture Zone — CRITICAL CHANGE

The capture zone was halved. This means:
- The dribbler CANNOT embrace the ball as much as before
- The mechanical design of the robot's front must be reviewed
- The ball must be accessible to other robots at all times
- Defined as: internal space created by placing a straight edge across the protruding points of the robot

## Communication Module — MANDATORY for International

- The Soccer League Committee provides a communication module to each team
- Interface: **one 2.54mm GPIO pin** for start/stop
- The module may exceed the robot's maximum height
- Must be **at least 1 cm** from the robot's exterior
- Must be protected from impacts
- Power: GND and BAT+ direct to battery (5.3V-25V) without switch. Must supply **500 mA** at all times
- Must be powered on at all times during the match (even when the robot is off the field)
- If battery voltage is not between 5.3V-25V, a 3V3 pin may be used
- **Does not count toward weight limit**
- More info: https://github.com/robocup-junior/soccer-communication-module
- Future: they plan to extend to UART for more complex applications

## Handle (Transport Handle)

- MANDATORY: stable and visible handle
- Must allow lifting the robot from at least **5 cm above** the highest structure
- Minimum **5 cm of clearance** for hands between the top of the robot and the handle
- The handle may exceed the height limit
- Handle weight counts toward total robot weight

## Top Markers

- White plastic circle of at least **4 cm diameter**
- Mounted horizontally on top
- The referee writes numbers on it with a marker
- Does not need to be within the size limit
- Robots without a top marker cannot play

## Prohibited Colors on the Robot

- **Orange, yellow, or blue** visible colors are not allowed
- Parts of those colors must be hidden or painted/covered with a neutral color
- Must not produce visible light that interferes with the opponent's vision
- Must not produce magnetic interference

## Kicker — Power Test

1. Place the robot inside the goal, touching the back wall
2. Perform a kick toward the opposite goal
3. **Passes the test if**: after bouncing off the opposite goal, the ball does NOT return all the way to the back wall of the goal it was kicked from

This is more permissive than in 2025 (previously the ball could not cross the penalty area line).

## Gameplay — Key Points

- 2 robots per team, 2 halves of 10 minutes with 5 min half-time
- Arrive 5 min before the match (penalty: 1 goal per 30 sec delay)
- Maximum score difference in the final score: 10 goals
- Out of bounds: 1 minute penalty (if touching wall or fully entering the area)
- Damaged robot: minimum 1 minute out or until next kickoff
- If both robots from the same team are damaged: 1 goal every 30 sec to the opponent
- Robots MUST be able to approach and touch the ball (or they are considered damaged)
- Pushing: if attacker and defender touch each other in the penalty area with ball contact

## Mandatory Documentation

1. **Technical poster**: maximum A1 (60×84 cm). Design summary. Displayed at the venue and shared afterward.
2. **Technical Description Video**: demo of the working robot, design process, innovation
3. **BOM (Bill of Materials)**: component name, supplier, status (new/reused), kit/custom, **price**
4. **Digital portfolio**: submitted before the competition
5. **Code and designs**: MANDATORY to share hardware and software in RoboCupJunior GitHub repos

## Interviews

- Conducted during Setup Day
- Each member must have a **defined technical role** (mechanical, electronics, software)
- Must be able to explain not only WHAT each component does but HOW it works
- The interview is part of the scoring

## Technical Challenges

- Additional technical challenges (unknown until competition day)
- Any team may participate
- May influence future rules

## SuperTeam

- Competition where teams from different countries share robots
- Played on a larger field ("Big Field")
- May require lenses/sensors optimized for larger fields
- Rules: https://robocup-junior.github.io/soccer-rules/master/superteam_rules.html

## Team — Requirements

- 2-4 members (they plan to increase to 5 in the future)
- Age: 14-19 years old as of July 1, 2026
- Mentor: 19+ years old, must be present during all official events
- At least 2 members present on site (1 alone can compete but cannot be eligible for finals/awards)
- Remote assistance is prohibited during the competition (calls, video calls, remote desktop)

## Specific Changes 2025 → 2026

| Change | Before (2025) | Now (2026) | Impact for us |
|--------|--------------|------------|---------------|
| League name | Soccer Open | Soccer Vision | Name only |
| Ball capture zone | 3.0 cm | **1.5 cm** | **CRITICAL** — review dribbler |
| IR league weight | 1400g | 1500g | Does not affect us (Vision) |
| IR ball | 74mm | 42mm | Does not affect us (we already use golf ball) |
| Pushing + multiple defense | Unspecified | Pushing resolved first | Gameplay rule |
| Kicker test | Request kick sample | Test kicker power | More formal |

## Conclusions and Actions

1. **URGENT**: Verify that the dribbler complies with the 1.5 cm capture zone (was 3.0 cm)
2. **IMPORTANT**: Integrate the Communication Module into the electronic design (GPIO pin + 500 mA power)
3. **IMPORTANT**: Prepare handle with 5 cm clearance
4. **IMPORTANT**: Prepare white top marker of 4 cm
5. **NEEDED**: Start assembling BOM with prices
6. **NEEDED**: Each member must be able to explain their technical role in the interview
7. **PREPARE**: A1 poster, technical video, digital portfolio, code for sharing
