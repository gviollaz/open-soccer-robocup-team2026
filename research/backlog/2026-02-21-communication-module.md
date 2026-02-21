---
title: "Integración del Communication Module"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
status: backlog
priority: alta
tags: [electronica, comunicacion, competencia, obligatorio]
---

# Integración del Communication Module

## Contexto

Desde 2025 (Brasil), el Soccer League Committee provee un módulo de comunicación a cada equipo. Es **obligatorio** en la competencia internacional.

## Qué investigar

- Leer documentación completa: https://github.com/robocup-junior/soccer-communication-module
- Entender el protocolo GPIO (pin 2.54mm para start/stop)
- Definir cómo alimentar el módulo (directo a batería 5.3V-25V, 500mA)
- Determinar ubicación física en el robot (≥1cm del borde, protegido de impactos)
- Implementar lógica de start/stop en el código del Teensy
- Verificar compatibilidad con el diseño actual de la placa Zircon

## Requisitos técnicos ya conocidos

- Alimentación: GND y BAT+ directo a batería sin switch intermedio
- Rango de voltaje: 5.3V - 25V (alternativa: pin 3V3)
- Corriente: debe poder suministrar 500mA en todo momento
- Siempre encendido durante el partido (incluso con robot fuera del campo)
- No cuenta para peso ni altura máxima
- Interfaz actual: 1 pin GPIO (start/stop)
- Futuro: UART para funciones más complejas

## Impacto en el diseño

- La placa Zircon necesita un conector para el módulo
- El código necesita escuchar el pin GPIO para iniciar/detener el robot
- Hay que reservar espacio físico en el chásis
- La batería debe poder suministrar la corriente adicional
