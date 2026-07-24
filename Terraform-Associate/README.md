[![](https://img.shields.io/badge/<-FF4859?style=for-the-badge)](../README.md)

# HashiCorp Certified: Terraform Associate (004)

> [!NOTE]
> Apuntes en construcción. Próxima certificación tras el [SAA](../SAA/README.md). Mismo enfoque: notas concisas de lo no obvio, no transcribir docs.
> **Versión 004** (actualización del 003): 8 objetivos en vez de 9 y evalúa **Terraform 1.12**.

## 🔗 Links

[![](https://img.shields.io/badge/Página_oficial_del_examen-7B42BC?style=for-the-badge)](https://developer.hashicorp.com/terraform/tutorials/certification-004)
[![](https://img.shields.io/badge/Guía_de_estudio_(objetivos)-7B42BC?style=for-the-badge)](https://developer.hashicorp.com/terraform/tutorials/certification-004/associate-study-004)
[![](https://img.shields.io/badge/Documentación_Terraform-7B42BC?style=for-the-badge)](https://developer.hashicorp.com/terraform/docs)

## 📋 Datos del examen

| | |
|---|---|
| **Examen** | Terraform Associate (004) — evalúa **Terraform 1.12** |
| **Preguntas** | Opción múltiple (≈57; confirmar al agendar) |
| **Duración** | 1 hora |
| **Coste** | $70.50 USD (+ impuestos; sin reintento gratis) |
| **Vigencia** | 2 años |
| **Formato** | Online proctored |

## 🎯 Dominios del examen (objetivos oficiales 004)

| # | Dominio | Apuntes |
|---|---|---|
| 1 | Learn about Infrastructure as Code (IaC) | [Bloque 01](./cards/01-iac/README.md) ✅ |
| 2 | Review Terraform fundamentals | [Bloque 02](./cards/02-fundamentals/README.md) ✅ |
| 3 | Use the core Terraform workflow | [Bloque 03](./cards/03-core-workflow/README.md) ✅ _(S4·S5)_ |
| 4 | Read and write configuration | [Bloque 04](./cards/04-configuration/README.md) ✅ _(S6·S7·S10·S13·S14; gap menor: dynamic blocks)_ |
| 5 | Use and create modules | [Bloque 05](./cards/05-modules/README.md) ✅ _(S12)_ |
| 6 | Learn about Terraform state management | [Bloque 06](./cards/06-state/README.md) ✅ _(local/remote backend, locking, drift/refresh, moved/removed, securing state)_ |
| 7 | Maintain infrastructure with Terraform | [Bloque 07](./cards/07-maintain/README.md) ✅ _(sub-obj 004: import · inspect state · verbose logging — todos cubiertos; `depends_on`/`lifecycle` extra)_ |
| 8 | Use HCP Terraform | [Bloque 08](./cards/08-hcp-terraform/README.md) ✅ _(S16 esenciales; projects/var-sets/teams/health en repaso ligero)_ |

## 📺 Temario del curso (referencia)

> Secciones del curso en vídeo que estoy siguiendo. **Las secciones 1-2 son intro + instalación** (no listadas en detalle). La columna "Bloque" indica a qué bloque de [`cards/`](./cards/README.md) alimenta cada sección. Total S3-S17: **~20 h** + 9 labs.

| Sección | Clases | Duración | Objetivo | Bloque cards |
|---|---|---|---|---|
| 3 — Terraform Foundations | 10 | 1h 02m | 1, 2 | 01, 02 |
| 4 — The Core Terraform Workflow | 13 | 1h 25m | 3 | 03 |
| 5 — Terraform CLI | 6 | 46m | 3 | 03 |
| 6 — Terraform File Structure and Organization | 1 | 11m | 4 | 04 |
| 7 — Terraform Configuration · Fundamentals | 12 | 2h 06m | 4 | 04 |
| 8 — Hands-On Labs · Writing Your First Configs (6 labs) | 7 | 1h 40m | 3, 4 | 03/04 |
| 9 — Understanding and Managing Terraform State | 14 | 1h 29m | 6 | 06 |
| 10 — Making Code Reusable (3 labs) | 10 | 2h 14m | 4 | 04 |
| 11 — Refactoring Terraform State | 5 | 41m | 6, 7 | 06, 07 |
| 12 — Terraform Modules | 8 | 1h 33m | 5 | 05 |
| 13 — Managing Resource Behavior and Dependencies | 5 | 41m | 7, 4 | 07, 04 |
| 14 — Securing Terraform Configurations | 9 | 1h 10m | 4, 6 | 04, 06 |
| 15 — Terraform Troubleshooting | 3 | 12m | 7 | 03 (refuerzo — `TF_LOG` ya en 03/09, sin cards) |
| 16 — HCP Terraform | 18 | 3h 07m | 8 | 08 |
| 17 — Exam Prep Essentials: What to Know for Each Objective | 11 | 1h 46m | todos | — (repaso; gap-fills a 04/05·05·08) |

## 🃏 Mazo de estudio

[![](https://img.shields.io/badge/Abrir_el_mazo-7B42BC?style=for-the-badge)](./cards/README.md)

El estudio principal vive en [`cards/`](./cards/README.md) — una flashcard por concepto, organizadas por los 8 objetivos. **Regla de oro de este examen:** por cada concepto, escríbelo y ejecútalo (`terraform apply`).

## 📊 Material de apoyo

| Carpeta | Para qué sirve |
|---|---|
| [`cards/`](./cards/README.md) | Mazo de flashcards (1 archivo = 1 concepto), 8 bloques = 8 objetivos. Estudio diario. |
| [`comparativas/`](./comparativas/README.md) | Tablas concepto-vs-concepto (`count` vs `for_each`, backend local vs remoto…). Consulta puntual. |
| [`glosario.md`](./glosario.md) | Términos del lenguaje/CLI y **pares que se confunden**. Ctrl+F en el repaso. |
| [`errors/`](./errors/README.md) | Doc maestro de errores de los simulacros (scorecard por objetivo + consolidado de cards). |
| [`questions/`](./questions/README.md) | Quizzes navegables estilo examen (respuestas las guardo yo). |
| [`assets/`](./assets/README.md) | Imágenes de soporte (ligero — casi todo es Mermaid inline). |
| [`planning/`](./planning/planning.md) | **Plan de estudio (examen: 31 jul 2026)** + [cheat-sheet final](./planning/cheat-sheet-final.md). |

## 🎯 Filosofía de estos apuntes

A diferencia del SAA (escenarios: "¿qué servicio?"), este examen es **práctico y preciso** (sintaxis HCL, comandos, comportamiento del state). Por eso:

- **No transcribir docs.** Solo lo no obvio, un flag/precedencia exacta, o lo que ya confundió.
- **Cada card muestra el código** (HCL o comando CLI) — no solo lo describe.
- **Las comparativas viven aparte** porque no caben en una card.
- **El consolidado de errores se llena tras cada simulacro**, no en frío.

---

[![](https://img.shields.io/badge/<-FF4859?style=for-the-badge)](../README.md)
