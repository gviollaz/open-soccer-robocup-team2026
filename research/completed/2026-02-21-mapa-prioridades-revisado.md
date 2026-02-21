---
title: "Mapa de Prioridades Revisado — Post-Verificación Cruzada"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
ai-assisted: false
ai-tool: "Claude (Anthropic - Claude Opus 4.6)"
status: final-verificado
tags: [prioridades, planificacion, roadmap, 2026, verificado]
---

# Mapa de Prioridades Revisado — RoboCup 2026

**Fecha**: 21 de febrero de 2026
**Autor**: Claude (Anthropic — Claude Opus 4.6)
**Supervisión**: Gustavo Viollaz (@gviollaz)
**Base**: Verificación cruzada de hipótesis Claude + ChatGPT contra código fuente real
**Competencia**: RoboCup 2026 Incheon, Corea del Sur — 30 junio a 6 julio 2026

---

## Resumen de Verificación

| Métrica | Resultado |
|---------|----------|
| Hipótesis confirmadas | 19/23 (83%) |
| Parcialmente confirmadas | 3/23 (13%) |
| Refutadas | 1/23 (4%) — #12 colisión header/dato |
| Hallazgos nuevos | 3 (N1: Pin 0/RX1, N2: migración truncada, N3: código competencia ausente) |
| Total problemas activos | 25 |

---

## Prioridad 0 — Reconstrucción (ANTES de todo lo demás)

### P0.1 — Reconstruir código de competencia
- **Qué**: Determinar qué código REALMENTE CORRIÓ en el campeonato nacional dic 2025
- **Cómo**: Sesión con María Virginia Viollaz y Elías Cordero
- **Entregable**: `legacy/2025-season/competition-actual/`

### P0.2 — Migrar TODOS los archivos del repo 2025
- **Qué**: Copiar contenido completo (no stubs) de todos los archivos
- **Entregable**: `legacy/2025-season/code/` con archivos completos

---

## Prioridad 1 — Bugs que impiden funcionamiento (Febrero-Marzo)

### P1.1 — Rediseñar protocolo UART
- Bugs: #13, #14 (nota: #12 REFUTADO)
- Diseño: Start byte 0xFF, longitud, XOR checksum, timeout 100ms, resync

### P1.2 — Corregir zirconLib
- Bugs: #6 (BNO055), #21 (sin moveOmni), N1 (Pin 0/RX1)

### P1.3 — Corregir delantero
- Bugs: #18 (timeout), #19 (ángulo arco), #20 (avanzarAlFrente)

### P1.4 — Reescribir arquero desde cero
- Bugs: #15, #16 (no hay estructura salvable)

---

## Prioridad 2 — Mejoras que cambian el nivel (Marzo-Abril)

### P2.1 — Integrar sensores IR 360°
### P2.2 — Centralizar configuración
### P2.3 — Control heading BNO055
### P2.4 — Calibración exprés

---

## Prioridad 3 — Requisitos internacionales (Abril-Junio)

### P3.1 — Communication Module
### P3.2 — Verificar zona captura 1.5cm
### P3.3 — Handle y top marker
### P3.4 — Documentación de competencia

---

## Timeline

```
Feb 2026          Mar 2026          Abr 2026          May 2026          Jun 2026
│                 │                 │                 │                 │
├─ P0 ───────────┤                 │                 │                 │
│                 ├─ P1 ───────────┤                 │                 │
│                 │                 ├─ P2 ───────────┤                 │
│                 │                 │                 ├─ P3 ───────────┤
                                                                    30 Jun
                                                                 INCHEON
```

---

*Documento generado por Claude (Anthropic — Claude Opus 4.6) bajo supervisión de Gustavo Viollaz (@gviollaz), 21 de febrero de 2026.*