[![](https://img.shields.io/badge/<_Errors-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/<_Terraform-175074?style=for-the-badge)](../README.md)

# Terraform Practice Test 1 — Weak Points

**Result:** 85% (49/57). Passed (70% required). 2026-07-25 · Exam mode · 1h06m.

| Objetivo | Score | Priority |
|---|---|---|
| 1 · Infrastructure as Code (IaC) | 2/5 (40%) | 🔴 |
| 4 · Terraform Configuration | 9/12 (75%) | 🟡 |
| 5 · Terraform Modules | 5/6 (83%) | 🟢 |
| 8 · HCP Terraform | 5/6 (83%) | 🟢 |
| 2 · Core workflow · 3 · CLI · 6 · State · 7 · Maintain | 100% | ✅ |

> **Lectura:** conceptos "blandos" (IaC, patrones, T/F) fueron el punto débil, no la sintaxis. 5 de 8 fallos son de definición/patrón, no de código.

---

# 🔴 1. Infrastructure as Code (IaC)

## 1.1 Beneficio multi-cloud = **workflow consistente**, no el state (Q2)
**Sample question:** *"Org corre en AWS, Azure y GCP; el equipo lucha con herramientas/interfaces/workflows distintos. ¿Qué capacidad de Terraform lo resuelve?"*
- ✅ **"Consistent workflow to manage multiple providers"** — un solo lenguaje/flujo (`init→plan→apply`) sobre cualquier provider = lo que estandariza el despliegue.
- ❌ **State files provide centralized tracking** — el state **rastrea** recursos, no **estandariza** el proceso. Trampa: suena bien pero no ataca el problema (herramientas/interfaces distintas).
- ❌ Modules reusables — ayudan, pero la palabra clave del enunciado es *standardize the deployment process* → workflow.
> [!TIP]
> Cuando el enunciado dice *standardize / consistent process across clouds* → responde **provider-agnostic workflow**. State = tracking; modules = reuse; ninguno "estandariza el proceso".
> 📇 [Card: IaC benefits](../cards/01-iac/02-iac-benefits.md)

## 1.2 Immutable infrastructure **elimina** el config drift → True (Q37)
**Sample question:** *"T/F: el patrón immutable (reemplazar servidores en vez de modificarlos in-place) elimina el config drift porque los recursos nunca cambian tras el provisioning."*
- ✅ **True.** Immutable = tratar la infra como desechable; una actualización **reemplaza** el recurso por una versión nueva → no hay ventana para que el drift se acumule. Además simplifica rollback (re-desplegar versión previa).
- ❌ Marqué **False.** Confundí "los recursos nunca cambian" (correcto en immutable) con "imposible que cambien nunca".
> [!TIP]
> **Mutable** = parchear in-place → el drift se acumula. **Immutable** = replace → **elimina** drift. Terraform soporta immutable de forma natural (enfoque declarativo + `replace`).
> 📇 [Card: What is IaC](../cards/01-iac/01-what-is-iac.md)

## 1.3 Terraform enseña los cambios en la etapa **Plan** (Q51)
**Sample question:** *"¿En qué momento del workflow Terraform te muestra qué cambios hará antes de modificar la infra?"*
- ✅ **Plan stage** — compara current state vs desired y muestra el **execution plan** (create/update/delete) antes de tocar nada.
- ❌ Marqué **Apply stage** — el apply **ejecuta** el plan; el "final confirmation" que muestra apply es la aprobación, no el **preview** de cambios.
> [!TIP]
> **`plan` = preview** (qué haría). **`apply` = ejecuta** (y pide aprobación del plan). La pregunta "when does Terraform *show* changes" → **plan**, siempre.
> 📇 [Card: Declarative vs imperative](../cards/01-iac/03-declarative-vs-imperative.md) · workflow en [03-core-workflow](../cards/03-core-workflow/README.md)

---

# 🟡 4. Terraform Configuration

## 4.1 `depends_on` — cuándo (select two) (Q19)
**Sample question:** *"¿Cuándo usar `depends_on` en vez de la detección implícita? (select two)"*
- ✅ **Dependencias fuera del resource graph de Terraform** (sistemas externos que deben estar operativos antes).
- ✅ **B necesita A operativo pero no lo referencia** (dependencia oculta, sin `A.id` en la config). ← esta la acerté.
- ❌ Marqué **"prevenir creación en paralelo"** — `depends_on` **solo ordena** create/destroy según dependencias; **no** es un control de paralelismo.
> [!TIP]
> `depends_on` = **dependencias ocultas / externas** que Terraform no infiere de las referencias. **No** controla paralelismo ni es sustituto de una referencia directa (esa es la implícita y siempre preferida).
> 📇 [Card: depends_on](../cards/07-maintain/02-depends-on.md)

## 4.2 `for_each` sobre una lista → hay que convertir con `toset()` (Q27)
**Sample question:** *"`variable type = list(string)`, quieres un `kubernetes_namespace` por entrada con `for_each`. ¿Qué config maneja la conversión de tipo?"*
- ✅ `for_each = toset(var.namespaces)` — `for_each` exige **set** o **map**; `toset()` convierte la lista → set. (`tomap()` también valdría para map.)
- ❌ Marqué `for_each = var.namespaces` (lista directa) → **error**: `for_each` no acepta `list`.
- ❌ `tolist(...)` — sigue siendo lista, no la acepta `for_each`.
```hcl
resource "kubernetes_namespace" "ns" {
  for_each = toset(var.namespaces)   # list(string) -> set; each.key == each.value
  metadata { name = each.value }
}
```
> [!TIP]
> `for_each` **nunca** acepta un `list` directo → envuélvelo en **`toset()`** (set) o `tomap()` (map). Con set: `each.key == each.value`.
> 📇 [Card: for_each](../cards/04-configuration/13-for_each-meta-argument.md) · [comparativa count vs for_each](../comparativas/count-vs-for-each.md)

## 4.3 Asegurar el state (select two) (Q49)
**Sample question:** *"¿Qué es verdad sobre asegurar los state files? (select two)"*
- ✅ **Remote backends recomendados sobre local** para producción (seguridad, acceso, colaboración).
- ✅ **State locking previene modificaciones concurrentes** que corromperían el state. ← acertada.
- ❌ Marqué **"write-only arguments prevent ALL secrets from being stored in state"** — un write-only arg mantiene fuera del state **ese** secreto concreto, **no todos** los secretos del state. "All" es la trampa.
- ❌ Distractor: *"encrypted state elimina el marcado `sensitive`"* — falso; cifrar (at rest) y marcar `sensitive` (exposición accidental en output/log) son capas distintas y complementarias.
> [!TIP]
> Seguridad de state = **remote backend + locking + cifrado**. `sensitive` y encryption **no se sustituyen**. Write-only/ephemeral sacan **un** secreto del state, no lo hacen "libre de secretos".
> 📇 [Card: Securing state files](../cards/06-state/08-securing-state-files.md) · [Ephemeral & write-only](../cards/04-configuration/18-ephemeral-values-and-write-only-arguments.md) · [comparativa secret-protection](../comparativas/secret-protection-techniques.md)

---

# 🟢 5. Terraform Modules

## 5.1 GitHub como module source: shorthand válido (Q44)
**Sample question:** *"T/F: puedes usar un repo de GitHub como fuente con `source = \"github.com/hashicorp/example\"`."*
- ✅ **True.** Terraform detecta GitHub por el **shorthand** `github.com/...` (sin prefijo de protocolo), o con el prefijo git explícito `git::https://github.com/hashicorp/example.git`.
- ❌ Marqué **False** — asumí que siempre hace falta el prefijo de protocolo. El shorthand `github.com/...` es un caso especial reconocido.
> [!TIP]
> Fuentes Git válidas: `github.com/org/repo` (shorthand), `git::https://...`, `git::ssh://...`. HTTPS directo también funciona. Versión con `?ref=v1.2.0` o `//subdir` para subcarpeta.
> 📇 [Card: Module sources & versioning](../cards/05-modules/03-module-sources-and-versioning.md) · [comparativa module-source-types](../comparativas/module-source-types.md)

---

# 🟢 8. HCP Terraform

## 8.1 Los tres run workflows: VCS / API / CLI (Q21)
**Sample question:** *"¿Qué tipos de workflow hay para gestionar runs en HCP Terraform? (select three)"*
- ✅ **VCS-driven** (integra Git → store/track/versionado).
- ✅ **API-driven** (interacción programática vía API calls → automatización).
- ✅ **CLI-driven** (la que faltó marcar).
- ❌ Marqué **Agent-driven** — "Agent" es un **execution mode** (Remote / Local / **Agent** para redes aisladas), **no** uno de los tres *run workflows*. Distractor cercano.
> [!TIP]
> **3 run workflows** = **VCS · CLI · API**. **3 execution modes** = **Remote · Local · Agent**. No confundir *cómo se dispara el run* (workflow) con *dónde se ejecuta* (mode). "SSH-driven" y "Agent-driven" son señuelos.
> 📇 [Card: HCP workspaces (workflows)](../cards/08-hcp-terraform/03-hcp-workspaces.md)

---

## Summary — qué repasar

| Prioridad | Tema | Objetivo | Card |
|---|---|---|---|
| 🔴 | Beneficios IaC (workflow consistente vs state/modules) | 1 | [01-iac/02](../cards/01-iac/02-iac-benefits.md) |
| 🔴 | Mutable vs immutable (drift) | 1 | [01-iac/01](../cards/01-iac/01-what-is-iac.md) |
| 🔴 | plan = preview / apply = ejecuta | 1 | [01-iac/03](../cards/01-iac/03-declarative-vs-imperative.md) |
| 🟡 | `depends_on` (no controla paralelismo) | 4 | [07-maintain/02](../cards/07-maintain/02-depends-on.md) |
| 🟡 | `for_each` list → `toset()` | 4 | [04-config/13](../cards/04-configuration/13-for_each-meta-argument.md) |
| 🟡 | Asegurar state (remote+lock; write-only ≠ "all secrets") | 4 | [06-state/08](../cards/06-state/08-securing-state-files.md) |
| 🟢 | GitHub module source shorthand | 5 | [05-modules/03](../cards/05-modules/03-module-sources-and-versioning.md) |
| 🟢 | HCP: 3 workflows (VCS/CLI/API) vs 3 modes (Remote/Local/Agent) | 8 | [08-hcp/03](../cards/08-hcp-terraform/03-hcp-workspaces.md) |

**Patrón del test:** la sintaxis (state, core workflow, maintain) salió 100%; falló lo **conceptual/definicional** (IaC al 40%, T/F, "select N" con distractores de matiz). Enfocar repaso en **beneficios/patrones IaC** y en **distinguir conceptos cercanos** (workflow vs mode, encryption vs sensitive, depends_on vs paralelismo).

---

[![](https://img.shields.io/badge/<_Errors-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/<_Terraform-175074?style=for-the-badge)](../README.md)
