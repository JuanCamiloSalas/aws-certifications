[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./02-connecting-and-authenticating.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_>-7B42BC?style=for-the-badge)](./04-private-registry.md)

# HCP Terraform Workspaces (vs CLI workspaces)

> **Pitch (1 line):** an HCP workspace is a **complete environment** (config + state + variables + settings + run history) — a very different thing from a CLI workspace, which is just multiple state files for one config.

## 🎯 What the exam tests

- The trap: **HCP workspaces ≠ CLI workspaces**. Same word, fundamentally different concept.
- What an HCP workspace holds: **configuration + state (with version history) + variables + run history + execution settings + team access**.
- The **three workflow types**: **CLI-driven**, **VCS-driven**, **API-driven**.

## 🧠 Core (non-obvious bits)

- **CLI workspace** = lightweight state isolation — several state files for **one** config, managed by `terraform workspace`. → [CLI workspaces card](../06-state/05-workspaces.md)
- **HCP workspace** = a full working environment managed in HCP (UI/CLI/API): config, state + rollback, variables (TF + env, with sensitive), run history, execution mode, permissions.
- **Workflow types:**
  - **CLI-driven** — trigger runs from the Terraform CLI.
  - **VCS-driven** — runs triggered by commits/PRs in a connected repo (traceability). A **push to a branch/PR** triggers a **speculative plan** (plan-only, **never applies**) for review; merging to the main branch runs a plan that then **waits for approval** before apply.
  - **API-driven** — runs triggered via the HCP API (custom pipelines).
- Execution mode (Remote/Local/Agent) is set **per workspace**. → [what is HCP Terraform](./01-what-is-hcp-terraform.md)

## ⚠️ Common traps

- "Workspace" on the exam: decide from context — **CLI** (multiple states, one config) vs **HCP** (full environment). They are **not** the same feature.
- An HCP workspace typically maps to **one** Terraform configuration (unlike CLI workspaces, which share one config across many states).

## 🔄 Easily confused with

- → [CLI workspaces vs HCP workspaces](../../comparativas/cli-workspaces-vs-hcp-workspaces.md)

---

[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./02-connecting-and-authenticating.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_>-7B42BC?style=for-the-badge)](./04-private-registry.md)
