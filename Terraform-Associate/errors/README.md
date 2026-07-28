[![](https://img.shields.io/badge/<_Terraform-7B42BC?style=for-the-badge)](../README.md)

# Error Reviews — Practice tests

> Un archivo por simulacro (`test-review-N.md`) en formato *weak points*: scorecard por objetivo + errores agrupados por prioridad + tabla resumen. Tras cada simulacro: registrar errores y actualizar el **progreso por objetivo** y el **consolidado de cards** de abajo. (Mismo método que funcionó en el SAA.)

## Reviews por simulacro

| Test | Fecha | Score | Review |
|---|---|---|---|
| #1 | 2026-07-25 | 85% (49/57) ✅ | [test-review-1.md](./test-review-1.md) |

---

## Progreso por objetivo

> Actualiza esta tabla tras cada simulacro. Lectura = tendencia, no medida exacta.

| # | Objetivo | #1 | #2 | Lectura |
|---|---|---|---|---|
| 1 | Infrastructure as Code (IaC) | 40% (2/5) | — | 🔴 punto débil |
| 2 | Terraform fundamentals | 100% | — | 🟢 |
| 3 | Core workflow & CLI | 100% | — | 🟢 |
| 4 | Read & write configuration | 75% (9/12) | — | 🟡 |
| 5 | Modules | 83% (5/6) | — | 🟢 |
| 6 | State management | 100% | — | 🟢 |
| 7 | Maintain infrastructure | 100% | — | 🟢 |
| 8 | HCP Terraform | 83% (5/6) | — | 🟢 |

---

## Cards a repasar — consolidado

> Orden de prioridad: lo que **repites entre simulacros** o pesa más, primero. "Fallos" = veces que el tema cayó.

### 🔴 Crítico — crónico (≥2 tests) o ≥3 fallos

| Card | Tema | Objetivo | Fallos |
|---|---|---|---|
| _(none yet)_ | — | — | — |

> Nota: el **objetivo 1 (IaC)** cayó al 40% en el test #1 (3 fallos en 3 temas distintos). Ninguna card individual llega a 3 fallos aún, pero **vigilar el objetivo** — si reaparece en el test #2 sube a 🔴.

### 🟡 Importante — 1–2 fallos

| Card | Tema | Objetivo | Fallos |
|---|---|---|---|
| [01-iac/02](../cards/01-iac/02-iac-benefits.md) | Beneficios IaC: workflow consistente vs state/modules | 1 | 1 |
| [01-iac/01](../cards/01-iac/01-what-is-iac.md) | Mutable vs immutable (config drift) | 1 | 1 |
| [01-iac/03](../cards/01-iac/03-declarative-vs-imperative.md) | plan = preview / apply = ejecuta | 1 | 1 |
| [07-maintain/02](../cards/07-maintain/02-depends-on.md) | `depends_on` no controla paralelismo | 4 | 1 |
| [04-config/13](../cards/04-configuration/13-for_each-meta-argument.md) | `for_each` sobre list → `toset()` | 4 | 1 |
| [06-state/08](../cards/06-state/08-securing-state-files.md) | Asegurar state: remote+lock; write-only ≠ "all secrets" | 4 | 1 |
| [08-hcp/03](../cards/08-hcp-terraform/03-hcp-workspaces.md) | HCP: 3 workflows (VCS/CLI/API) vs 3 modes (Remote/Local/Agent) | 8 | 1 |
| [05-modules/03](../cards/05-modules/03-module-sources-and-versioning.md) | GitHub module source shorthand | 5 | 1 |

### ⚠️ Sin card — candidatos a crear

| Tema | Objetivo | Nota |
|---|---|---|
| _(none — todos los fallos tenían card)_ | — | — |

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
