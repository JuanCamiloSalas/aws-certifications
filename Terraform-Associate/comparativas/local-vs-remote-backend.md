[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)

# Local vs Remote backend

> One-line summary: **local** = `terraform.tfstate` on your disk (solo/learning); **remote** = shared state (S3/azurerm/gcs/HCP) with locking + encryption (teams/prod).

## Decision table

| | Local backend (default) | Remote backend |
|---|---|---|
| Where state lives | `terraform.tfstate` in the working dir | S3, Azure Blob (`azurerm`), GCS, HCP Terraform |
| Collaboration | ❌ one person | ✅ shared across the team/CI |
| State locking | ❌ none | ✅ (automatic on most; S3 via `use_lockfile = true`) |
| Encryption at rest | ❌ plain text on disk | ✅ backend-managed |
| Versioning / backups | ❌ manual | ✅ automatic (e.g. S3 bucket versioning) |
| Setup / cost | zero | initial setup, may incur cost |
| Good for | learning, experiments, solo | **all team projects & production** |

## When the exam picks each

- **Local:** "default", "solo project", "on your machine", "learning".
- **Remote:** "team", "collaboration", "locking", "prevent concurrent runs", "encrypted", "S3 / Azure Storage / HCP".

## Common traps

- **Locking is a *remote* feature** — local state has none; concurrent applies can corrupt it.
- Config goes in the **`terraform {}` block**, not a `provider` block; re-run **`terraform init`** after adding/changing it (offers to migrate state).
- **S3 locking = `use_lockfile = true`** now; **DynamoDB table locking is deprecated** (older docs/dumps still show it).
- Backend block takes **literal values only** — supply dynamic values with `terraform init -backend-config=...`.
- Never put credentials in the backend block — use roles / env vars.

## Linked cards

- [Terraform State](../cards/06-state/01-terraform-state.md)
- [Backends & State Storage](../cards/06-state/02-backends-and-state-storage.md)
- [`terraform init`](../cards/03-core-workflow/02-terraform-init.md) (`-migrate-state` / `-reconfigure`)

---

[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)
