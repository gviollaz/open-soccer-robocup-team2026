---
title: "Mejoras en el sistema de visión para 2026"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
status: backlog
priority: alta
tags: [vision, openmv, mejoras]
---

# Mejoras en el sistema de visión para 2026

## Qué investigar

- Evaluar si la cámara OpenMV H7 es suficiente o conviene H7 Plus
- Mejorar la calibración de thresholds (actualmente manual con script interactivo)
- Optimizar la detección de arcos (actual: 1 o 2 arcos por color)
- Evaluar uso de transformación homográfica vs. métodos más simples
- Investigar detección por forma (roundness) vs. solo color
- Optimizar FPS del pipeline de visión

## Por qué es importante

La visión es el sensor principal del robot. Mejor visión = mejor toma de decisiones.
