# S17 — Exam Prep Essentials (resumen de slides)

> Outline de la sección 17 del curso (Bryan Krausen). Tema: **repaso condensado de los 8 objetivos** con "EXAM TIPS". 65 slides simplificadas para imprimir/repasar.
> **No es transcripción** del PDF (`terraform_associate_004_exam_prep.pdf`, no versionado).
> **Sin cards nuevas** — es repaso global. Confirma ~90% de la cobertura del deck; se usó para **rellenar huecos** (mejoras a cards existentes), según el plan ("S17 = rellenar huecos, no cards nuevas").

## Huecos rellenados (gap-fills a cards existentes)

| Hueco (EXAM TIP de S17) | Card mejorada |
|---|---|
| **Tipos estructurales `object({...})` / `tuple([...])`** (schema fijo / longitud fija, tipos mixtos) vs map/list | [`04/05 variable-block`](../cards/04-configuration/05-variable-block.md) |
| **Los providers SÍ se heredan del padre; las variables NO** (trap clásico) | [`05/04 inputs-outputs-scope`](../cards/05-modules/04-module-inputs-outputs-scope.md) |
| **El bloque `cloud` REEMPLAZA al `backend`** — no se pueden usar ambos | [`08/02 connecting-and-authenticating`](../cards/08-hcp-terraform/02-connecting-and-authenticating.md) |
| **Speculative plan** en VCS-driven (push→plan-only; merge→plan+approval) | [`08/03 hcp-workspaces`](../cards/08-hcp-terraform/03-hcp-workspaces.md) |

## Confirmado ya cubierto (no requiere cambios)

- **Obj 1-2:** declarative vs imperative, IaC "big four", multi-cloud, provider install (`init` → `.terraform/providers/` + `.terraform.lock.hcl`), `required_providers` vs `provider`, `alias`, qué hace/no hace el state.
- **Obj 3:** core workflow, `init`/`validate`/`plan`/`apply`/`destroy`/`fmt`; **plan NO es requerido antes de apply** y **saved plan salta el prompt** (ya en `03/05`); cuándo re-`init`.
- **Obj 4:** resource vs data, referencias, variables/outputs, precedencia, `sensitive` (no cifra), ephemeral/write-only, funciones, dependencias implícita/explícita, `lifecycle`, validación (check = warning; pre/post), Vault (data source).
- **Obj 5:** por qué módulos, root/child, count/for_each en módulos, source formats (registry/git/local), version solo registry, `~>`, scope de variables, re-`init` al añadir módulo.
- **Obj 6:** local vs remoto, state locking (auto en apply/destroy/import), backend sin variables (`-backend-config`), `-migrate-state`, `state list/show`, moved/removed/import, refresh-only.
- **Obj 7:** import workflow (escribir resource primero; `-generate-config-out`), `state show/list`, `output`/`-json`, `TF_LOG`/`TF_LOG_PATH`, **`terraform apply -replace=ADDR`** (ya mencionado en `03/05`).
- **Obj 8:** `terraform login`, `cloud` block, HCP vs CLI workspaces, variable sets (scopes), execution modes (local = state remoto igual), workflows, collaboration features (run triggers, policy, health, drift, explorer, teams, private registry, projects).

## Cards generadas desde esta sección

**Ninguna.** Solo gap-fills a 4 cards existentes + este registro. El resto de la S17 valida cobertura ya existente.

> El PDF termina con la lista completa de sub-objetivos oficiales por objetivo (útil como checklist final de repaso; ya reflejada en la tabla de dominios del [README](../README.md)).
