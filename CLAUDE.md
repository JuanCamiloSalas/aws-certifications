# CLAUDE.md — Repo de certificaciones (método compartido)

Contexto para Claude. Este repo son **mazos de estudio** para certificaciones (CCP, SAA, Terraform Associate…). Aquí vive el **método común a todos los cursos**. Cada curso tiene además su propio `<Curso>/CLAUDE.md` con las reglas **específicas del examen** — ⚠️ **el estilo de card y las plantillas NO son iguales entre cursos** (el de AWS/SAA es por escenarios; el de Terraform es práctico/código). Al trabajar dentro de una carpeta de curso, Claude carga ambos en cascada.

## ⭐ Principio rector: los READMEs de estudio son MÍNIMOS

El usuario **lee los READMEs para estudiar** → deben contener **solo contenido/índice**. Toda la **meta de autoría** (reglas de card, how-to-add, mapeos, skeletons, flujos) vive en los `CLAUDE.md`, **nunca** en un README de estudio. No volver a proponer meter meta en un README.

## Método de flashcards

- **1 concepto = 1 card**, un archivo por card.
- Los **bloques = los objetivos del examen** (p.ej. 8 bloques = 8 objetivos).
- Navegación lineal dentro de un bloque con botones **Prev / Block / Next** (badges shields.io, arriba y abajo de cada card).
- **No transcribir docs**: solo lo no obvio, un flag/precedencia exacta, o lo que ya confundió en un simulacro.

## Estructura de carpetas de un curso

| Carpeta/archivo | Para qué |
|---|---|
| `cards/` | Mazo. `_TEMPLATE.md` + `README.md` (índice) + `NN-bloque/` con cards `NN-nombre.md`. Bloques = objetivos. |
| `comparativas/` | Tablas concepto-vs-concepto. **Fuera** del flujo lineal de cards. |
| `glosario.md` | Términos del lenguaje/CLI + **pares que se confunden**. |
| `errors/` | Consolidado de errores de simulacros. |
| `questions/` | Quizzes navegables estilo examen. |
| `planning/` | Plan de estudio + cheat-sheet final. |
| `assets/` | Imágenes de soporte (preferir **Mermaid inline**). |
| `slides/` | Outlines `.md` de las slides del curso (los PDFs **no** se versionan). |

## Comparativas (convención)

- Una comparativa por archivo; **table-first** (tabla de decisión primero).
- Se crean **cuando aparece la confusión** (lab/simulacro), no en frío.
- Naming: `<tema-o-conceptos>.md`, minúsculas, separado por guiones.
- **Barra de nav** arriba y abajo (es el único "volver", porque no están en el flujo lineal). Segundo badge → README del curso.
- Al crear una: añadirla al índice del `comparativas/README.md` y enlazarla desde la(s) card(s) relacionada(s) (`🔄 Easily confused with`).

**Skeleton:**

```markdown
[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/<Curso>-175074?style=for-the-badge)](../README.md)

# X vs Y vs Z

> Resumen de una línea: cuándo gana cada uno.

## Decision table
| | X | Y | Z |
|---|---|---|---|
| Qué hace | … | … | … |
| Cuándo usar | … | … | … |
| Gotcha | … | … | … |

## When the exam picks each
- **X:** trigger / frase
- **Y:** trigger / frase

## Common traps
- …

## Linked cards
- [X card](../cards/NN-block/...)

---

[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/<Curso>-175074?style=for-the-badge)](../README.md)
```

## Flujo: generar cards al terminar una sección del curso

- **Disparador:** `cards SNN` (o "terminé la sección N").
- **Modo fiel (recomendado):** el usuario da la lista de clases y/o slides → cards calcadas, sin inventar cobertura. **Modo rápido:** solo `cards SNN`.
- **Input de slides:** suele bastar el índice de clases; el PDF solo hace falta si una slide trae algo no obvio (un flag, una precedencia, un default).
- **El mapeo sección→bloque es ORIENTATIVO, no camisa de fuerza:** antes de escribir, cruzar los temas **reales** de la sección con el objetivo del bloque destino y **remapear si no encaja**. Una sección puede **partirse** en varios bloques.
- Pasos: mapeo → bloque(s) correcto(s) · redactar con el template del curso · numerar · cablear prev/next · actualizar README del bloque + comparativas + glosario · registrar un outline en `slides/`.
- **Reúso sobre duplicar:** si un concepto ya está cardeado, enlazar/ampliar; no duplicar.

## Montar un curso nuevo

1. Crear la estructura de carpetas de arriba (READMEs **mínimos** + `cards/_TEMPLATE.md` + `comparativas/README.md`).
2. Crear **`<Curso>/CLAUDE.md`** con las reglas específicas del examen (estilo de card, idioma, objetivos, mapeo sección→bloque).
3. No copiar a ciegas el template de otro curso: **ajústalo al tipo de examen** (escenarios vs práctico/código, etc.).
