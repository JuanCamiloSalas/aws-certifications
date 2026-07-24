# CLAUDE.md — Terraform Associate (004)

Reglas **específicas de este examen**. El método común (flashcards, estructura, comparativas, flujo `cards SNN`, "READMEs mínimos") está en [`../CLAUDE.md`](../CLAUDE.md).

## Naturaleza del examen → estilo de card

A diferencia del **SAA** (escenarios: "¿qué servicio encaja?" → cards que abren con frases-gatillo), el **Terraform Associate es práctico y preciso**: sintaxis HCL, comandos CLI, comportamiento del state. Por eso:

- **Cada card práctica MUESTRA el código** (HCL o comando + los flags que importan) — es el corazón de la card.
- **`💻 Syntax / Example` es OBLIGATORIO en cards prácticas**; opcional en conceptuales (IaC/fundamentos) — no inventar HCL de relleno.
- Regla de oro: por cada concepto, **escríbelo y ejecútalo** (`terraform apply`).

## Idioma

**Cards en inglés** (el examen es solo en inglés → inmersión). La prosa meta y los READMEs, en español.

## Template y secciones

- Plantilla: [`cards/_TEMPLATE.md`](./cards/_TEMPLATE.md).
- **Siempre:** nav (Prev/Block/Next), título, `> Pitch`, `🎯 What the exam tests`, `🧠 Core`.
- **Según aplique:** `💻 Syntax / Example` (obligatorio en prácticas), `🚩 Flags & values to memorize`, `⚠️ Common traps`, `🔄 Easily confused with` (→ comparativa), `🖼️ Diagram`.
- **Una card por concepto.** Los bloques conceptuales (01-02) van densos: no una card por slide.

## Cómo añadir una card

1. Copiar `_TEMPLATE.md` al bloque `NN-…/`. 2. Renombrar `NN-nombre.md`. 3. Cablear prev/next (arriba **y** abajo). 4. Añadirla a la tabla del README del bloque. 5. Si va en medio, arreglar el `next` del anterior y el `prev` del siguiente.

## Bloques = 8 objetivos (004)

`01-iac` · `02-fundamentals` · `03-core-workflow` (workflow & CLI) · `04-configuration` (read & write config) · `05-modules` · `06-state` · `07-maintain` · `08-hcp-terraform`.

## Mapeo sección del curso → bloque (ORIENTATIVO — verificar y remapear)

| Sección curso | Bloque(s) |
|---|---|
| S3 Foundations | 01 + 02 |
| S4 Core Workflow · S5 CLI | 03 |
| S6 File Structure · S7 Config · **S10 Making Code Reusable** · S14 (secrets en config) | 04 |
| S8 Hands-On Labs | refuerzan 03/04 |
| S9 State · S11 (`moved`/`removed`) · S14 (securing state) | 06 |
| S11 (`import`) · S13 (`depends_on`/`lifecycle`) | 07 |
| S12 Modules | 05 |
| S13 (validación: variable validation, pre/post, checks) | 04 |
| S15 Troubleshooting (`TF_LOG`) | **refuerza `03/09`, sin cards** |
| S16 HCP Terraform | 08 |
| S17 Exam Prep | repaso global — rellenar huecos, **no cards nuevas** |

**Remaps ya aplicados (por si refactorizas):** S10 tiene título "reusable" pero es lenguaje de config → 04, no 05; S11/S13/S14 se **partieron** por objetivo; S15 no generó card (todo era `TF_LOG`, ya en `03/09`); S17 solo rellenó huecos editando cards existentes.

## Notas del curso

- **Reúso sobre duplicar** — hay mucho cruce entre 04/06/07 (secretos, state, meta-args); enlazar antes que repetir.
- **Time-box en S16 (HCP):** objetivo modesto y "explain/describe" → cardear esenciales, no exhaustivo (projects/variable-sets/teams/health quedaron como repaso ligero, ver [outline S16](./slides/tf_004_16_hcp_terraform.md)).
- **Estado:** los 8 objetivos cardeados (deck completo). Falta simulacros + repaso dirigido (`errors/`, cheat-sheet).
