[![](https://img.shields.io/badge/<_Prev_block-7B42BC?style=for-the-badge)](../03-core-workflow/README.md)
[![](https://img.shields.io/badge/Deck-175074?style=for-the-badge)](../README.md)
[![](https://img.shields.io/badge/Next_block_>-7B42BC?style=for-the-badge)](../05-modules/README.md)

# Block 04 — Read & write configuration

> **Objetivo 4:** variables, outputs, data sources, expresiones y funciones. **Bloque pesado** — sintaxis HCL pura, ideal para cards con snippets.

## 🃏 Cards in this block

| # | Card | Concept |
|---|---|---|
| 01 | [Resource referencing](./01-resource-referencing.md) | Attribute references → implicit dependencies _(seen in S3)_ |
| 02 | [File structure & organization](./02-file-structure.md) | All `.tf` merged; conventional filenames; state/lock/gitignore; blast radius _(S6)_ |
| 03 | [Provider block](./03-provider-block.md) | `provider "…"`; auth via env vars; one provider→many resources; `alias` _(S7)_ |
| 04 | [Resource & data blocks](./04-resource-and-data-blocks.md) | resource (create) vs data (read); type/name; naming rules _(S7)_ |
| 05 | [Variable block & types](./05-variable-block.md) | `variable`; description/type/default; string/number/bool/list/map/set _(S7)_ |
| 06 | [Variable precedence](./06-variable-precedence.md) | Assign values + order (default < env < tfvars < auto.tfvars < `-var`) _(S7)_ |
| 07 | [Output block](./07-output-block.md) | `output`; value/description/sensitive; module integration; interpolation _(S7)_ |
| 08 | [Terraform block & version constraints](./08-terraform-block.md) | `required_version` · `required_providers` · `backend`; `=`/`>=`/`~>` _(S7)_ |
| 09 | [Built-in functions](./09-built-in-functions.md) | `name(args)`; no user-defined fns; type/numeric/string/collection/CIDR; `terraform console` _(S10)_ |
| 10 | [Dynamic values](./10-dynamic-values.md) | Interpolation `${…}`; data sources are read-only; compose var+local+data _(S10)_ |
| 11 | [Local values](./11-locals.md) | `locals {}` (define) vs `local.` (ref); computed inside; not settable from outside _(S10)_ |
| 12 | [`count` meta-argument](./12-count-meta-argument.md) | N copies by index; `count.index`; reindex gotcha; `cond ? 1 : 0` _(S10)_ |
| 13 | [`for_each` meta-argument](./13-for_each-meta-argument.md) | Map/set → per-key; `each.key`/`each.value`; stable keys; `toset(list)` _(S10)_ |
| 14 | [Custom conditions & validation](./14-custom-conditions-and-validation.md) | variable validation · pre/postcondition · `check` (top-level, warns) _(S13)_ |
| 15 | [Securing secrets (overview)](./15-securing-secrets.md) | Dónde se filtran (logs/state/git); defensa por capas _(S14)_ |
| 16 | [Sensitive variables & outputs](./16-sensitive-variables-and-outputs.md) | `sensitive = true` enmascara output/logs; **NO** cifra ni saca del state _(S14)_ |
| 17 | [Secrets: env vars & external sources](./17-secrets-env-vars-and-external-sources.md) | `TF_VAR_*` + Secrets Mgr/Vault; fuera de git pero **siguen en state**; Vault dynamic creds _(S14)_ |
| 18 | [Ephemeral values & write-only args](./18-ephemeral-values-and-write-only-arguments.md) | TF 1.10+; solo en memoria, **fuera del state**; `ephemeral`/write-only _(S14)_ |

> **S10 "Making Code Reusable"** entra aquí (no en el bloque 05): functions, interpolación, locals, `count`/`for_each` = objetivo 4. La **validación** de **S13** también aterriza aquí (card 14). De **S14** vienen las cards 15-18 (secretos en config, obj 4b); el ángulo de **securing state** de S14 vive en [`06/08`](../06-state/08-securing-state-files.md) (obj 6). `depends_on`/`lifecycle` de S13 van al bloque [`07`](../07-maintain/README.md).

## 🎯 Suggested concepts to cover

- ✅ **File structure**: all `.tf` merged; conventional filenames; state/lock/gitignore; blast radius. → card 02 _(S6)_
- ✅ **Provider block**: `provider "…"`, auth via env vars, `alias` / `provider =`. → card 03 _(S7)_
- ✅ **Resource & data blocks**: create vs read; type/name; naming rules. → card 04 _(S7)_
- ✅ **Input variables**: `type`, `default`, `description`; primitive + collection types. → card 05 _(S7)_ · `validation` in S13 (card 14); `sensitive` in S14 (card 16).
- ✅ **Variable precedence** (low→high): defaults → `TF_VAR_*` → `terraform.tfvars` → `*.auto.tfvars` → `-var`/`-var-file`. → card 06 _(S7)_
- ✅ **Outputs**: `value`, `sensitive`, `description`; consumed by parent modules. → card 07 _(S7)_
- ✅ **Terraform block**: `required_version`, `required_providers`, `backend`; version constraints `=`/`>=`/`~>`. → card 08 _(S7)_
- ✅ Resource addressing & **references** (`resource.name.attr`), implicit dependencies. → card 01
- ✅ **Types & complex structures**: string/number/bool, list, map, set + structural `object`/`tuple`. → card 05 _(object/tuple added from S17 exam-prep)_
- ✅ **Local values** (`locals` define / `local.` ref) and when to use them vs variables. → card 11 _(S10)_
- ✅ **Built-in functions** (`name(args)`; no user-defined; type/numeric/string/collection/CIDR) — test with `terraform console`. → card 09 _(S10)_
- ✅ **Dynamic values / interpolation** (`${…}`, compose var+local+data; data sources read-only). → card 10 _(S10)_
- ✅ **Meta-arguments `count` / `for_each`** (index vs stable keys). → cards 12·13 _(S10)_ · [comparativa](../../comparativas/count-vs-for-each.md)
- ✅ **Custom conditions & validation** (variable validation, pre/postcondition, `check`). → card 14 _(S13)_ · [comparativa](../../comparativas/validation-mechanisms.md)
- ✅ **Secure secret injection** (obj 4b): where secrets leak, `sensitive`, env vars/external sources, ephemeral/write-only. → cards 15-18 _(S14)_ · [comparativa](../../comparativas/secret-protection-techniques.md)
- ⬜ Dynamic blocks; more expressions (conditionals `? :`, `for` expressions, splat `[*]`). _(minor gap — no dedicated lecture)_

## 🔗 Related comparisons

- ✅ [count vs for_each](../../comparativas/count-vs-for-each.md) — numeric index vs stable keys.
- ✅ [validation mechanisms](../../comparativas/validation-mechanisms.md) — variable validation vs pre/postcondition vs `check`.
- ✅ [secret protection techniques](../../comparativas/secret-protection-techniques.md) — sensitive vs env vars vs external vs ephemeral (logs/state/git).

_(still to create in [`../../comparativas/`](../../comparativas/) when they appear)_

- Input variables vs locals vs outputs
- list vs set vs map vs object

---

[![](https://img.shields.io/badge/<_Prev_block-7B42BC?style=for-the-badge)](../03-core-workflow/README.md)
[![](https://img.shields.io/badge/Deck-175074?style=for-the-badge)](../README.md)
[![](https://img.shields.io/badge/Next_block_>-7B42BC?style=for-the-badge)](../05-modules/README.md)
