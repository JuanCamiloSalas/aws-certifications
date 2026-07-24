[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)

# `moved` vs `removed` vs `import` (state refactoring)

> One-line summary: three **declarative** blocks to change how Terraform tracks infra **without the CLI**: **`moved`** = rename/relocate in state · **`removed`** = stop managing (keep it alive) · **`import`** = adopt existing infra. All run inside plan/apply and are deleted afterward.

## The three refactoring blocks

| | `moved` | `removed` | `import` |
|---|---|---|---|
| Purpose | Rename / relocate a resource in state | Stop managing a resource | Adopt existing infra into management |
| Real infra | untouched (no recreate) | **kept running** (`lifecycle { destroy = false }`) | untouched (just tracked) |
| Key args | `from` + `to` (addresses) | `from` + `lifecycle { destroy = false }` | `to` (address) + `id` (provider ID) |
| Also required | rename the `resource` block | **delete** the `resource` block | **write** the `resource` block first |
| Replaces (CLI) | `terraform state mv` | `terraform state rm` | `terraform import` |
| When | refactor names, reorg into modules | handoff to another team, manual control | bring manually-created resources under TF |

## Old CLI way vs new block way

| | CLI commands (`state mv`/`rm`, `import`) | Declarative blocks (`moved`/`removed`/`import`) |
|---|---|---|
| Where | run outside the config | live in your `.tf` files |
| Version control | ❌ not tracked | ✅ committed alongside infra |
| Review | hard (no PR diff) | ✅ reviewable in PRs |
| Workflow | direct state edit, **no plan** | part of **plan/apply**, testable first |
| Risk | one wrong command = permanent state change | previewed in `plan` before apply |

> **004 direction:** the CLI commands still work, but HashiCorp pushes the blocks. `terraform import` (CLI) is worth knowing exists; focus on the `import` **block**.

## Decision tree (what block do I need?)

- Renaming a resource? → **`moved`**
- Reorganizing resources into modules? → **`moved`**
- Stopping Terraform management (keep infra running)? → **`removed`**
- Handing off ownership to another team? → **`removed`**
- Splitting configurations apart? → **`removed`**
- Bringing existing/manually-created infra under management? → **`import`**

## Common refactoring workflow (all three)

1. **Write** the block(s) into your config (and adjust the matching `resource` block).
2. **`terraform plan`** — validate the change (no destroy/recreate for `moved`; no destroy for `removed`).
3. **`terraform apply`** — updates the **state**.
4. **Delete** the block once applied.

## Common traps

- **`moved` without renaming the resource block** — the block only rewrites state; rename the resource too.
- **Deleting a resource block with no `removed` block → it gets DESTROYED.** `removed { … destroy = false }` is what preserves it.
- **`import` to a non-existent resource block → error.** Write it first, or `terraform plan -generate-config-out=PATH`.
- CLI `terraform import` has **no plan step** and edits state immediately.

## Linked cards

- [`moved` block](../cards/06-state/06-moved-block.md)
- [`removed` block](../cards/06-state/07-removed-block.md)
- [`import` block](../cards/07-maintain/01-import-block.md)
- [Inspecting State](../cards/06-state/03-inspecting-state.md) (legacy `state mv` / `state rm`)

---

[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)
