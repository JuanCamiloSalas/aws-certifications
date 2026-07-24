[![](https://img.shields.io/badge/<_Block-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_>-7B42BC?style=for-the-badge)](./02-connecting-and-authenticating.md)

# What is HCP Terraform

> **Pitch (1 line):** a hosted platform (formerly Terraform Cloud) that **extends** Terraform — same code, but state, execution, collaboration and governance move to a managed service.

## 🎯 What the exam tests

- That HCP Terraform **doesn't change the language** — it changes **where and how** your code runs (same `.tf`, same `plan`/`apply`).
- What it adds over community Terraform: **remote state**, **remote execution**, **RBAC/collaboration**, **policy enforcement**, **cost estimation**, **private registry**, **audit logs**.
- **Execution modes** per workspace: **Remote** (default), **Local**, **Agent**.
- Roughly, the tiers (**Free** includes 500 managed resources · Essentials · Standard · Premium).

## 🧠 Core (non-obvious bits)

- **Same code, different runtime.** Your resources/modules/data sources are unchanged; you adopt at your own pace (start with just remote state).
- **Execution modes** (set **per workspace**):
  - **Remote** (default) — plan/apply run on HCP's infra; full features (Sentinel, cost estimation); output streamed to your CLI.
  - **Local** — plan/apply run on your machine; HCP only **stores/syncs state**; workspace variables are **NOT** evaluated.
  - **Agent** — runs on a **self-hosted agent** for private/on-prem infra HCP can't reach directly.
- **Free tier** includes **500 managed resources**; governance features (drift detection, policy enforcement, audit) start at **Standard/Premium**.
- Remote execution = **consistent environment**, no local dependencies.

## 🚩 Flags & values to memorize

- HCP **doesn't** change the Terraform **language** — only where/how it runs.
- Execution modes: **Remote (default)** · Local (state only) · Agent (self-hosted).
- Free tier = **500 managed resources**.

## ⚠️ Common traps

- In **Local** execution mode, HCP **does not evaluate workspace variables/variable sets** — only stores state.
- HCP Terraform ≠ the `terraform` CLI/binary; it's a SaaS platform around it. → [glosario](../../glosario.md)

---

[![](https://img.shields.io/badge/<_Block-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_>-7B42BC?style=for-the-badge)](./02-connecting-and-authenticating.md)
