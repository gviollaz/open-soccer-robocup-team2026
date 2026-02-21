---
title: "Kickoff del proyecto temporada 2026"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
ai-assisted: true
ai-tool: "Claude (Anthropic)"
status: final
tags: [organizacion, kickoff]
---

# Kickoff del proyecto - Temporada 2026

## Objetivo

Crear la estructura organizativa del repositorio para la temporada 2026 de RoboCupJunior Soccer Open League.

## Trabajo realizado

- Creación del repositorio `open-soccer-robocup-team2026`
- Diseño de la estructura de carpetas separando:
  - Journal de ingeniería (cronológico)
  - Pipeline de investigación (backlog → in-progress → completed)
  - Testing (protocolos + resultados)
  - Hardware (electrónica, eléctrica, mecánica)
  - Software (por robot + visión + comunicación + librerías)
  - Competencia (reglas, cronograma)
  - Legacy (preservación del trabajo 2025)
- Definición de convenciones de atribución (humano, humano+IA, IA supervisada)
- Creación de `AI-INSTRUCTIONS.md` para que cualquier IA pueda operar correctamente
- Migración del código de la temporada 2025 como referencia histórica

## Decisiones tomadas

- **Separar journal de documentación**: El journal es cronológico (qué pasó cada día), la documentación de research es temática (qué se investigó sobre cada tema)
- **Pipeline de research con estados**: backlog → in-progress → completed permite trackear qué está pendiente y qué ya se analizó
- **Legacy como read-only**: El código 2025 se preserva intacto como referencia, el código nuevo va en software/
- **Atribución obligatoria**: Todo commit indica quién (humano o IA) y cómo (solo, asistido, supervisado)

## Próximos pasos

- Transferir el repo de gviollaz a la organización IITA-Proyectos
- Completar datos del equipo 2026 en README.md
- Definir cronograma en competition/timeline.md
- Identificar temas prioritarios de investigación y agregarlos al backlog
- Revisar el código legacy para identificar qué funciona y qué hay que mejorar
