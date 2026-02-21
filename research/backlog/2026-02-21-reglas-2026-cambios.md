---
title: "Análisis de cambios en reglas 2026"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
status: backlog
priority: alta
tags: [reglas, competencia, 2026]
---

# Análisis de cambios en reglas 2026

## Qué investigar

- Leer completas las reglas 2026 draft: https://robocup-junior.github.io/soccer-rules/2026-soccer-draft-rules/rules.html
- Comparar con reglas 2025 para identificar todos los cambios
- Extraer restricciones físicas exactas (peso, diámetro, altura, voltaje)
- Entender el impacto de la nueva pelota de 42mm (era 74mm)
- Documentar los requisitos de documentación (BOM, poster, entrevista)
- Verificar si hay cambios en reglas de comunicación entre robots

## Cambios ya identificados

- **Pelota IR nueva**: 42mm diámetro (antes 74mm). Open source: https://github.com/robocup-junior/ir-golf-ball
- **BOM obligatorio**: Lista de componentes con proveedor y estado
- **Roles técnicos definidos**: Cada miembro debe poder explicar su rol
- **Crédito obligatorio**: Para código y recursos externos

## Por qué es urgente

La pelota nueva de 42mm es un cambio significativo. Si el robot actual está calibrado para una pelota de 74mm, hay que recalibrar:
- Detección visual (blob más pequeño en la imagen)
- Detección IR (si se usa sensor IR además de visión)
- Mecánica del dribbler (tamaño de contacto)
- Mecanismo de kick
