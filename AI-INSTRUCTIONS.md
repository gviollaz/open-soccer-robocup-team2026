# Instructions for AI Assistants

> This file is intended for any language model (LLM) or AI assistant working with this repository.
> 🇪🇸 Versión en español: [AI-INSTRUCTIONS.es.md](AI-INSTRUCTIONS.es.md)

## Project Context

This repository belongs to a student team from IITA (Instituto de Informática y Tecnología Aplicada) in Salta, Argentina, competing in the **RoboCupJunior Soccer Vision League 2026** (formerly Soccer Open League).

The team builds and programs **2 autonomous robots** (a goalie and a striker) that play soccer on a regulation field. The robots use:

- **Microcontrollers**: Teensy / Arduino
- **Vision**: OpenMV cameras (H7 / H7 Plus) with MicroPython
- **Communication**: UART between OpenMV and Teensy
- **Sensors**: Line (infrared), gyroscope/IMU, ultrasonic
- **Actuators**: TT motors with H-bridge drivers, servos, dribbler, solenoid kicker
- **Structure**: 3D printing (Tinkercad/OpenSCAD) + manual construction
- **Custom PCB**: "Zircon" board with in-house library (zirconLib)

## Rules You Must Follow

### 1. Attribution

Every change you make **must** include in the commit message:
```
Author: [Your AI name] ([Provider])
Requested-by: [Name of the human who asked you]
```

### 2. Structure

- Respect the existing folder structure
- New files: `kebab-case` convention
- Dated files: `YYYY-MM-DD-description.md`
- Include YAML frontmatter in every `.md` file

### 3. Language & Translations

- **English is the canonical language** for all documentation
- Spanish translations are welcome as `.es.md` files (see `TRANSLATIONS.md`)
- Filenames always in English (for new files)
- Code comments in English; brief Spanish clarifications in parentheses are acceptable
- Include `lang: en` or `lang: es` in frontmatter

### 4. Engineering Journal

When documenting work or analysis, create an entry in `journal/` with the format:
```
journal/YYYY-MM-DD-description.md
```

### 5. Research

- New topics to investigate → `research/backlog/`
- In-progress analysis → `research/in-progress/`
- Completed analysis → `research/completed/`
- External references → `research/references/`

### 6. Testing

- Test protocols → `testing/protocols/`
- Results → `testing/results/YYYY-MM-DD-description.md`

### 7. Code

- **Do not modify** files in `legacy/` (they are historical reference)
- New code goes in `software/` in the corresponding subfolder
- Always comment the code
- Indicate whether the code was tested on real hardware

### 8. Hardware

- Schematics and PCB → `hardware/electronics/`
- Batteries, motors, power → `hardware/electrical/`
- 3D designs and assembly → `hardware/mechanical/`

## Key Files to Read First

1. `README.md` — Project overview
2. `CONTRIBUTING.md` — Contribution rules and attribution
3. `TRANSLATIONS.md` — Language and translation policy
4. `competition/timeline.md` — Schedule and deadlines
5. `legacy/2025-season/README.md` — What was done last year

## Tag Conventions

Use these tags in `.md` file frontmatter:

**Area**: `vision`, `mobility`, `control`, `dribbler`, `communication`, `electronics`, `mechanical`, `battery`, `sensors`
**Type**: `analysis`, `tutorial`, `protocol`, `result`, `decision`, `comparison`
**Robot**: `goalie`, `striker`, `both`
**Priority**: `high`, `medium`, `low`
