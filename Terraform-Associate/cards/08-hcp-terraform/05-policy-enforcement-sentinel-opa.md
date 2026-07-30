[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./04-private-registry.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_>-7B42BC?style=for-the-badge)](./06-run-triggers.md)

# Policy Enforcement — Sentinel & OPA

> **Pitch (1 line):** policy-as-code that runs **after the plan, before the apply** — **Sentinel** (HashiCorp) or **OPA** (open source, Rego) — to block non-compliant infrastructure before it's created.

## 🎯 What the exam tests

- **When** policies run: **after the plan phase, before apply** (block bad changes before creation). If the plan fails, policies never run.
- **Sentinel vs OPA**: two frameworks, same goal — Sentinel (HashiCorp, Sentinel language) vs **OPA** (CNCF, **Rego** language).
- **Enforcement levels**: Sentinel has **three** (advisory / soft mandatory / hard mandatory); OPA has **two** (advisory / mandatory).
- That overriding a failed (overridable) policy needs the **Manage Policy Overrides** permission.

## 🧠 Core (non-obvious bits)

- **Sentinel enforcement levels:**
  - **Advisory** — fail = warning, run continues.
  - **Soft mandatory** — fail = stops the run, but can be **overridden** (needs Manage Policy Overrides; overrides are logged).
  - **Hard mandatory** — fail = stops the run, **cannot** be overridden (default for Sentinel).
- **OPA enforcement levels:** **Advisory** (warn) and **Mandatory** (stop, overridable).
- **Policy sets** group policies; each set is **one framework only** (all Sentinel or all OPA); scope **global / project / workspace**; you can exclude workspaces.
- Sentinel policy checks can read **cost estimation** data.
- **What policies CAN'T do:** monitor after deploy, remove existing violating resources, validate attribute values at runtime, analyze runtime behavior. (They gate the plan, not live infra.)

## 🚩 Flags & values to memorize

- Run **after plan, before apply**; plan failure → policies skipped.
- **Sentinel = 3 levels** (advisory / soft / hard mandatory); **OPA = 2** (advisory / mandatory).
- OPA language = **Rego**; Sentinel language = **Sentinel**.
- Override → **Manage Policy Overrides** permission (soft mandatory / OPA mandatory only).
- Policy set = **one framework**; scope global/project/workspace.

## ⚠️ Common traps

- **Hard mandatory (Sentinel) can't be overridden**; soft mandatory can. OPA's mandatory is overridable.
- Policies **gate the plan** — they don't monitor or remediate **already-deployed** resources.
- A policy **set** can't mix Sentinel and OPA.

## 🔄 Easily confused with

- → [Sentinel vs OPA](../../comparativas/sentinel-vs-opa.md)

---

[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./04-private-registry.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_>-7B42BC?style=for-the-badge)](./06-run-triggers.md)
