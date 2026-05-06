# LuedanSkills

Repositorio para guardar y versionar skills reutilizables para agentes.

## Estructura

```text
.agents/
  skills/
    functional-spec-workshop/
      SKILL.md
```

## Agregar una nueva skill

1. Crea una carpeta dentro de `.agents/skills/` usando kebab-case.
2. Agrega un archivo `SKILL.md` dentro de esa carpeta.
3. Incluye frontmatter con `name` y `description`.

Ejemplo:

```md
---
name: nombre-de-la-skill
description: Describe cuándo debe usarse esta skill.
---

# Nombre de la Skill

Instrucciones de uso de la skill.
```
