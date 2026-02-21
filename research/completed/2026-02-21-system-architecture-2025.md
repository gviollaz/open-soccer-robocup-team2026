---
title: "Robot System Architecture — 2025 Season"
date: 2026-02-21
updated: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
ai-assisted: false
ai-tool: "Claude (Anthropic - Claude Opus 4.6)"
status: final-verified
tags: [architecture, hardware, software, openmv, teensy, protocol, sensors, analysis]
lang: en
translations:
  es: "./2026-02-21-system-architecture-2025.es.md"
legacy-es: "./2026-02-21-arquitectura-sistema-2025.md"
note: "Updated post cross-verification against real source code. Hypothesis #12 REFUTED. Added findings N1, N2, N3."
---

# Robot System Architecture — 2025 Season

## Complete Technical Analysis for 2026 Planning

**Team**: IITA RoboCup Junior Soccer Open (now Soccer Vision)
**Analysis author**: Claude (Anthropic — Claude Opus 4.6)
**Supervision**: Gustavo Viollaz (@gviollaz)
**Date**: February 21, 2026
**Last update**: February 21, 2026 — post cross-verification against source code
**Sources**: Repositories `IITA-Proyectos/RoboCupJunior-Soccer-Open-League-2025` and `IITA-Proyectos/open-soccer-robocup-team2026`

> ⚠️ **Verification status**: This document was verified line by line against real source code from both repositories (2025 and 2026). Hypothesis #12 was corrected (REFUTED) and 3 new findings were added (N1, N2, N3). See [complete cross-analysis](2026-02-21-cross-verification-hypothesis-analysis.md) for methodology and details.

---

## 1. Executive Summary

The 2025 system consists of two robots (goalie and striker) built on the Zircon controller board (in-house design in two variants: Mark1 and Naveen1) with a Teensy 4.1 microcontroller, OpenMV H7 camera as the primary vision sensor, BNO055 gyroscope for orientation, reflective line sensors for boundary detection, IR sensors for ball detection, and an HC-SR04 ultrasonic sensor for distance on the goalie.

The team won the national championship in December 2025 with this system. However, the analysis reveals **significant structural software issues** that must be resolved before competing internationally in Incheon (June-July 2026).

### Findings

A total of **23 original failure points** + **3 new verification findings** were identified, categorized as: software bugs (8), design deficiencies (7), protocol vulnerabilities (5), risks from 2026 rule changes (3), and cross-verification findings (3).

Of the 23 originals: **19 confirmed**, **3 partially confirmed**, **1 refuted** (#12).

---

## 2. System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        ROBOT (Goalie or Striker)                │
│                                                                 │
│  ┌──────────────┐     UART (Serial1)      ┌──────────────────┐ │
│  │  OpenMV H7   │ ───────────────────────► │   Teensy 4.1     │ │
│  │  (Camera)    │   19200 baud            │   (Controller)   │ │
│  │              │   Proprietary protocol  │                  │ │
│  │  - RGB565    │   [Header][Data][Data]   │  ┌────────────┐ │ │
│  │  - QVGA      │                          │  │ zirconLib  │ │ │
│  │  - find_blobs│                          │  │ (HAL)      │ │ │
│  │  - Homography│                          │  └─────┬──────┘ │ │
│  └──────────────┘                          │        │        │ │
│                                            │        ▼        │ │
│                              ┌─────────────┤  ┌──────────┐  │ │
│  ┌──────────────┐            │             │  │ Motors   │  │ │
│  │  BNO055      │◄───── I2C ┤             │  │ x3 (120°)│  │ │
│  │  (Gyroscope) │            │             │  └──────────┘  │ │
│  └──────────────┘            │             │                │ │
│                              │  Zircon PCB │  ┌──────────┐  │ │
│  ┌──────────────┐            │  (Mark1 or  │  │ Sensors  │  │ │
│  │  HC-SR04     │◄── GPIO ──┤   Naveen1)  │  │ IR x8    │  │ │
│  │  (Ultrasonic)│            │             │  └──────────┘  │ │
│  └──────────────┘            │             │                │ │
│                              │             │  ┌──────────┐  │ │
│  ┌──────────────┐            │             │  │ Line x3  │  │ │
│  │  Dribbler    │◄── PWM ───┤             │  │ (Analog) │  │ │
│  │  (DC Motor)  │            │             │  └──────────┘  │ │
│  └──────────────┘            └─────────────┤                │ │
│                                            │  ┌──────────┐  │ │
│                                            │  │Buttons x2│  │ │
│                                            │  └──────────┘  │ │
│                                            └────────────────┘ │
│                                                                 │
│  ⚠️ 2026: Missing Communication Module (mandatory international)│
└─────────────────────────────────────────────────────────────────┘
```

---

## 3. Hardware Layer

### 3.1 Microcontroller: Teensy 4.1

- **MCU**: NXP i.MX RT1062, ARM Cortex-M7 at 600 MHz
- **RAM**: 1024 KB, Flash: 8 MB
- **Serial ports**: 8 hardware UARTs (Serial1 used for OpenMV)
- **ADC**: Multiple analog channels (used for IR and line sensors)
- **PWM**: Multiple channels (motor and dribbler control)
- **I2C**: Bus for BNO055 gyroscope

**Observation**: The Teensy 4.1 has more than enough capacity for what is demanded of it. The bottleneck is not the processing hardware but the software.

### 3.2 Controller Board: Zircon PCB

The Zircon board is a PCB designed for RoboCup Junior that acts as a shield/carrier for the Teensy. It exists in **two variants** with different pinouts:

| Component | Mark1 | Naveen1 |
|-----------|-------|---------|
| Motor 1 | INA=2, INB=5, PWM=3 | DIR1=3, DIR2=4 (no separate PWM) |
| Motor 2 | INA=8, INB=7, PWM=6 | DIR1=6, DIR2=7 |
| Motor 3 | INA=11, INB=12, PWM=4 | DIR1=5, DIR2=2 |
| Ball IR 1-4 | 14, 15, 16, 17 | 20, 21, 14, 15 |
| Ball IR 5-8 | 20, 21, 22, 23 | 16, 17, 18, 19 |
| Buttons | 9, 10 | 8, 10 |
| Line 1-3 | A11, A13, A12 | A8, A9, A12 |
| Version detection | Pin 32 LOW = Mark1 | Pin 32 HIGH = Naveen1 |

**Key difference in motor control**:
- **Mark1**: Uses classic DIR + DIR + PWM scheme (3 pins per motor, 9 total). Direction is set with `digitalWrite` and speed with `analogWrite` on the PWM pin.
- **Naveen1**: Uses direct H-Bridge scheme with 2 PWM pins per motor (6 pins total). Speed and direction are both controlled with `analogWrite` on the direction pins.

**⚠️ Failure point #1** *(partially confirmed)*: Version detection uses `INPUT_PULLDOWN` (not a purely floating pin). The Teensy 4.1's internal ~100kΩ pulldown significantly mitigates noise risk. Reasonably reliable if the pin is connected to VCC (Naveen1) or left open (Mark1). The risk exists but is lower than originally indicated.

**Suggestion**: Add a mandatory `Serial.println(getZirconVersion())` in every program's `setup()`, and verify visually before each match.

### 3.3 Motor Configuration (3-Wheel Omnidirectional)

The robot uses 3 DC motors with omnidirectional wheels at 120° apart. The library exposes `motor1(power, direction)`, `motor2(power, direction)`, `motor3(power, direction)` with power 0-100 and direction 0/1.

**⚠️ Failure point #2**: There is no unified omnidirectional movement function `move(angle, speed)`. Each program reimplements motor combinations differently and inconsistently. This is a primary source of bugs.

### 3.4 Sensors

#### 3.4.1 Line Sensors (x3 — Analog Reflective)

**⚠️ Failure point #3**: Ranges are hardcoded with exact values calibrated for ONE specific field. In international competition, lighting conditions will be completely different.

**⚠️ Failure point #4**: Only 3 line sensors cover a very limited angle.

#### 3.4.2 Ball IR Sensors (x8 — Infrared Photodiodes)

**⚠️ Failure point #5**: In the current striker code, the IR sensors **are not used**. The 8 IR sensors are installed but **wasted** in the competition program.

#### 3.4.3 Gyroscope: Adafruit BNO055

**⚠️ Failure point #6** *(confirmed)*: The variable `compassCalibrated` initializes as `false` and **is never set to `true`**. The function `readCompass()` always returns 0. The BNO055 was **DELIBERATELY COMMENTED OUT** before the competition. The robot won the nationals WITHOUT orientation control.

#### 3.4.4 Ultrasonic Sensor: HC-SR04 (goalie only)

**⚠️ Failure point #7**: `pulseIn(ECHO, HIGH)` is **blocking** — halts the entire program for up to 1 second if there is no echo.

---

## 4. Software Layer — OpenMV (Vision)

**⚠️ Failure points #8-9** *(confirmed)*: Thresholds with L_min > L_max break blob detection.

**⚠️ Failure point #10**: Two different homography matrices (h=10cm vs h=18.7cm).

---

## 5. Communication Protocol OpenMV → Teensy

> **⚠️ CORRECTION — Failure point #12 REFUTED** (post-verification update, Feb 21, 2026)
>
> The protocol was designed with intentional separation: data in range 0–200, headers 201+. **No collision risk.**
>
> **Real protocol problems that DO persist:** #13 (no checksum/CRC), #14 (fixed-block reading without resync), no resynchronization, no timeout.

---

## 6. Software Layer — Teensy (Control)

**⚠️ Failure point #15-16** *(confirmed)*: Goalie does not compile, sensors read garbage.
**⚠️ Failure point #18** *(confirmed)*: Kick timeout inverted.
**⚠️ Failure point #19** *(confirmed)*: Goal angle uses ball coordinates.
**⚠️ Failure point #21**: No high-level movement functions in zirconLib.
**⚠️ Failure point #22** *(confirmed)*: Dribbler was NEVER automatically activated.

---

## 7. New Cross-Verification Findings

**🆕 N1 — Pin 0/RX1 Conflict in Naveen1 Mode (HIGH)**: `initializePins()` executes `pinMode(0, OUTPUT)` — Pin 0 is RX1 (Serial1 receive). Works by accident because `Serial1.begin()` reconfigures afterward.

**🆕 N2 — Truncated Migration (MEDIUM)**: Multiple critical files from 2025 repo are absent or stubs.

**🆕 N3 — Competition Code Not in Repository (HIGH)**: Converging evidence that the code that ran at nationals differs from the repo.

---

## 8. Failure Points Summary

| Metric | Result |
|--------|--------|
| Confirmed | 19/23 (83%) |
| Partial | 3/23 (13%) |
| Refuted | 1/23 (4%) |
| New | 3 |
| Total active | 25 |

See **[Revised Priority Map](2026-02-21-priority-map-revised.md)** for the action plan.

---

## 9. Sensor Inventory

| Sensor | Qty. | Interface | Used in 2025 |
|--------|------|-----------|-------------|
| OpenMV H7 | 1 | UART | ✅ Striker |
| BNO055 | 1 | I2C | ⚠️ Commented out |
| HC-SR04 | 1 | GPIO | ✅ Goalie |
| IR Ball | 8 | ADC | ❌ Not used |
| Line | 3 | ADC | ✅ Goalie |
| Buttons | 2 | GPIO | Partial |
| Dribbler | 1 | PWM | ❌ No auto activation |

---

*Document generated by Claude (Anthropic — Claude Opus 4.6) under supervision of Gustavo Viollaz (@gviollaz), February 21, 2026.*
*Updated post cross-verification against real source code, February 21, 2026.*
