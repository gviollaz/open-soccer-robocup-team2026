---
title: "Revisión completa del código 2025"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
status: completed
priority: alta
tags: [software, revision, legado, analisis]
---

# Revisión completa del código 2025

## Resumen ejecutivo

Se revisó todo el código del repositorio 2025. El software está en estado de prototipo funcional con varios bugs y código incompleto. La arquitectura básica (OpenMV → UART → Teensy → motores) es correcta. La máquina de estados del delantero es la pieza más completa.

## Inventario de código

### 1. Librería zirconLib (placa custom)

**Estado: Funcional, con limitaciones**

- Soporta 2 versiones de PCB: "Mark1" (dir+pwm) y "Naveen1" (dir1+dir2)
- Detección automática de versión via pin 32
- API de motores: `motor1/2/3(power, direction)` con límite de 100
- API de sensores: `readLine(1-3)`, `readBall(1-8)`, `readButton(1-2)`
- Compass BNO055: **código presente pero DESHABILITADO** (comentado)
- **Bug**: La función `isCompassCalibrated()` está fuera de scope (llave extra al final del .cpp)

### 2. Código del Arquero

**Estado: Incompleto / WIP**

- Tiene dos bloques `setup()`/`loop()` en el mismo archivo (el segundo sobreescribe al primero)
- Variable `potencia` usada pero **nunca definida** → no compila
- Lógica de oscilación en el arco usando giróscopo + sensores línea + ultrasonido
- Funciones `girar()` y `Adelante()` son no-bloqueantes (buen diseño)
- Sensor ultrasónico (TRIG/ECHO en pins 9/10) para medir distancia
- Thresholds de color hardcodeados para blanco/verde/negro por sensor
- Lógica de patrullaje: oscila izquierda-derecha detectando línea blanca
- **Problema grave**: Código mezclado con fragmentos experimentales, no es compilable

### 3. Código del Delantero (perseguir pelota)

**Estado: La pieza más completa del proyecto**

- Máquina de estados clara: `GIRANDO → AVANZANDO → CENTRANDO → PATEANDO`
- Decodifica protocolo UART de la cámara: [201][Xp][Yp][202][Xa][Ya]
- Calcula ángulo hacia pelota y arco usando `atan2()`
- Tolerancias configurables: `tolerancia_centrado=10`, `tolerancia_cercania=19`
- Giróscopo BNO055: código presente pero **comentado**
- **Bug en PATEANDO**: La condición `millis() - millis_inicio_estado <= 2000` parece invertida (debería ser `>=`)
- Estado CENTRANDO: Usa medio círculo para alinear pelota con arco (factor k=0.6)
- No hay manejo de sensores de línea (el robot puede salirse de la cancha)

### 4. Código OpenMV (visión)

**Estado: Funcional, con espacio para mejoras**

- `calcula-coordenadas-pelota.py`: Detección por color LAB + transformación homográfica
  - Threshold naranja: `(30, 60, 20, 60, 10, 50)` — hardcodeado
  - Matriz H hardcodeada — depende de la posición exacta de la cámara
  - Calcula coordenadas físicas y ángulo
- `calibrar-threshold.py`: Herramienta interactiva para calibrar colores
  - Usa UART para recibir comandos ('a' = muestra, 'b' = terminar)
  - Calcula threshold óptimo con margen de tolerancia 1.5x
  - **Muy útil para recalibrar en diferentes condiciones de luz**
- Scripts de envío de coordenadas (pelota, 1 arco, 2 arcos)
- Scripts para H7 y H7 Plus (diferencias de configuración)

### 5. Código de Control

- `giro-y-avance-zircon`: Control básico de movimiento
- `junta-control-y-movilidad`: Integración (la pieza más grande, ~6KB)
- `seguidor-de-linea`: Base para detección de línea blanca/verde
- `valores-sensores.xlsx`: Datos de calibración de sensores

## Protocolo de comunicación

```
OpenMV → UART (19200 baud) → Teensy

Paquete: 6 bytes
[201] [Xp] [Yp] [202] [Xa] [Ya]

- 201 = header pelota
- Xp  = distancia pelota (0-255, dividir por 2 para cm)
- Yp  = offset Y pelota (0=no detectada, else dividir por 2 y restar 50)
- 202 = header arco  
- Xa  = distancia arco
- Ya  = offset Y arco
```

## Bugs y problemas identificados

| # | Severidad | Descripción | Ubicación |
|---|-----------|-------------|----------|
| 1 | CRÍTICO | Variable `potencia` no definida en arquero | arquero-base.ino |
| 2 | CRÍTICO | Dos bloques setup()/loop() en el mismo archivo | arquero-base.ino |
| 3 | ALTO | Bug lógico en estado PATEANDO (`<=` debería ser `>=`) | perseguir-pelota.ino |
| 4 | ALTO | Sin detección de línea en delantero (se sale de cancha) | perseguir-pelota.ino |
| 5 | ALTO | Giróscopo BNO055 deshabilitado en todo el código | zirconLib + todos |
| 6 | MEDIO | Llave extra al final de zirconLib.cpp | zirconLib.cpp |
| 7 | MEDIO | Thresholds de visión hardcodeados | scripts OpenMV |
| 8 | BAJO | Nombres de archivos con espacios y sin extensión | Todo el repo 2025 |

## Conclusiones

1. **El delantero es la base más sólida** para iterar. La máquina de estados funciona conceptualmente.
2. **El arquero necesita reescritura** — el código actual no compila.
3. **La visión funciona** pero depende de calibración manual y thresholds fijos.
4. **El giróscopo nunca se integró exitosamente** — está comentado en todas partes.
5. **La placa Zircon es funcional** pero necesita evaluación física.
6. **Falta comunicación entre robots** (arquero-delantero no se coordinan).

## Recomendaciones para 2026

1. **Prioridad 1**: Limpiar y hacer compilar el código del delantero. Corregir el bug de PATEANDO. Agregar detección de línea.
2. **Prioridad 2**: Reescribir el código del arquero desde cero, usando la estructura del delantero como plantilla.
3. **Prioridad 3**: Integrar el giróscopo BNO055 para mantener orientación.
4. **Prioridad 4**: Mejorar la visión (auto-calibración, robustez ante cambios de luz).
5. **Prioridad 5**: Evaluar comunicación entre robots para estrategia coordinada.
6. **Adaptar a pelota nueva de 42mm** — puede afectar detección por visión y por IR.
