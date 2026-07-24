[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)

# Sentinel vs OPA (HCP Terraform policy frameworks)

> One-line summary: two policy-as-code frameworks for HCP Terraform, same goal (gate the plan before apply) — **Sentinel** = HashiCorp, 3 enforcement levels · **OPA** = open source (CNCF), Rego language, 2 levels. You can use both in the same workspace.

## Decision table

| | Sentinel | OPA (Open Policy Agent) |
|---|---|---|
| Maintained by | HashiCorp | CNCF (open source) |
| Language | Sentinel | **Rego** |
| Enforcement levels | **advisory / soft mandatory / hard mandatory** | **advisory / mandatory** |
| Default level | hard mandatory | — |
| Cost estimation access | ✅ yes | — |
| Pre-written policies | from HashiCorp | community-driven |
| In one workspace | can use **both** frameworks together | can use both |

## Enforcement levels

| Level | Behaviour | Override? |
|---|---|---|
| **Advisory** (both) | fail → warning, run continues | n/a |
| **Soft mandatory** (Sentinel) / **Mandatory** (OPA) | fail → stops the run | ✅ with **Manage Policy Overrides** (logged) |
| **Hard mandatory** (Sentinel only) | fail → stops the run | ❌ cannot override — must fix config |

## When the exam picks each

- **Sentinel:** "HashiCorp policy language", "three enforcement levels", "hard mandatory", "cost estimation in policy".
- **OPA:** "Rego", "open source / CNCF", "two enforcement levels".

## Common traps

- **Sentinel has 3 levels, OPA has 2.** Only **hard mandatory (Sentinel)** can't be overridden.
- A **policy set is one framework only** (all Sentinel or all OPA) — you can't mix within a set, though a workspace can have both sets.
- Policies run **after plan, before apply**; they **can't** monitor or remediate already-deployed resources.
- Overriding requires the **Manage Policy Overrides** permission.

## Linked cards

- [Policy enforcement — Sentinel & OPA](../cards/08-hcp-terraform/05-policy-enforcement-sentinel-opa.md)

---

[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)
