---
title: "Revisión completa del código 2025"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
status: backlog
priority: alta
tags: [software, revision, legado]
---

# Revisión completa del código 2025

## Qué investigar

- Revisar todo el código en `legacy/2025-season/` para entender qué funciona y qué no
- Identificar bugs conocidos (ej: `potencia` no definida en el código del arquero)
- Documentar la arquitectura actual del software
- Listar qué funcionalidades están completas vs. incompletas

## Por qué es importante

El nuevo equipo necesita entender el código existente antes de mejorarlo. Hay código duplicado y versiones incompletas que hay que limpiar.

## Observaciones preliminares

- El código del arquero tiene una variable `potencia` que nunca se define
- Hay dos bloques `setup()`/`loop()` en el archivo del arquero (parece WIP)
- La máquina de estados del delantero es funcional pero básica
- El protocolo UART está hardcodeado (headers 201/202)
- La librería zirconLib soporta 2 versiones de PCB pero el compass está deshabilitado
