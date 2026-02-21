---
title: "Kickoff del proyecto temporada 2026"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
ai-assisted: true
ai-tool: "Claude (Anthropic)"
status: final
tags: [organizacion, kickoff, migracion]
---

# Kickoff del proyecto - Temporada 2026

## Contexto

El equipo IITA se clasificó como campeón nacional en la Roboliga Argentina (diciembre 2025) y representará a Argentina en RoboCup 2026 en Incheon, Corea del Sur (30 jun - 6 jul 2026). El equipo Soccer Open está formado por María Virginia Viollaz (veterana 2025) y Elías Cordero (nuevo integrante, estudiante de Ing. Electromecánica en UNSa).

## Trabajo realizado

- Creación del repositorio `open-soccer-robocup-team2026` con estructura profesional
- Definición de convenciones: atribución (humano, humano+IA, IA supervisada), naming, frontmatter
- Creación de `AI-INSTRUCTIONS.md` para normalizar trabajo con cualquier IA
- Migración del código 2025: arquero, delantero, visión OpenMV, control, zirconLib
- Archivos binarios (STL, GCode) referenciados por link al repo original
- Revisión completa del código 2025 con identificación de 8 bugs
- Cronograma actualizado con fechas reales de RoboCup 2026
- Documentación de cambios en reglas 2026 (pelota nueva 42mm, BOM obligatorio)
- Backlog de investigación con 6 temas prioritarios

## Decisiones tomadas

- Separar journal (cronológico) de research (temático) de testing (experimental)
- Pipeline de research: backlog → in-progress → completed
- Legacy como read-only: código 2025 intacto como referencia
- Atribución obligatoria en cada commit

## Bugs críticos identificados en código 2025

1. Arquero no compila (variable `potencia` no definida, doble setup/loop)
2. Bug lógico en estado PATEANDO del delantero (`<=` en vez de `>=`)
3. Giróscopo deshabilitado en todo el código
4. Delantero sin detección de línea (se sale de cancha)

Ver análisis completo: `research/completed/2026-02-21-revision-codigo-2025.md`

## Próximos pasos

1. Transferir repo de `gviollaz` a `IITA-Proyectos`
2. Analizar reglas 2026 draft completas (pelota 42mm es cambio crítico)
3. Evaluar estado físico del hardware (placa Zircon, motores, cámara)
4. Definir plan de mejoras priorizadas para los ~4 meses hasta la competencia
5. Gestionar financiamiento del viaje a Corea del Sur
