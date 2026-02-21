---
title: "Mejoras mecánicas para 2026"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
status: backlog
priority: alta
tags: [mecanica, hardware, diseño, dribbler, kicker]
---

# Mejoras mecánicas para 2026

## Qué investigar

- Evaluar estructura actual (peso, centro de gravedad, rigidez)
- **CRÍTICO**: Verificar zona de captura del dribbler — el límite cambió de 3.0cm a 1.5cm
- Evaluar sistema de dribbler (eficiencia, desgaste)
- Evaluar sistema de kicker (solenoide, potencia, test de potencia 2026)
- Analizar archivos STL del repo 2025 para identificar piezas a rediseñar
- Diseñar espacio para Communication Module (≥1cm del borde, protegido)
- Verificar handle con 5cm de clearance
- Top marker blanco ≥4cm integrado en el diseño

## Restricciones de las reglas 2026 (Soccer Vision)

- Diámetro: 18.0 cm (recomendado ≤17.7 cm)
- Altura: 18.0 cm (recomendado ≤17.7 cm, sin contar handle/marker)
- Peso: sin límite
- Zona captura: 1.5 cm máximo
- Sin colores naranja/amarillo/azul visibles
- Handle: 5cm clearance obligatorio

## Archivos 3D del 2025

Los archivos STL del diseño 2025 están en:
https://github.com/IITA-Proyectos/RoboCupJunior-Soccer-Open-League-2025/tree/main/Diseño3D

## Preguntas clave

1. ¿El dribbler actual cumple con la zona de captura de 1.5cm?
2. ¿El kicker pasa el test de potencia 2026? (kick de un arco, no debe volver al arco origen)
3. ¿Hay espacio para el Communication Module?
4. ¿El handle actual tiene 5cm de clearance?
5. ¿Qué piezas se pueden reusar vs rediseñar?
