---
title: "Mejoras de electrónica para 2026"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
status: backlog
priority: media
tags: [electronica, pcb, zircon, bno055, drivers]
---

# Mejoras de electrónica para 2026

## Qué investigar

- Evaluar estado de la placa Zircon (versión Mark1 vs Naveen1)
- **Integrar BNO055** (giróscopo/IMU) — está en el código pero deshabilitado
- Evaluar drivers de motor (eficiencia, calentamiento)
- Evaluar sistema de alimentación (batería, reguladores)
- **NUEVO**: Integrar conector para Communication Module (GPIO + alimentación 500mA)
- Evaluar sensores de línea actuales (3 sensores analógicos)
- Evaluar sensor ultrasónico del arquero

## Requisitos del Communication Module

- Pin GPIO 2.54mm para start/stop
- Alimentación directa a batería (5.3V-25V) sin switch
- 500mA mínimo en todo momento
- Alternativa: pin 3V3 si voltaje fuera de rango
- Siempre encendido (incluso robot fuera del campo)

## Preguntas clave

1. ¿Por qué el BNO055 está deshabilitado? ¿Hubo problemas de I2C? ¿Conflictos de pines?
2. ¿La Zircon tiene pines libres para el Communication Module?
3. ¿La batería actual puede suministrar 500mA adicionales?
4. ¿Se necesita nueva versión de PCB o se puede adaptar la existente?
5. ¿Los sensores de línea son suficientes o necesitamos más cobertura?

## BOM (para ir armando)

_Empezar a registrar componentes con proveedor y precio:_

| Componente | Cantidad | Proveedor | Precio | Nuevo/Reusado | Kit/Custom |
|-----------|----------|-----------|--------|---------------|------------|
| Teensy | 2 | _pendiente_ | _pendiente_ | Reusado 2025 | Kit |
| OpenMV H7 | 2 | _pendiente_ | _pendiente_ | Reusado 2025 | Kit |
| BNO055 | 2 | _pendiente_ | _pendiente_ | Reusado 2025 | Kit |
| Placa Zircon | 2 | Custom IITA | _pendiente_ | Reusado 2025 | Custom |
| Motores TT | 6 | _pendiente_ | _pendiente_ | _pendiente_ | Kit |
| _agregar más..._ | | | | | |
