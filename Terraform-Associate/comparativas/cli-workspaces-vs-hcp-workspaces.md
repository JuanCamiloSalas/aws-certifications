[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)

# CLI workspaces vs HCP Terraform workspaces

> One-line summary: **CLI workspaces** = multiple state files for **one config + one backend** (lightweight env switch). **HCP workspaces** = a full cloud working directory (its own config, variables, state, VCS, runs). Same word, different thing — the exam exploits this.

## Decision table

| | CLI workspaces | HCP Terraform workspaces |
|---|---|---|
| What it is | Multiple **isolated state files** for one configuration | A complete **working directory in the cloud** |
| Code | Same code across all workspaces | Each can point to a different repo/config |
| State | One backend, many states (`terraform.tfstate.d/<name>/`) | Each workspace has its own state |
| Variables | Shared (from the same config) | Per-workspace variables & secrets |
| Extras | Just state isolation | VCS integration, run history, team collaboration, policies |
| Managed via | `terraform workspace <cmd>` (CLI) | HCP Terraform UI / API |
| Typical use | Quick dev/qa/prod split, same code | Real multi-team/multi-repo org setup |

## When the exam picks each

- **CLI workspaces:** "same configuration, multiple environments", "`terraform workspace`", "one backend", "on your local machine or remote backend".
- **HCP workspaces:** "HCP Terraform / Terraform Cloud", "VCS-connected", "per-workspace variables", "run history", "team collaboration".

## Common traps

- The two are **not the same feature** despite the shared name — read which product the question is about.
- CLI workspaces share the **same backend and credentials** → not a strong prod isolation boundary; HashiCorp suggests separate dirs/backends for hard separation.
- `default` CLI workspace always exists; its state stays in the root `terraform.tfstate`.

## Linked cards

- [Workspaces (CLI)](../cards/06-state/05-workspaces.md)
- HCP Terraform → block 08 (S16, pending)

---

[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)
