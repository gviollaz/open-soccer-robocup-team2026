---
lang: es
translation-of: "./AI-INSTRUCTIONS.md"
---

# Instrucciones para asistentes de IA

> Este archivo está dirigido a cualquier modelo de lenguaje (LLM) o asistente de IA que trabaje con este repositorio.

## Contexto del proyecto

Este repositorio pertenece a un equipo de estudiantes del IITA (Instituto de Informática y Tecnología Aplicada) en Salta, Argentina, que participa en la competencia **RoboCupJunior Soccer Vision League 2026** (anteriormente Soccer Open League).

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

### 3. Idioma y traducciones

- **El inglés es el idioma canónico** para toda la documentación
- Las traducciones al español son bienvenidas como archivos `.es.md` (ver `TRANSLATIONS.md`)
- Los nombres de archivos siempre en inglés (para archivos nuevos)
- Comentarios de código en inglés; aclaraciones breves en español entre paréntesis son aceptables
- Incluir `lang: en` o `lang: es` en el frontmatter

### 4. Journal de ingeniería

Si documentás trabajo o análisis, crear una entrada en `journal/` con el formato:
```
journal/YYYY-MM-DD-descripcion.md
```

### 5. Investigación

- Temas nuevos a investigar → `research/backlog/`
- Análisis en curso → `research/in-progress/`
- Análisis terminados → `research/completed/`
- Referencias externas → `research/references/`

### 6. Testing

- Protocolos de prueba → `testing/protocols/`
- Resultados → `testing/results/YYYY-MM-DD-descripcion.md`

### 7. Código

- **No modificar** archivos en `legacy/` (son referencia histórica)
- Código nuevo va en `software/` en la subcarpeta correspondiente
- Siempre comentar el código
- Indicar si el código fue probado en hardware real

### 8. Hardware

- Esquemáticos y PCB → `hardware/electronics/`
- Baterías, motores, potencia → `hardware/electrical/`
- Diseños 3D y ensamblaje → `hardware/mechanical/`

## Archivos clave a leer primero

1. `README.md` — Visión general del proyecto
2. `CONTRIBUTING.md` — Reglas de contribución y atribución
3. `TRANSLATIONS.md` — Política de idioma y traducciones
4. `competition/timeline.md` — Cronograma y deadlines
5. `legacy/2025-season/README.md` — Qué se hizo el año pasado

## Convenciones de tags

Usar estos tags en el frontmatter de archivos `.md`:

**Área**: `vision`, `mobility`, `control`, `dribbler`, `communication`, `electronics`, `mechanical`, `battery`, `sensors`
**Tipo**: `analysis`, `tutorial`, `protocol`, `result`, `decision`, `comparison`
**Robot**: `goalie`, `striker`, `both`
**Prioridad**: `high`, `medium`, `low`
