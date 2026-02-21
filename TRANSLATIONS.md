# Translation Policy / Política de Traducción

> 🇬🇧 English is the canonical language for all documentation in this repository.
> 🇪🇸 Se permiten traducciones al español como ayuda para los alumnos.

## Convention / Convención

| Rule | Example |
|------|---------|
| English file (canonical, always required) | `rules-analysis-2026.md` |
| Spanish translation (optional) | `rules-analysis-2026.es.md` |
| Filenames always in English | ✅ `system-architecture.md` ❌ `arquitectura-sistema.md` |
| Date prefix when applicable | `2026-02-21-rules-analysis-2026.md` |

## Frontmatter Fields

Every document must include `lang` in its YAML frontmatter. Translations must reference the canonical file:

**English (canonical):**
```yaml
lang: en
translations:
  es: "./2026-02-21-rules-analysis-2026.es.md"
```

**Spanish (translation):**
```yaml
lang: es
translation-of: "./2026-02-21-rules-analysis-2026.md"
```

## Rules / Reglas

1. **English first**: The English version is the source of truth. If content diverges, English wins.
2. **Spanish is optional**: Not every document needs a Spanish translation. Focus on documents students read frequently (architecture, rules, roadmaps).
3. **Same directory**: Translations live next to the canonical file, not in separate folders.
4. **Sync responsibility**: When the English version is updated, the Spanish translation should be updated too. If the translation is outdated, add `translation-outdated: true` to its frontmatter.
5. **Code stays in English**: Variable names, function names, comments in code files remain in English. Code comments may include brief Spanish clarifications in parentheses if helpful for students.
6. **Journal entries**: Journal entries may be written in either language. If bilingual, prefer English as the main version.
7. **Technical terms**: Keep technical terms in English even in Spanish translations (e.g., "dribbler", "blob detection", "heading", "checksum"). Add a brief Spanish explanation in parentheses on first use if the term may be unfamiliar.

## Legacy Files / Archivos Legados

Documents created before this policy was adopted have Spanish filenames and content. These are kept as-is to avoid breaking cross-references. Their English canonical counterparts use English filenames and include a `legacy-es` field pointing to the original Spanish file:

```yaml
legacy-es: "./2026-02-21-arquitectura-sistema-2025.md"
```

Going forward, all new documents must follow the English-first naming convention.

## Why English? / ¿Por qué inglés?

- RoboCup is an international competition. Technical documentation, posters, TDPs, and interviews are all in English.
- Learning to document technical work in English is a valuable professional skill.
- Open-source collaboration (GitHub issues, PRs, README) is predominantly in English.
- Spanish translations help students who are still building their English skills understand the content faster, without slowing down the project.

---

*Policy established 2026-02-21 by Gustavo Viollaz (@gviollaz).*
