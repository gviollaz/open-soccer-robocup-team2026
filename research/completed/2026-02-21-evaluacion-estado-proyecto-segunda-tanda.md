---
title: "Evaluación de estado del proyecto — segunda tanda (post-verificación y nuevo código en legacy)"
date: 2026-02-21
author: "ChatGPT-5.2 Pro (OpenAI)"
requested-by: "Gustavo Viollaz (@gviollaz)"
ai-assisted: true
ai-tool: "ChatGPT / GPT-5.2 Pro (OpenAI)"
status: final
tags: [evaluacion, estado-proyecto, robocup2026, verificacion, software, hardware, legacy-2025, planificacion]
---

# Evaluación de estado del proyecto (segunda tanda)

**Este documento fue generado por _ChatGPT (modelo GPT-5.2 Pro, OpenAI)_ bajo la supervisión de _Gustavo Viollaz_.**  
Fecha: **2026-02-21**.

## 0. Qué cambió desde la evaluación anterior (por qué esta "segunda tanda")

En esta ronda ya no estamos evaluando solo estructura y documentos: ahora hay **más código fuente** dentro del repo 2026, y además existe una **verificación cruzada** (Claude vs ChatGPT) contra el código real.

Nuevos insumos (dentro del repo):

- `research/completed/2026-02-21-arquitectura-sistema-2025.md` (Claude) — análisis completo con 23 puntos de falla.
- `research/completed/2026-02-21-analisis-cruzado-verificacion-hipotesis.md` (Claude) — verificación directa contra código fuente.
- `research/completed/2026-02-21-mapa-prioridades-revisado.md` (Claude) — roadmap P0–P3 post-verificación.
- Código migrado a `legacy/2025-season/code/` (parcial):
  - `delantero/perseguir-pelota.ino` (con máquina de estados)
  - `arquero/arquero-base.ino` (parcial/incompleto)
  - `libraries/zirconLib/*` (sí está el código)
  - `vision-openmv/calcula-coordenadas-pelota.py` (sí está el código)
  - *stubs* (solo encabezados / link al repo 2025) en otros archivos.

Conclusión rápida: **el repo creció y ahora se puede medir con evidencia** qué era real, qué era hipótesis, y qué partes faltan.

---

## 1. Estado real del repo hoy (visión honesta, sin maquillaje)

### 1.1 Fortalezas actuales

- **Estructura limpia** y con intención 2026: `software/`, `hardware/`, `testing/`, `competition/`, `research/`, `journal/`, `legacy/`.
- Existe una "capa meta" muy buena: *AI-INSTRUCTIONS* + investigación versionada con frontmatter y trazabilidad.
- Ya hay un "forense" fuerte del sistema 2025 y un **roadmap P0–P3** que es coherente con lo observado en el código.

### 1.2 Debilidades/alertas actuales

- **La base de código "que realmente corrió en nacional 2025" NO está identificada aún.**
  - Esto no es un detalle: hay bugs en los archivos migrados que harían imposible que el robot haya jugado como campeón si esos archivos fueran el binario real.
- La carpeta `legacy/` está **a medio camino**: hay piezas completas (zirconLib) pero también hay muchos archivos que son solo "títulos" que apuntan al repo 2025.
- En `software/` (2026) todavía no se ve un "núcleo ejecutable" (goalie/striker 2026) con tests, logging y protocolo robusto.

---

## 2. Evidencias que CONFIRMAN puntos críticos (y por qué importan)

Esta sección usa evidencia directa del código migrado + la verificación cruzada.

### 2.1 UART OpenMV → Teensy: protocolo mínimo y frágil (confirmado)

El delantero (`perseguir-pelota.ino`) decodifica datos asumiendo exactamente 6 bytes:

- `[201][Xp][Yp][202][Xa][Ya]`
- sin checksum
- sin re-sincronización robusta (si se pierde 1 byte, quedás "corridos")

Esto confirma el riesgo que ya se venía señalando: es un canal "sensible al ruido" y difícil de depurar.

### 2.2 Bug CRÍTICO en "pateo" del delantero (confirmado)

En el estado `PATEANDO_adelante`, la condición temporal está invertida (comparación al revés).  
En la práctica, esto haría que el "pateo" se apague inmediatamente en el primer ciclo.

Esto es una evidencia fuerte de que **el archivo migrado no es el que ganó** (o fue editado después), porque un striker que "nunca patea" no encaja con un campeón (salvo estrategias muy raras / compensaciones externas).

### 2.3 Bug CRÍTICO: ángulo del arco calculado con coordenadas de pelota (confirmado)

El cálculo de `anguloArco` usa `(decodedYp, decodedXp)` en vez de `(decodedYa, decodedXa)`.  
Eso vuelve la variable `anguloArco` básicamente inútil.

### 2.4 BNO055 en zirconLib está "presente pero inutilizado" (confirmado)

`zirconLib` expone `readCompass()` pero depende de una variable `compassCalibrated` que inicia en `false` y no se activa en ningún lado.

Resultado: por esa API, el heading devuelve 0 o "no sirve".  
Esto confirma por qué la orientación probablemente quedó deshabilitada en competencia (o fue usada por hacks "por fuera" de zirconLib).

### 2.5 `pulseIn()` en arquero: bloqueo duro (confirmado)

El arquero usa `pulseIn(ECHO, HIGH)` para ultrasonido.  
Eso puede bloquear hasta ~1s y es incompatible con control reactivo.

### 2.6 Visión: thresholds inconsistentes y (en algunos archivos) inválidos (confirmado)

Hay evidencia de thresholds LAB invertidos (L_min > L_max) en ciertos archivos de visión.  
Eso explica fallas "místicas" de detección: el mismo robot puede pasar de ver todo a no ver nada solo por un archivo errado.

---

## 3. Evidencias que REFUTAN / AJUSTAN puntos (correcciones intelectuales sanas)

### 3.1 Refutado: "colisión header/dato" en el UART (refutado)

Durante la verificación cruzada se refutó la hipótesis de colisión de headers 201+ con datos:
- El diseño intencional separa **datos en 0–200** y **headers en 201+**.

Esto es una buena noticia: el protocolo *al menos* intentaba evitar colisiones por rango.  
Sigue siendo frágil por otros motivos (checksum/resync), pero este punto en particular queda descartado.

---

## 4. Hallazgos NUEVOS habilitados por el nuevo código (cosas que antes no se veían)

### 4.1 Riesgo de pin conflict en Naveen1: posible choque con Serial1 (nuevo)

En modo Naveen1, `zirconLib` no asigna pines `motorXpwm`, pero igualmente hace `pinMode(motorXpwm, OUTPUT)`.  
Si esos enteros quedan en 0, el pin 0 (RX1) puede ser reconfigurado como salida y romper la UART hacia el OpenMV.

Esto es un hallazgo enorme porque:
- explicaría comportamientos intermitentes "UART rara"
- explicaría por qué una versión funciona y otra no al cambiar de PCB

### 4.2 Migración truncada: falta código clave (nuevo)

Se detectan archivos migrados como *stubs* (solo encabezado y link al repo 2025) y herramientas críticas (p.ej. calibrador) no están completas.

Implicancia:
- No se puede reproducir ni depurar 2025 "desde el repo 2026" todavía.
- Hay riesgo de "proyecto de documentación impecable + sistema real no reproducible".

### 4.3 El repo no refleja todavía el "código de competencia real" (nuevo)

Esto aparece como un requisito P0: identificar exactamente qué versiones se compilaron y subieron a los robots en diciembre 2025, porque lo que está en `legacy/2025-season/code/` hoy no encaja con un sistema campeón (por bugs críticos).

---

## 5. Propuestas de solución (con pros/cons, no solo "deberíamos hacer X")

A continuación, propuestas concretas alineadas con el roadmap P0–P3, y donde tiene sentido incluyo alternativas y trade-offs.

### 5.1 P0 — Reconstrucción primero (sí, antes de "mejorar")

**Propuesta P0.1:** crear `legacy/2025-season/competition-actual/` con:
- `.ino` y `.py` EXACTOS que corrieron (las versiones subidas a los robots)
- notas mínimas: "qué robot, qué fecha, qué hardware (Mark1/Naveen1), qué OpenMV"

**Pros**
- Te da una base real para medir regresiones y mejoras.
- Evita perseguir fantasmas (código que "creemos que corría" pero no).

**Contras**
- Requiere una sesión humana (recolectar archivos de PCs/pendrives/firmwares).
- Puede descubrir "deuda técnica incómoda" (que el campeón fue un parche heroico).

**Propuesta P0.2:** migración completa del repo 2025 (sin stubs) a `legacy/2025-season/code/`  
**Pros:** reproducibilidad, auditoría, base para refactor.  
**Contras:** riesgo de "volcar basura" sin curaduría; hay que acompañarlo con índice y clasificación.

---

### 5.2 UART OpenMV ↔ Teensy: tres opciones, elegí según apetito de riesgo

**Opción A — "Upgrade mínimo" (bajo costo, mediana robustez)**  
Mantener estructura fija pero agregar:
- byte de inicio (SOF) único (ej: 0xFF)
- checksum simple (XOR) al final
- re-sincronización: buscar SOF en un ring buffer

**Pros**
- Cambios chicos en OpenMV y Teensy.
- Debug simple.

**Contras**
- XOR no detecta todos los errores.
- Longitud fija limita extensiones futuras.

**Opción B — "Frame serio" (costo medio, alta robustez)**  
Paquete: `[SOF][LEN][TYPE][PAYLOAD...][CRC8/CRC16]`  
+ parser con ring buffer + timeout (ej 100 ms) + contador de secuencia.

**Pros**
- Resiste pérdida de bytes y ruido.
- Escalable: podés sumar objetos, flags, timestamps.

**Contras**
- Más trabajo y más superficie de bugs al principio.
- Requiere disciplina de testing.

**Opción C — SLIP/COBS (costo medio/alto, robustez muy alta)**  
Codificación para garantizar delimitación.
**Pros:** excelente para streams ruidosos; resync casi automático.  
**Contras:** más complejidad, overhead, y debugging menos directo.

Mi recomendación 2026: **Opción B** (Frame serio) porque el costo es pagable y el beneficio (debug + estabilidad) es masivo.

---

### 5.3 zirconLib2: arreglar HAL + agregar cinemática (P1.2)

**Propuesta:** crear `zirconLib2` (como sugiere el roadmap), con 3 objetivos:

1) **Seguridad de pinout**  
   - No tocar pines no inicializados (fix Naveen1/pin0-RX1)  
   - Log obligatorio en `setup()` de: versión Zircon + modo UART

2) **Cinemática unificada**  
   - `moveOmni(vx, vy, w)` o `movePolar(angle, speed, w)`  
   - normalización y saturación consistente (0–100 o -100..100)

3) **IMU usable**  
   - Inicialización real de BNO055 en `begin()`  
   - calibración con procedimiento (y fallback seguro si falla)

**Pros**
- Elimina el "cada sketch maneja motores distinto" (causa raíz de bugs).
- Permite agregar heading control sin reescribir todo.

**Contras**
- Requiere refactor de todos los sketches (dolor inicial).
- Hay que testear en ambos PCBs.

---

### 5.4 Delantero 2026: mantener FSM, pero con estructura y sensores

**Propuesta base (pragmática):** FSM modular + fusión cámara/IR.

- Cámara: cuando la pelota está en FOV → control fino (centro + distancia).
- IR 8×: cuando NO hay pelota en FOV → giro al máximo IR y adquisición.

**Pros**
- Usa hardware que ya existe (los 8 IR dejan de ser decoración).
- Reduce el "me quedé ciego porque la pelota está atrás".

**Contras**
- Hay que calibrar IR (offsets + umbrales) y filtrar ruido.
- Fusión mal hecha puede crear oscilaciones (cámara dice "izq", IR dice "atrás").

**Alternativa ambiciosa:** Behavior Tree en vez de FSM  
**Pros:** modular, más legible en equipos grandes.  
**Contras:** para microcontroladores y alumnos nuevos, puede ser más difícil de depurar que FSM.

---

### 5.5 Arquero 2026: reescritura total con reglas claras (P1.4)

El arquero legado actual es "código mezclado" y no compila, así que conviene empezar de cero.

**Propuesta funcional mínima**
- Modo "línea de arco": mantener posición lateral con sensores de línea (y/o referencias de campo)
- Modo "bloqueo": cuando pelota viene al arco, priorizar detenerla (sin perseguirla como delantero)
- Ultrasonido: si se usa, que sea **no bloqueante** (NewPing o interrupciones)

**Pros:** base sólida y testeable.  
**Contras:** requiere definir claramente "qué es ser arquero" (estrategia) y testear mucho.

---

### 5.6 Visión: seguir con blobs… pero con disciplina (por ahora)

**Opción A — Mantener blob detection + calibración express**  
- Centralizar thresholds en `config.py`
- Validar automáticamente que L_min < L_max
- Tool de calibración completa (no stub) + procedimiento < 5 min

**Pros:** rápido, simple, suficiente si está bien calibrado.  
**Contras:** sensible a iluminación y pisos.

**Opción B — Modelo ML pequeño en OpenMV**  
**Pros:** robustez potencial ante cambios de luz y variaciones de pelota/arcos.  
**Contras:** dataset, entrenamiento, tiempo; riesgo de "no llega a andar" antes del mundial.

Recomendación: **Opción A para 2026** como baseline; ML puede vivir como rama experimental paralela.

---

## 6. Evaluación por áreas (mapa mental aplicado)

Alineado a las áreas: *Detección y Seguimiento*, *Hardware/Plataformas*, *Localización/Navegación*, *Control/Comms*, *Estrategia/Competición*.

- **Detección**: hay base (OpenMV + homografía) pero configuración inconsistente; prioridad P2.2.
- **Hardware**: Zircon dual (Mark1/Naveen1) es poder… y fuente de bugs; prioridad P1.2.
- **Localización**: IMU está pero no está "operativa" por software; prioridad P2.3.
- **Control/Comms**: UART es el cuello de botella; prioridad P1.1.
- **Estrategia**: striker y goalie necesitan comportamientos separados y testeables; prioridad P1.3/P1.4.

---

## 7. Recomendación final (la más importante)

La acción más "high-leverage" ahora mismo es:

1) **P0: reconstruir el código de competencia real 2025**  
2) **P1: protocolo UART + zirconLib2 + striker/goalie compilables**  
3) **P2: fusión IR + calibración + heading**  
4) **P3: requisitos internacionales 2026 (comm module, captura, handle, marker)**

Sin P0, todo lo demás es ingeniería sobre arena.

---

## 8. Referencias internas (repo)

- `research/completed/2026-02-21-arquitectura-sistema-2025.md`
- `research/completed/2026-02-21-revision-codigo-2025.md`
- `research/completed/2026-02-21-analisis-cruzado-verificacion-hipotesis.md`
- `research/completed/2026-02-21-mapa-prioridades-revisado.md`
- `legacy/2025-season/README.md`
- `legacy/2025-season/code/libraries/zirconLib/*`
- `legacy/2025-season/code/delantero/perseguir-pelota.ino`
- `legacy/2025-season/code/vision-openmv/calcula-coordenadas-pelota.py`
