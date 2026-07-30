[![](https://img.shields.io/badge/<_Terraform-7B42BC?style=for-the-badge)](../README.md)

# Error Reviews — Practice tests

> Un archivo por simulacro (`test-review-N.md`) en formato *weak points*: scorecard por objetivo + errores agrupados por prioridad + tabla resumen. Tras cada simulacro: registrar errores y actualizar el **progreso por objetivo** y el **consolidado de cards** de abajo. (Mismo método que funcionó en el SAA.)

## Reviews por simulacro

| Test | Fecha | Score | Review |
|---|---|---|---|
| #1 | 2026-07-25 | 85% (49/57) ✅ | [test-review-1.md](./test-review-1.md) |
| #2 | 2026-07-28 | 80% (46/57) ✅ | [test-review-2.md](./test-review-2.md) |

---

## Progreso por objetivo

> Actualiza esta tabla tras cada simulacro. Lectura = tendencia, no medida exacta.

| # | Objetivo | #1 | #2 | Lectura |
|---|---|---|---|---|
| 1 | Infrastructure as Code (IaC) | 40% (2/5) | 100% (4/4) | 🟢 recuperado (era 🔴) |
| 2 | Terraform fundamentals | 100% | 86% (6/7) | 🟢 |
| 3 | Core workflow & CLI | 100% | 78% (7/9) | 🟡 bajó |
| 4 | Read & write configuration | 75% (9/12) | 92% (12/13) | 🟢 subió |
| 5 | Modules | 83% (5/6) | 63% (5/8) | 🔴 punto débil (bajó bajo 70%) |
| 6 | State management | 100% | 86% (6/7) | 🟢 |
| 7 | Maintain infrastructure | 100% | 75% (3/4) | 🟡 bajó |
| 8 | HCP Terraform | 83% (5/6) | 60% (3/5) | 🔴 punto débil (bajó bajo 70%) |

---

## Cards a repasar — consolidado

> Orden de prioridad: lo que **repites entre simulacros** o pesa más, primero. "Fallos" = veces que el tema cayó.

### 🔴 Crítico — crónico (≥2 tests) o ≥3 fallos

| Card | Tema | Objetivo | Fallos |
|---|---|---|---|
| [05-modules/03](../cards/05-modules/03-module-sources-and-versioning.md) | `source` shape (registry/local `./`/git) + `version` optional | 5 | 3 (T1·Q44 · T2·Q30 · T2·Q23) |
| [01-iac/03](../cards/01-iac/03-declarative-vs-imperative.md) + [03-core/03](../cards/03-core-workflow/03-terraform-plan.md) | `plan` = preview/review · `apply` = ejecuta | 1·3 | 2 (T1·Q51 · T2·Q42) |
| [08-hcp/01](../cards/08-hcp-terraform/01-what-is-hcp-terraform.md) + [03](../cards/08-hcp-terraform/03-hcp-workspaces.md) | HCP: **workflow** (CLI/VCS/API) vs **execution mode** (Remote/Local/Agent) — dónde/cómo corre | 8 | 2 (T1·Q21 · T2·Q21) |

> **Módulos = punto débil #1 del T2** (63%, 3 fallos). La misma card (05-modules/03) cae en los dos simulacros. Repasar módulos en bloque antes del siguiente intento.

### 🟡 Importante — 1–2 fallos

| Card | Tema | Objetivo | Fallos |
|---|---|---|---|
| [05-modules/04](../cards/05-modules/04-module-inputs-outputs-scope.md) | Args del `module` block = **inputs** al hijo (no outputs/declaraciones) | 5 | 1 (T2·Q18) |
| [08-hcp/06](../cards/08-hcp-terraform/06-run-triggers.md) | Run trigger = apply del source encola run en el destination | 8 | 1 (T2·Q45) |
| [06-state/03](../cards/06-state/03-inspecting-state.md) | `show` (todo, sin args) vs `state show <addr>`; JSON es opt-in | 3 | 1 (T2·Q12) |
| [06-state/07](../cards/06-state/07-removed-block.md) | Decomisionar = borrar bloque+apply; `removed` = mantener vivo | 7 | 1 (T2·Q46) |
| [04-config/03](../cards/04-configuration/03-provider-block.md) | Dependencia de provider: ref en config / declaración / state (no plugin local) | 2 | 1 (T2·Q26) |
| [04-config/05](../cards/04-configuration/05-variable-block.md) | `map(string)` (lookup por key) vs `object` (esquema fijo) | 4 | 1 (T2·Q39) |
| [07-maintain/02](../cards/07-maintain/02-depends-on.md) | `depends_on` no controla paralelismo | 4 | 1 (T1·Q19) |
| [04-config/13](../cards/04-configuration/13-for_each-meta-argument.md) | `for_each` sobre list → `toset()` | 4 | 1 (T1·Q27) |
| [06-state/08](../cards/06-state/08-securing-state-files.md) | Asegurar state: remote+lock; write-only ≠ "all secrets" | 4 | 1 (T1·Q49) |
| [01-iac/02](../cards/01-iac/02-iac-benefits.md) | Beneficios IaC: workflow consistente vs state/modules | 1 | 1 (T1·Q2) ✅ recuperado T2 |
| [01-iac/01](../cards/01-iac/01-what-is-iac.md) | Mutable vs immutable (config drift) | 1 | 1 (T1·Q37) ✅ recuperado T2 |

> IaC (objetivo 1) pasó de 40% → 100%: las tres cards de IaC del T1 se dan por **recuperadas** (el repaso funcionó); se dejan aquí como histórico.

### ⚠️ Sin card — candidatos a crear

| Tema | Objetivo | Nota |
|---|---|---|
| _(none — todos los fallos tienen card)_ | — | Run triggers (T2·Q45) → creada [08-hcp/06](../cards/08-hcp-terraform/06-run-triggers.md). |

---

## Cómo registrar un nuevo simulacro

1. Sube los screenshots de las preguntas a `errors/test-N/`.
2. Crea `errors/test-review-N.md` copiando el formato de un review existente (scorecard → secciones por objetivo → summary). Mientras no exista uno, usa el esqueleto de abajo.
3. Actualiza la tabla **Reviews por simulacro**, el **Progreso por objetivo** y el **consolidado de cards** (suma los `Fallos` de los temas que reaparezcan; sube a 🔴 lo que caiga en ≥2 tests).

### Esqueleto de `test-review-N.md`

```markdown
# Terraform Practice Test N — Weak Points

**Result:** XX% (NN/MM). YYYY-MM-DD.

| Objetivo | Score | Priority |
|---|---|---|
| ... | ... | 🔴/🟡/🟢 |

# 🔴 1. <Objetivo> 

## 1.1 <Tema fallado>
**Sample question:** *"..."*
- ✅ correcto — por qué.
- ❌ distractor — por qué no.
> [!TIP]
> Regla de examen: ...
> 📇 [Card: ...](../cards/NN-block/...)
```

---

[![](https://img.shields.io/badge/<_Terraform-7B42BC?style=for-the-badge)](../README.md)
