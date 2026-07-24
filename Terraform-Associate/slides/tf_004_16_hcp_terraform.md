# S16 — HCP Terraform (resumen de slides)

> Outline de la sección 16 del curso (Bryan Krausen). Tema: **HCP Terraform** (antes Terraform Cloud). La sección **más larga** (18 lectures / 3h07 / 92 slides).
> **No es transcripción** del PDF (`tf_004_16_hcp_terraform.pdf`, no versionado).
> **Alimenta:** bloque `08-hcp-terraform` (objetivo 8). ⚠️ **Cardeada en modo "esenciales"** (decisión del usuario + time-box del plan: obj 8 pesa modesto y es "explain/describe"). Cards solo para los 5 temas de examen; el resto se anota abajo como **repaso ligero, sin card**.

## Lectures — por tema

- **Intro to HCP Terraform:** servicio gestionado que **extiende** Terraform (mismo código, cambia **dónde y cómo** corre). Valor: remote state (cifrado, locking), remote execution, colaboración/RBAC, gobernanza (policy, cost estimation, drift). Community vs HCP. **Tiers:** Free (500 recursos gestionados) · Essentials · Standard · Premium. → **card 01**
- **Getting Connected & Authenticated:** bloque **`cloud`** (`organization` + `workspaces` por `name`/`project` o por `tags`). **`terraform login`** → token en `~/.terraform.d/credentials.tfrc.json` (texto plano). Env var alternativa **`TF_TOKEN_app_terraform_io`** (CI/CD sin browser). **Tipos de token API:** User (multi-org, = tu cuenta), Team (automatización, corre plan/apply), Organization (gestiona teams/workspaces, **no** corre plans), Audit (read-only, audit trail). → **card 02**
- **Understanding HCP Workspaces:** HCP workspace ≠ CLI workspace. CLI = varios state files de una misma config. HCP = **entorno completo** (config + state + variables + settings + credenciales + run history). Workflow types: **CLI-driven / VCS-driven / API-driven**. → **card 03**
- **Execution & Remote Operations:** remote operations = plan/apply corren en HCP (full features: Sentinel, cost estimation). **Execution mode por workspace: Remote (default) / Local (solo sincroniza state; NO evalúa variables del workspace) / Agent (self-hosted, para infra privada/on-prem)**. → **card 03** (incluido)
- **Organizing Workspaces with Projects** _(repaso ligero, sin card)_: Org > Project > Workspace. Cada workspace en **exactamente un** project. Default project (renombrable, no borrable). Permisos scoped por project, variable sets por project, herencia del execution mode. `project` en el bloque `cloud` (TF 1.6+). Solo se borran projects vacíos.
- **Sharing Variables with Variable Sets** _(repaso ligero, sin card)_: colecciones reutilizables de variables (TF + env vars). Scopes: **global / project / workspace**. Precedencia: **workspace-specific gana** sobre variable set; CLI/`TF_VAR_` ganan en CLI-driven; priority sets pueden forzar; empate en el mismo scope → orden **lexical** del nombre del set.
- **Teams & Permissions (RBAC)** _(repaso ligero, sin card)_: Users ∈ Teams ∈ Org. Permisos **aditivos** → **gana el más alto**. Least privilege. **Owners team** por defecto (admin total). Team visibility: visible/secret. Team tokens. Niveles: organization / project / workspace.
- **The HCP Private Registry:** módulos/providers **privados** (solo la org) + **curated public** (públicos aprobados). Publicados desde **VCS**, versionados con **git tags**. Formato source: **`app.terraform.io/<ORG>/<NAME>/<PROVIDER>`**. Publicar requiere permiso **Manage Private Registry**. → **card 04**
- **Policy Enforcement (Sentinel & OPA):** policy as code, evaluada **después del plan, antes del apply**. **Sentinel** (HashiCorp, lenguaje Sentinel; niveles: advisory / soft mandatory / hard mandatory) vs **OPA** (CNCF, lenguaje Rego; niveles: advisory / mandatory). **Policy sets** (un solo framework cada uno; scope global/project/workspace). Override requiere **Manage Policy Overrides**. Qué NO pueden: monitorizar tras deploy, borrar recursos existentes, analizar runtime. → **card 05**
- **Additional Features (health assessments & run triggers)** _(repaso ligero, sin card)_: **Health assessments** = drift detection + continuous validation (`check`/pre/postconditions), cada ~24h, Standard/Premium. **Run triggers** = un workspace dispara runs en otro al **apply exitoso** (hasta 20 source workspaces). Change requests, Explorer.

## Cards generadas desde esta sección

| Bloque | Cards |
|---|---|
| `08-hcp-terraform` | 01 what-is-hcp-terraform (+tiers) · 02 connecting-and-authenticating (cloud block, `terraform login`, token types) · 03 hcp-workspaces (vs CLI + execution modes + workflows) · 04 private-registry · 05 policy-enforcement-sentinel-opa |

**Comparativas creadas:** `sentinel-vs-opa`. (La `cli-workspaces-vs-hcp-workspaces` ya existía desde S9 → la card 03 la enlaza.)

**Temas deliberadamente sin card (repaso ligero):** projects, variable sets, teams/RBAC, health assessments, run triggers. Si en un simulacro cae algo de estos y falla → se convierte en card entonces.

> Reconciliación: la comparativa `cli-workspaces-vs-hcp-workspaces` (S9) se reaprovecha. El private registry cruza con módulos (`05`, formato source vs público). "Sensitive" en variables de workspace conecta con `04/16`. Continuous validation (health assessments) conecta con `04/14` (check/pre/postconditions).
