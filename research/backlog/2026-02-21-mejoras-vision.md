---
title: "Mejoras de visión para 2026"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
status: backlog
priority: alta
tags: [vision, openmv, camara, software]
---

# Mejoras de visión para 2026

## Qué investigar

- Evaluar OpenMV H7 vs H7 Plus (resolución, FPS, procesamiento)
- Mejorar la calibración de threshold (actualmente manual y hardcodeada)
- Optimizar detección de arcos (actualmente solo se envía posición de 1 arco)
- Mejorar robustez ante cambios de iluminación
- Evaluar si se necesita auto-calibración o calibración adaptativa
- Optimizar FPS para respuesta más rápida
- Considerar detección de robots oponentes

## Notas de las reglas 2026

- El robot debe poder lidiar con colores visibles por encima de las paredes (camisetas)
- Se permite cualquier número de cámaras sin restricción de lentes
- Para SuperTeam en campo grande puede convenir lentes optimizados

## Estado actual (2025)

- Cámara OpenMV detecta pelota naranja por color LAB
- Transformación homográfica para coordenadas físicas
- Thresholds hardcodeados: `(30, 60, 20, 60, 10, 50)` para naranja
- Herramienta de calibración interactiva disponible
- Scripts separados para H7 y H7 Plus

## Mejoras potenciales

1. Calibración automática al encender (muestrear pelota en posición conocida)
2. Detección de ambos arcos simultáneamente
3. Tracking de pelota con predicción de trayectoria
4. Detección de líneas de cancha por visión (complemento a sensores de línea analógicos)
5. Detección de robots oponentes para evasión
