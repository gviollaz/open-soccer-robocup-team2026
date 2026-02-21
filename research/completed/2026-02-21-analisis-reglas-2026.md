---
title: "Análisis completo de reglas Soccer 2026"
date: 2026-02-21
author: "Claude (Anthropic - Claude Opus 4.6)"
status: completed
priority: alta
tags: [reglas, competencia, 2026, analisis]
---

# Análisis completo de reglas RoboCupJunior Soccer 2026

Fuente: https://robocup-junior.github.io/soccer-rules/2026-soccer-draft-rules/rules.html  
Fecha de las reglas: 2026-01-21

## Cambio de nombres

- **Soccer Open** ahora se llama **Soccer Vision**
- **Soccer Lightweight** ahora se llama **Soccer Infrared**
- Nuestro equipo compite en **Soccer Vision**

## Restricciones físicas del robot (Soccer Vision)

| Parámetro | Límite | Notas |
|----------|--------|-------|
| Diámetro máximo | **18.0 cm** | Debe caber suavemente en un cilindro de este diámetro |
| Altura máxima | **18.0 cm** | Medido con todas las partes extendidas |
| Peso máximo | **Sin límite** | Soccer Vision no tiene restricción de peso |
| Zona de captura de pelota | **1.5 cm** | ⚠️ **CAMBIADO de 3.0 cm a 1.5 cm** — la pelota no puede entrar más de 1.5cm en la envolvente convexa del robot |
| Voltaje máximo | 48V DC / 25V AC RMS | Debe ser medible durante inspecciones |
| Comunicación entre robots | 2.4 GHz, máx 100 mW EIRP | El espectro no está garantizado |

**Tip oficial**: Estar 3mm por debajo del límite de tamaño y 50g por debajo del límite de peso hace que las inspecciones sean más fluidas.

## Pelota

- **Soccer Vision usa pelota de golf naranja pasiva** (42mm diámetro) — **SIN CAMBIO para nuestra liga**
- El cambio de pelota de 74mm a 42mm es solo para la liga Infrared
- Pero la **zona de captura cambió de 3.0 a 1.5 cm** — esto SÍ nos afecta

## Zona de captura de pelota — CAMBIO CRÍTICO

La zona de captura se redujo a la mitad. Esto significa:
- El dribbler NO puede abrazar tanto la pelota como antes
- Hay que revisar el diseño mecánico del frontal del robot
- La pelota debe ser accesible por otros robots en todo momento
- Se define como: espacio interno creado al colocar un borde recto en los puntos salientes del robot

## Communication Module — OBLIGATORIO en internacional

- El Soccer League Committee provee un módulo de comunicación a cada equipo
- Interfaz: **un pin GPIO de 2.54mm** para start/stop
- El módulo puede exceder la altura máxima del robot
- Debe estar a **al menos 1cm** del exterior del robot
- Debe estar protegido de impactos
- Alimentación: GND y BAT+ directo a batería (5.3V-25V) sin switch. Debe poder suministrar **500mA** en todo momento
- Debe estar encendido todo el tiempo durante el partido (incluso cuando el robot está fuera del campo)
- Si el voltaje de batería no está entre 5.3V-25V, se puede usar un pin 3V3
- **No cuenta para el límite de peso**
- Más info: https://github.com/robocup-junior/soccer-communication-module
- Futuro: planean extender a UART para aplicaciones más complejas

## Handle (asa de transporte)

- OBLIGATORIO: asa estable y visible
- Debe permitir levantar el robot desde al menos **5 cm por encima** de la estructura más alta
- Mínimo **5 cm de espacio** para las manos entre la parte más alta del robot y el asa
- El asa puede exceder el límite de altura
- El peso del asa cuenta en el peso total del robot

## Top Markers

- Círculo plástico blanco de al menos **4 cm de diámetro**
- Montado horizontalmente en la parte superior
- El árbitro escribe números en él con marcador
- No necesita estar dentro del límite de tamaño
- Robots sin top marker no pueden jugar

## Colores prohibidos en el robot

- No se puede usar **naranja, amarillo o azul** visibles
- Partes de esos colores deben estar ocultas o pintadas/tapadas con color neutro
- No producir luz visible que interfiera con la visión del oponente
- No producir interferencia magnética

## Kicker — test de potencia

1. Colocar el robot dentro del arco, tocando la pared de fondo
2. Realizar un kick hacia el arco contrario
3. **Pasa el test si**: después de rebotar en el arco contrario, la pelota NO vuelve hasta la pared de fondo del arco desde donde se disparó

Esto es más permisivo que en 2025 (antes la pelota no podía pasar la línea del área penal).

## Gameplay — puntos clave

- 2 robots por equipo, 2 tiempos de 10 minutos con 5 min de descanso
- Llegar 5 min antes del partido (penalidad: 1 gol por cada 30 seg de demora)
- Diferencia máxima en el score final: 10 goles
- Out of bounds: 1 minuto de penalidad (si toca pared o entra completamente al área)
- Robot dañado: mínimo 1 minuto fuera o hasta próximo kickoff
- Si ambos robots del mismo equipo están dañados: 1 gol cada 30 seg al oponente
- Los robots DEBEN poder acercarse y tocar la pelota (o se los considera dañados)
- Pushing: si atacante y defensor se tocan en el área penal con contacto con la pelota

## Documentación obligatoria

1. **Poster técnico**: máximo A1 (60×84cm). Resumen del diseño. Se cuelga en el venue y se comparte después.
2. **Technical Description Video**: demo del robot funcional, proceso de diseño, innovación
3. **BOM (Bill of Materials)**: nombre componente, proveedor, estado (nuevo/reusado), kit/custom, **precio**
4. **Portfolio digital**: envío antes de la competencia
5. **Código y diseños**: OBLIGATORIO compartir hardware y software en GitHub repos de RoboCupJunior

## Entrevistas

- Se realizan durante el Setup Day
- Cada miembro debe tener un **rol técnico definido** (mecánica, electrónica, software)
- Deben poder explicar no solo QUÉ hace cada componente sino CÓMO funciona
- La entrevista es parte del scoring (puntaje)

## Technical Challenges

- Desafíos técnicos adicionales (desconocidos hasta el día de la competencia)
- Cualquier equipo puede participar
- Pueden influir en reglas futuras

## SuperTeam

- Competencia donde equipos de diferentes países comparten robots
- Se juega en un campo más grande ("Big Field")
- Puede requerir lentes/sensores optimizados para campos más grandes
- Reglas: https://robocup-junior.github.io/soccer-rules/master/superteam_rules.html

## Equipo — requisitos

- 2-4 miembros (planean aumentar a 5 en el futuro)
- Edad: 14-19 años al 1 de julio de 2026
- Mentor: 19+ años, debe estar presente durante todos los eventos oficiales
- Al menos 2 miembros presentes en el sitio (1 solo puede competir pero no ser elegible para finales/premios)
- Prohibido recibir ayuda remota durante la competencia (llamadas, videollamadas, remote desktop)

## Cambios específicos 2025 → 2026

| Cambio | Antes (2025) | Ahora (2026) | Impacto para nosotros |
|--------|-------------|-------------|----------------------|
| Nombre liga | Soccer Open | Soccer Vision | Solo nombre |
| Zona captura pelota | 3.0 cm | **1.5 cm** | **CRÍTICO** — revisar dribbler |
| Peso IR league | 1400g | 1500g | No nos afecta (Vision) |
| Pelota IR | 74mm | 42mm | No nos afecta (ya usamos golf ball) |
| Pushing + multiple defense | No especificado | Se resuelve pushing primero | Regla de juego |
| Test kicker | Pedir muestra de kick | Testear potencia del kicker | Más formal |

## Conclusiones y acciones

1. **URGENTE**: Verificar que el dribbler cumple con la zona de captura de 1.5cm (era 3.0cm)
2. **IMPORTANTE**: Integrar el Communication Module en el diseño electrónico (pin GPIO + alimentación 500mA)
3. **IMPORTANTE**: Preparar asa con 5cm de clearance
4. **IMPORTANTE**: Preparar top marker blanco de 4cm
5. **NECESARIO**: Empezar a armar BOM con precios
6. **NECESARIO**: Cada miembro debe poder explicar su rol técnico en la entrevista
7. **PREPARAR**: Poster A1, video técnico, portfolio digital, código para compartir
