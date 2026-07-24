[![](https://img.shields.io/badge/<_Prev_block-7B42BC?style=for-the-badge)](../07-maintain/README.md)
[![](https://img.shields.io/badge/Deck-175074?style=for-the-badge)](../README.md)
[![](https://img.shields.io/badge/Back_to_Deck_>-7B42BC?style=for-the-badge)](../README.md)

# Block 08 — HCP Terraform

> **Objetivo 8:** HCP Terraform (antes Terraform Cloud) — remote runs, state, registry privado, teams y políticas.

## 🃏 Cards in this block

> **Cardeado en modo "esenciales"** (obj 8 pesa modesto + time-box del plan). Cubre lo del examen; projects, variable sets, teams/RBAC y health/run-triggers quedan como **repaso ligero** (ver [outline S16](../../slides/tf_004_16_hcp_terraform.md)).

| # | Card | Concept |
|---|---|---|
| 01 | [What is HCP Terraform](./01-what-is-hcp-terraform.md) | Extiende TF (mismo código); remote state/exec; tiers (Free 500 recursos); execution modes Remote/Local/Agent _(S16)_ |
| 02 | [Connecting & authenticating](./02-connecting-and-authenticating.md) | `cloud` block (org + workspaces name/tags); `terraform login`; `TF_TOKEN_*`; token types (User/Team/Org/Audit) _(S16)_ |
| 03 | [HCP workspaces (vs CLI)](./03-hcp-workspaces.md) | HCP = entorno completo ≠ CLI (varios states); workflows CLI/VCS/API-driven _(S16)_ |
| 04 | [Private registry](./04-private-registry.md) | Módulos privados + curated public; VCS + git tags; `app.terraform.io/<ORG>/<NAME>/<PROVIDER>` _(S16)_ |
| 05 | [Policy enforcement (Sentinel & OPA)](./05-policy-enforcement-sentinel-opa.md) | Tras plan/antes de apply; Sentinel (3 niveles) vs OPA/Rego (2); policy sets _(S16)_ |

## 🎯 Suggested concepts to cover

- ✅ **Remote runs & remote state** managed by HCP. → card 01 _(S16)_
- ✅ **Workflows** CLI-driven / VCS-driven / API-driven; execution modes Remote/Local/Agent. → cards 01·03 _(S16)_
- ✅ **Private module registry** (source format, git tags, permissions). → card 04 _(S16)_
- ✅ **HCP workspaces ≠ CLI workspaces**. → card 03 + [comparativa](../../comparativas/cli-workspaces-vs-hcp-workspaces.md) _(S16)_
- ✅ **Sentinel & OPA** policy enforcement; cost estimation. → card 05 + [comparativa](../../comparativas/sentinel-vs-opa.md) _(S16)_
- 🟡 **Projects, variable sets, teams/RBAC** — repaso ligero, sin card (ver outline). _(S16)_
- 🟡 **Health assessments** (drift + continuous validation) y **run triggers** — repaso ligero, sin card. _(S16)_

## 🔗 Related comparisons

- ✅ [CLI workspaces vs HCP workspaces](../../comparativas/cli-workspaces-vs-hcp-workspaces.md)
- ✅ [Sentinel vs OPA](../../comparativas/sentinel-vs-opa.md) — frameworks + enforcement levels.

- HCP Terraform workspaces vs CLI workspaces
- CLI-driven vs VCS-driven vs API-driven runs

---

[![](https://img.shields.io/badge/<_Prev_block-7B42BC?style=for-the-badge)](../07-maintain/README.md)
[![](https://img.shields.io/badge/Deck-175074?style=for-the-badge)](../README.md)
[![](https://img.shields.io/badge/Back_to_Deck_>-7B42BC?style=for-the-badge)](../README.md)
