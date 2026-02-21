---
title: "Arquitectura del sistema de robots — Temporada 2025"
date: 2026-02-21
updated: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
ai-assisted: false
ai-tool: "Claude (Anthropic - Claude Opus 4.6)"
status: final-verificado
tags: [arquitectura, hardware, software, openmv, teensy, protocolo, sensores, analisis]
nota: "Actualizado post-verificación cruzada contra código fuente real. Hipótesis #12 REFUTADA. Agregados hallazgos N1, N2, N3."
---

# Arquitectura del Sistema de Robots — Temporada 2025

## Análisis Técnico Completo para Planificación 2026

**Equipo**: IITA RoboCup Junior Soccer Open (ahora Soccer Vision)
**Autor del análisis**: Claude (Anthropic — Claude Opus 4.6)
**Supervisión**: Gustavo Viollaz (@gviollaz)
**Fecha**: 21 de febrero de 2026
**Última actualización**: 21 de febrero de 2026 — post-verificación cruzada contra código fuente
**Fuentes**: Repositorios `IITA-Proyectos/RoboCupJunior-Soccer-Open-League-2025` y `IITA-Proyectos/open-soccer-robocup-team2026`

> ⚠️ **Estado de verificación**: Este documento fue verificado línea por línea contra el código fuente real de ambos repositorios (2025 y 2026). Se corrigió la hipótesis #12 (REFUTADA) y se agregaron 3 hallazgos nuevos (N1, N2, N3). Ver [análisis cruzado completo](2026-02-21-analisis-cruzado-verificacion-hipotesis.md) para metodología y detalles.

---

## 1. Resumen Ejecutivo

El sistema 2025 consiste en dos robots (arquero y delantero) construidos sobre la placa controladora Zircon (diseño propio en dos variantes: Mark1 y Naveen1) con microcontrolador Teensy 4.1, cámara OpenMV H7 como sensor de visión primario, giroscopio BNO055 para orientación, sensores de línea reflectivos para detección de límites, sensores IR para detección de pelota, y un sensor ultrasónico HC-SR04 para distancia en el arquero.

El equipo ganó el campeonato nacional en diciembre 2025 con este sistema. Sin embargo, el análisis revela **problemas estructurales significativos** en software que deben resolverse antes de competir internacionalmente en Incheon (junio-julio 2026).

### Hallazgos

Se identificaron **23 puntos de falla originales** + **3 hallazgos nuevos de verificación** categorizados en: bugs de software (8), deficiencias de diseño (7), vulnerabilidades de protocolo (5), riesgos por cambios de reglas 2026 (3), y hallazgos de verificación cruzada (3).

De los 23 originales: **19 confirmados**, **3 parcialmente confirmados**, **1 refutado** (#12).

---

## 2. Diagrama de Arquitectura del Sistema

```
┌─────────────────────────────────────────────────────────────────┐
│                        ROBOT (Arquero o Delantero)              │
│                                                                 │
│  ┌──────────────┐     UART (Serial1)      ┌──────────────────┐ │
│  │  OpenMV H7   │ ───────────────────────► │   Teensy 4.1     │ │
│  │  (Cámara)    │   19200 baud            │   (Controlador)  │ │
│  │              │   Protocolo propietario  │                  │ │
│  │  - RGB565    │   [Header][Data][Data]   │  ┌────────────┐ │ │
│  │  - QVGA      │                          │  │ zirconLib  │ │ │
│  │  - find_blobs│                          │  │ (HAL)      │ │ │
│  │  - Homografía│                          │  └─────┬──────┘ │ │
│  └──────────────┘                          │        │        │ │
│                                            │        ▼        │ │
│                              ┌─────────────┤  ┌──────────┐  │ │
│  ┌──────────────┐            │             │  │ Motores  │  │ │
│  │  BNO055      │◄───── I2C ┤             │  │ x3 (120°)│  │ │
│  │  (Giroscopio)│            │             │  └──────────┘  │ │
│  └──────────────┘            │             │                │ │
│                              │  Zircon PCB │  ┌──────────┐  │ │
│  ┌──────────────┐            │  (Mark1 o   │  │ Sensores │  │ │
│  │  HC-SR04     │◄── GPIO ──┤   Naveen1)  │  │ IR x8    │  │ │
│  │  (Ultrasón.) │            │             │  └──────────┘  │ │
│  └──────────────┘            │             │                │ │
│                              │             │  ┌──────────┐  │ │
│  ┌──────────────┐            │             │  │ Línea x3 │  │ │
│  │  Dribbler    │◄── PWM ───┤             │  │ (Analog) │  │ │
│  │  (Motor DC)  │            │             │  └──────────┘  │ │
│  └──────────────┘            └─────────────┤                │ │
│                                            │  ┌──────────┐  │ │
│                                            │  │Botones x2│  │ │
│                                            │  └──────────┘  │ │
│                                            └────────────────┘ │
│                                                                 │
│  ⚠️ 2026: Falta Communication Module (obligatorio internacional)│
└─────────────────────────────────────────────────────────────────┘
```

---

## 3. Capa de Hardware

### 3.1 Microcontrolador: Teensy 4.1

- **MCU**: NXP i.MX RT1062, ARM Cortex-M7 a 600 MHz
- **RAM**: 1024 KB, Flash: 8 MB
- **Puertos seriales**: 8 UART hardware (se usa Serial1 para OpenMV)
- **ADC**: Múltiples canales analógicos (se usan para sensores IR y de línea)
- **PWM**: Múltiples canales (control de motores y dribbler)
- **I2C**: Bus para giroscopio BNO055

**Observación**: El Teensy 4.1 tiene capacidad de sobra para lo que se le exige. El cuello de botella no es el hardware de procesamiento sino el software.

### 3.2 Placa Controladora: Zircon PCB

La placa Zircon es un PCB diseñado para RoboCup Junior que actúa como shield/carrier para el Teensy. Existe en **dos variantes** con diferente pinout:

| Componente | Mark1 | Naveen1 |
|------------|-------|---------|
| Motor 1 | INA=2, INB=5, PWM=3 | DIR1=3, DIR2=4 (sin PWM separado) |
| Motor 2 | INA=8, INB=7, PWM=6 | DIR1=6, DIR2=7 |
| Motor 3 | INA=11, INB=12, PWM=4 | DIR1=5, DIR2=2 |
| Ball IR 1-4 | 14, 15, 16, 17 | 20, 21, 14, 15 |
| Ball IR 5-8 | 20, 21, 22, 23 | 16, 17, 18, 19 |
| Botones | 9, 10 | 8, 10 |
| Línea 1-3 | A11, A13, A12 | A8, A9, A12 |
| Detección versión | Pin 32 LOW = Mark1 | Pin 32 HIGH = Naveen1 |

**Diferencia clave en control de motores**:
- **Mark1**: Usa esquema clásico DIR + DIR + PWM (3 pines por motor, 9 total). La dirección se fija con `digitalWrite` y la velocidad con `analogWrite` en el pin PWM.
- **Naveen1**: Usa esquema H-Bridge directo con 2 pines PWM por motor (6 pines total). La velocidad y dirección se controlan ambas con `analogWrite` en los pines de dirección.

**⚠️ Punto de falla #1** *(parcialmente confirmado)*: La detección de versión usa `INPUT_PULLDOWN` (no pin puramente flotante). El pulldown interno de ~100kΩ del Teensy 4.1 mitiga significativamente el riesgo de ruido. Es razonablemente confiable si el pin está conectado a VCC (Naveen1) o abierto (Mark1). El riesgo existe pero es menor al indicado originalmente.

**Sugerencia**: Agregar un `Serial.println(getZirconVersion())` obligatorio en el `setup()` de cada programa, y verificar visualmente antes de cada partido.

### 3.3 Configuración de Motores (Omnidireccional 3 ruedas)

El robot usa 3 motores DC con ruedas omnidireccionales a 120° entre sí:

```
         Motor 1 (trasero)
            ↕
    Motor 3 ←→ Motor 2
     (izq)      (der)
```

La librería expone `motor1(power, direction)`, `motor2(power, direction)`, `motor3(power, direction)` con potencia 0-100 y dirección 0/1.

**⚠️ Punto de falla #2**: No existe una función de movimiento omnidireccional unificada `move(angle, speed)`. Cada programa reimplementa las combinaciones de motores de manera diferente e inconsistente. Esto es una fuente primaria de bugs.

### 3.4 Sensores

#### 3.4.1 Sensores de Línea (x3 — Reflectivos analógicos)

- **Cantidad**: 3 (izquierda, centro, derecha — posición inferior del robot)
- **Tipo**: Sensores reflectivos analógicos (probablemente QRE1113 o similar)
- **Lectura**: `readLine(n)` → `analogRead(linepin_n)` → Valor 0-1023
- **Calibración del arquero**:

| Color | S1 (Izq) | S2 (Centro) | S3 (Der) |
|-------|-----------|-------------|----------|
| Blanco | 575-753 | 494-762 | 590-754 |
| Verde | 210-340 | 370-467 | 254-342 |
| Negro | 174-227 | 418-422 | 234-269 |

**⚠️ Punto de falla #3**: Los rangos están hardcodeados con valores exactos calibrados para UNA cancha. En competencia internacional las condiciones de iluminación serán completamente diferentes.

**⚠️ Punto de falla #4**: Solo 3 sensores de línea cubren un ángulo muy limitado.

#### 3.4.2 Sensores de Pelota IR (x8 — Fotodiodos infrarrojos)

- **Cantidad**: 8 fotodiodos distribuidos en anillo alrededor del robot
- **Tipo**: Fotodiodos IR que detectan la señal pulsada de la pelota (40 kHz modulada)
- **Lectura**: `readBall(n)` → `1024 - analogRead(ballpin_n)` → Valor invertido (mayor = más cerca)
- **Distribución**: 8 sensores a 45° cada uno para cobertura de 360°

**⚠️ Punto de falla #5**: En el código actual del delantero, los sensores IR **no se usan**. Los 8 sensores IR están instalados pero **desperdiciados** en el programa de competencia.

#### 3.4.3 Giroscopio: Adafruit BNO055

- **Chip**: Bosch BNO055 — IMU de 9 ejes con fusión sensorial integrada
- **Interfaz**: I2C (dirección 0x28)
- **Dato utilizado**: `orientation.x` (Euler heading, 0-360°)

**⚠️ Punto de falla #6** *(confirmado)*: La variable `compassCalibrated` se inicializa como `false` y **nunca se establece como `true`**. La función `readCompass()` devuelve siempre 0.

**Hallazgo adicional de verificación**: El BNO055 fue **DELIBERADAMENTE COMENTADO** antes de la competencia. El robot ganó el nacional SIN control de orientación.

#### 3.4.4 Sensor Ultrasónico: HC-SR04 (solo arquero)

**⚠️ Punto de falla #7**: `pulseIn(ECHO, HIGH)` es **bloqueante** — detiene todo el programa hasta 1 segundo si no hay eco.

---

## 4. Capa de Software — OpenMV (Visión)

### 4.1 Configuración de Cámara

```python
sensor.reset()
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)
sensor.skip_frames(time=2000)
sensor.set_auto_whitebal(False)
```

### 4.2 Detección de Objetos por Color (Blob Detection)

**⚠️ Punto de falla #8-9** *(confirmado)*: Thresholds con L_min > L_max rompen blob detection.

### 4.3 Transformación Homográfica

**⚠️ Punto de falla #10**: Dos matrices de homografía diferentes (h=10cm vs h=18.7cm).

---

## 5. Protocolo de Comunicación OpenMV → Teensy

### 5.1 Capa Física

| Parámetro | Valor |
|-----------|-------|
| Interfaz | UART (Serial asíncrono) |
| OpenMV TX | UART3 (OpenMV) → Pin 0 RX1 (Teensy) |
| Baud rate | 19200 bps (versión final) |
| Formato | 8N1 |

**⚠️ Punto de falla #11** *(parcialmente confirmado)*: Par funcional principal sincronizado a 19200, pero archivos alternativos con baud diferente.

### 5.2 Estructura de Paquetes

Tres versiones incompatibles (V1, V2, V3).

### 5.3 Codificación de Datos

Datos: rango 0-200. Headers: 201-204.

### 5.4 Análisis de Separación Header/Dato

> **⚠️ CORRECCIÓN — Punto de falla #12 REFUTADO** (actualización post-verificación, 21 feb 2026)
>
> El análisis original afirmaba que los headers (201-204) podían colisionar con valores de datos legítimos. **Esto es INCORRECTO**. La verificación del código fuente real demuestra que el protocolo fue diseñado con separación intencional:
>
> ```python
> byteXp = min(max(int(Xp * 2), 0), 200)   # Rango: 0–200
> byteYp = min(max(int((Yp + 50) * 2), 0), 200)  # Rango: 0–200
> # Headers: 201, 202, 203 → FUERA del rango de datos
> ```
>
> **Problemas reales del protocolo que SÍ persisten:**
> - #13: Sin checksum ni CRC
> - #14: Lectura de bloque fijo sin resync
> - Sin mecanismo de resincronización
> - Sin timeout de protocolo

---

## 6. Capa de Software — Teensy (Control)

### 6.1 Programa del Arquero

**⚠️ Punto de falla #15** *(confirmado — peor de lo esperado)*: Múltiples errores que impiden compilación.

**⚠️ Punto de falla #16** *(confirmado — peor de lo documentado)*: Sensores leen pines NO CONFIGURADOS a nivel global.

### 6.2 Programa del Delantero

**⚠️ Punto de falla #18** *(confirmado)*: Timeout de pateo invertido.
**⚠️ Punto de falla #19** *(confirmado)*: Ángulo arco usa coordenadas de pelota.
**⚠️ Punto de falla #20**: avanzarAlFrente() diagonal.

### 6.3 Librería zirconLib

**⚠️ Punto de falla #21**: Sin funciones de movimiento de alto nivel.

### 6.4 Código del Dribbler

**⚠️ Punto de falla #22** *(confirmado)*: Dribbler NUNCA se activó automáticamente.
**⚠️ Punto de falla #23 — REGLAS 2026**: Zona de captura 3.0→1.5cm.

---

## 7. Hallazgos Nuevos de Verificación Cruzada

### 🆕 N1 — Conflicto Pin 0/RX1 en modo Naveen1 (SEVERIDAD: ALTA)

En `zirconLib.cpp`, `motor1pwm`/`motor2pwm`/`motor3pwm` como `int` globales (default 0). En Naveen1, nunca se asignan.
`initializePins()` ejecuta `pinMode(0, OUTPUT)` — Pin 0 es RX1 (Serial1 receive).
Funciona por accidente porque `Serial1.begin()` se llama después y reconfigura el pin.

### 🆕 N2 — Código migrado truncado (SEVERIDAD: MEDIA)

Múltiples archivos críticos del repo 2025 están ausentes o son stubs en el repo 2026.

### 🆕 N3 — Código de competencia no está en el repositorio (SEVERIDAD: ALTA)

Evidencia convergente de que el código que corrió en el nacional difiere del repo.

---

## 8. Resumen de Puntos de Falla (Actualizado post-verificación)

### Críticos

| # | Estado | Componente | Problema |
|---|--------|-----------|----------|
| 6 | ✅ Confirmado | zirconLib | `compassCalibrated` siempre false |
| 8-9 | ✅ Confirmado | OpenMV | Thresholds con L_min > L_max |
| ~~12~~ | ❌ **REFUTADO** | ~~UART~~ | ~~Colisión header/dato~~ → Separación correcta por diseño |
| 13-14 | ✅ Confirmado | UART | Sin checksum, sin resync |
| 15-16 | ✅ Confirmado | Arquero | No compila, sensores leen basura |
| 18 | ✅ Confirmado | Delantero | Timeout pateo invertido |
| 19 | ✅ Confirmado | Delantero | Ángulo arco usa datos pelota |
| N1 | 🆕 Nuevo | zirconLib | Pin 0/RX1 como OUTPUT en Naveen1 |
| N3 | 🆕 Nuevo | General | Código competencia ausente |

### Altos

| # | Estado | Componente | Problema |
|---|--------|-----------|----------|
| 2 | ✅ | Motores | Sin moveOmni() |
| 3 | ✅ | Línea | Thresholds hardcodeados |
| 5 | ✅ | IR | 8 sensores no usados |
| 7 | ✅ | Ultrasónico | pulseIn() bloqueante |
| 11 | ⚠️ Parcial | UART | Baud rates diferentes en archivos alternativos |
| 22 | ✅ | Dribbler | String serial que nadie envía |
| N2 | 🆕 | Migración | Archivos críticos ausentes |

### Moderados

| # | Estado | Componente | Problema |
|---|--------|-----------|----------|
| 1 | ⚠️ Parcial | Zircon | Detección versión (mitigado con pulldown) |
| 4 | ✅ | Línea | Solo 3 sensores |
| 17 | ✅ | Arquero | Adelante() con static |
| 20 | ✅ | Delantero | avanzarAlFrente() diagonal |
| 21 | ✅ | zirconLib | Sin funciones alto nivel |
| 23 | ✅ | Dribbler | Zona captura 3→1.5cm |

---

## 9. Métricas de Fiabilidad

| Métrica | Resultado |
|---------|----------|
| Confirmadas | 19/23 (83%) |
| Parciales | 3/23 (13%) |
| Refutadas | 1/23 (4%) |
| Nuevas | 3 |
| Total activos | 25 |

---

## 10. Recomendaciones para 2026

Ver **[Mapa de Prioridades Revisado](2026-02-21-mapa-prioridades-revisado.md)**.

### Arquitectura de Software Propuesta para 2026

```
OpenMV: config.py + detect_ball() + detect_goals() + send_packet()
Teensy: config.h + zirconLib2.h + protocol.h + sensors.h + strategy_goalie.h + strategy_striker.h + comm_module.h + main.cpp
```

---

## 11. Inventario de Sensores

| Sensor | Cant. | Interfaz | Usado en 2025 |
|--------|-------|----------|---------------|
| OpenMV H7 | 1 | UART | ✅ Delantero |
| BNO055 | 1 | I2C | ⚠️ Comentado |
| HC-SR04 | 1 | GPIO | ✅ Arquero |
| IR Ball | 8 | ADC | ❌ No usados |
| Line | 3 | ADC | ✅ Arquero |
| Botones | 2 | GPIO | Parcial |
| Dribbler | 1 | PWM | ❌ Sin activación auto |

---

*Documento generado por Claude (Anthropic — Claude Opus 4.6) bajo supervisión de Gustavo Viollaz (@gviollaz), 21 de febrero de 2026.*
*Actualizado post-verificación cruzada contra código fuente real, 21 de febrero de 2026.*