# Instrucciones para asistentes de IA

> Este archivo está dirigido a cualquier modelo de lenguaje (LLM) o asistente de IA que trabaje con este repositorio.

## Contexto del proyecto

Este repositorio pertenece a un equipo de estudiantes del IITA (Instituto de Informática y Tecnología Aplicada) en Salta, Argentina, que participa en la competencia **RoboCupJunior Soccer Open League 2026**.

El equipo construye y programa **2 robots autónomos** (un arquero y un delantero) que juegan fútbol en una cancha regulada. Los robots usan:

- **Microcontroladores**: Teensy / Arduino
- **Visión**: Cámaras OpenMV (H7 / H7 Plus) con MicroPython
- **Comunicación**: UART entre OpenMV y Teensy
- **Sensores**: Línea (infrarrojos), giróscopo/IMU, ultrasonido
- **Actuadores**: Motores TT con drivers H-bridge, servos, dribbler, solenoide
- **Estructura**: Impresión 3D (Tinkercad/OpenSCAD) + construcción manual
- **Placa custom**: PCB "Zircon" con librería propia (zirconLib)

## Reglas que debés seguir

### 1. Atribución

Todo cambio que hagas **debe** incluir en el mensaje de commit:
```
Author: [Tu nombre de IA] ([Proveedor])
Requested-by: [Nombre del humano que te lo pidió]
```

### 2. Estructura

- Respetar la estructura de carpetas existente
- Archivos nuevos: convención `kebab-case`
- Archivos con fecha: `YYYY-MM-DD-descripcion.md`
- Incluir frontmatter YAML en todo archivo `.md`

### 3. Journal de ingeniería

Si documentás trabajo o análisis, crear una entrada en `journal/` con el formato:
```
journal/YYYY-MM-DD-descripcion.md
```

### 4. Investigación

- Temas nuevos a investigar → `research/backlog/`
- Análisis en curso → `research/in-progress/`
- Análisis terminados → `research/completed/`
- Referencias externas → `research/references/`

### 5. Testing

- Protocolos de prueba → `testing/protocols/`
- Resultados → `testing/results/YYYY-MM-DD-descripcion.md`

### 6. Código

- **No modificar** archivos en `legacy/` (son referencia histórica)
- Código nuevo va en `software/` en la subcarpeta correspondiente
- Siempre comentar el código
- Indicar si el código fue probado en hardware real

### 7. Hardware

- Esquemáticos y PCB → `hardware/electronics/`
- Baterías, motores, potencia → `hardware/electrical/`
- Diseños 3D y ensamblaje → `hardware/mechanical/`

## Archivos clave a leer primero

1. `README.md` — Visión general del proyecto
2. `CONTRIBUTING.md` — Reglas de contribución y atribución
3. `competition/timeline.md` — Cronograma y deadlines
4. `legacy/2025-season/README.md` — Qué se hizo el año pasado

## Convenciones de tags

Usar estos tags en el frontmatter de archivos `.md`:

**Área**: `vision`, `movilidad`, `control`, `dribbler`, `comunicacion`, `electronica`, `mecanica`, `bateria`, `sensores`  
**Tipo**: `analisis`, `tutorial`, `protocolo`, `resultado`, `decision`, `comparacion`  
**Robot**: `arquero`, `delantero`, `ambos`  
**Prioridad**: `alta`, `media`, `baja`
