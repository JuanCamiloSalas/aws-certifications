[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)

# Module source types: registry vs Git vs local

> One-line summary: **registry** = versioned, shareable public/private modules (`version` allowed) · **Git** = a repo, pinned with `?ref=` · **local** = a path in the same project, no versioning.

## Decision table

| | Registry | Git repo | Local path |
|---|---|---|---|
| `source` example | `"terraform-aws-modules/vpc/aws"` | `"github.com/org/repo//sub?ref=v1.2.0"` | `"./modules/network"` |
| Format | `[HOST/]NAMESPACE/NAME/PROVIDER` | Git URL (`//` = subdir, `?ref=` = tag/branch/commit) | relative/absolute path |
| Versioning | ✅ `version` constraint (`~> 4.0`) | ❌ use **`?ref=`** | ❌ none (same repo) |
| Downloaded on `init` | ✅ into `.terraform/modules/` | ✅ into `.terraform/modules/` | ❌ read in place |
| Best for | shared/approved reusable modules | private/org modules not in a registry | modules that live inside this project |

## When the exam picks each

- **Registry:** "public/private module", "pin a version", "`terraform-aws-modules/...`", "Terraform Registry".
- **Git:** "module in a Git/GitHub repo", "specific branch/tag/commit", "`?ref=`".
- **Local:** "module in a subfolder", "relative path", "same configuration".

## Common traps

- **`version` is registry-only.** For Git use `?ref=`; local has no versioning. Putting `version` on Git/local is wrong.
- Registry `source` has a fixed shape `NAMESPACE/NAME/PROVIDER` (a private registry adds a `HOST/` prefix).
- Git `source` uses **`//`** to point at a subdirectory inside the repo.
- Adding any new module `source` requires **`terraform init`** (or `terraform get`) before plan/apply; local modules are read in place but a *new* one still needs init.

## Linked cards

- [Module sources & versioning](../cards/05-modules/03-module-sources-and-versioning.md)
- [Calling modules — the `module` block](../cards/05-modules/02-module-block.md)
- [Terraform block & version constraints](../cards/04-configuration/08-terraform-block.md) (`=` / `>=` / `~>`)

---

[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)
