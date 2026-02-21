# Testing

Protocolos de prueba y resultados de experimentos.

## Estructura

- **protocols/**: Procedimientos paso a paso para ejecutar pruebas repetibles
- **results/**: Resultados de pruebas ejecutadas, con fecha y datos

## Formato de protocolos

```yaml
---
title: "Nombre del protocolo"
date: 2026-MM-DD
author: "Nombre"
tags: [area]
robot: arquero/delantero/ambos
---
```

Incluir: objetivo, materiales necesarios, pasos, criterios de éxito/fallo, y cómo registrar resultados.

## Formato de resultados

```yaml
---
title: "Resultado de prueba X"
date: 2026-MM-DD
author: "Nombre"
protocol: "link al protocolo"
robot: arquero/delantero/ambos
result: pass/fail/partial
tags: [area]
---
```

Incluir: condiciones de la prueba, datos medidos, observaciones, y conclusiones.
