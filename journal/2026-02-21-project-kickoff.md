---
title: "Kickoff del proyecto temporada 2026"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
ai-assisted: true
ai-tool: "Claude (Anthropic)"
status: final
tags: [organizacion, kickoff, migracion, reglas]
---

# Kickoff del proyecto - Temporada 2026

## Contexto

El equipo IITA se clasificó como campeón nacional en la Roboliga Argentina (diciembre 2025) y representará a Argentina en RoboCup 2026 en Incheon, Corea del Sur (30 jun - 6 jul 2026). El equipo Soccer Vision está formado por María Virginia Viollaz (veterana 2025, 18 años) y Elías Cordero (estudiante de Ing. Electromecánica en UNSa).

## Trabajo realizado

### Repositorio y estructura
- Creación del repositorio `open-soccer-robocup-team2026`
- Estructura profesional con separación: journal / research / testing / hardware / software / competition / legacy
- Convenciones de atribución (humano, humano+IA, IA supervisada)
- `AI-INSTRUCTIONS.md` para normalizar trabajo con cualquier IA
- `CONTRIBUTING.md` con reglas de commits y formato

### Migración de código 2025
- Arquero, delantero, visión OpenMV, control, zirconLib migrados
- Archivos binarios (STL, GCode) referenciados por link

### Revisión de código 2025
- Análisis completo: 8 bugs identificados
- Bug crítico: arquero no compila (`potencia` no definida)
- Bug lógico: estado PATEANDO del delantero
- Giróscopo BNO055 deshabilitado en todo el código
- Ver: `research/completed/2026-02-21-revision-codigo-2025.md`

### Análisis de reglas 2026
- Reglas draft completas leídas y analizadas
- **Cambio crítico**: zona de captura de pelota reducida de 3.0cm a 1.5cm
- Communication Module obligatorio en competencia internacional
- Documentación obligatoria: BOM (con precios), poster A1, video, portfolio, código compartido
- Scoring incluye entrevistas (cada miembro debe explicar su rol)
- Liga renombrada: Soccer Open → Soccer Vision
- Ver: `research/completed/2026-02-21-analisis-reglas-2026.md`

### Referencias y recursos
- TDPs 2025, awesome-rcj-soccer, communication module, simulador Webots
- Equipo de referencia: ducc (Singapore) con repo público completo
- Ver: `research/references/README.md`

## Decisiones tomadas

- Separar journal (cronológico) de research (temático) de testing (experimental)
- Pipeline de research: backlog → in-progress → completed
- Legacy como read-only
- Atribución obligatoria en cada commit

## Backlog de investigación

| Prioridad | Tema | Estado |
|-----------|------|--------|
| ALTA | Revisión código 2025 | ✅ Completado |
| ALTA | Análisis reglas 2026 | ✅ Completado |
| ALTA | Mejoras mecánicas (dribbler 1.5cm!) | Backlog |
| ALTA | Mejoras de visión | Backlog |
| ALTA | Communication Module | Backlog |
| ALTA | Financiamiento viaje | Backlog |
| MEDIA | Mejoras electrónica | Backlog |
| MEDIA | Documentación obligatoria | Backlog |

## Próximos pasos

1. Transferir repo de `gviollaz` a `IITA-Proyectos`
2. Evaluar hardware físico (robots, placa Zircon, motores, cámara)
3. Verificar zona de captura del dribbler (1.5cm)
4. Investigar Communication Module e integrarlo
5. Resolver integración del BNO055
6. Gestionar financiamiento del viaje a Corea del Sur
7. Definir roles técnicos para la entrevista
