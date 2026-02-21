# Investigación

Pipeline de investigación y análisis del equipo.

## Flujo de trabajo

```
backlog/          →  in-progress/       →  completed/
(temas por          (en análisis         (análisis terminado
 investigar)         activo)               con conclusiones)
```

- **backlog/**: Temas identificados que necesitan investigación. Cualquiera puede agregar temas.
- **in-progress/**: Temas que alguien está investigando activamente. Mover desde backlog cuando se empieza.
- **completed/**: Análisis terminados con conclusiones y recomendaciones. Mover desde in-progress.
- **references/**: Proyectos, repos, papers y recursos externos para estudiar y comparar.

## Formato de archivos

Nombre: `YYYY-MM-DD-tema-descriptivo.md`

Frontmatter obligatorio:
```yaml
---
title: "Título del tema"
date: 2026-MM-DD
author: "Nombre"
status: backlog/in-progress/completed
priority: alta/media/baja
tags: [area1, area2]
---
```

Para completed, agregar sección de **Conclusiones** y **Recomendaciones**.
