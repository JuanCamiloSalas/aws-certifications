[![](https://img.shields.io/badge/<_Terraform-7B42BC?style=for-the-badge)](../README.md)

# 📊 Comparisons (`comparativas/`)

> Side-by-side tables for the distinctions the exam keeps testing on purpose. Open these when a card says "easily confused with…". In Terraform these are almost as important as the cards — the exam lives on `count` vs `for_each`, `plan` vs `apply` vs `refresh`, CLI vs HCP workspaces, etc.

## How this folder works

- **Not in the card flow.** Cards link here when relevant; you don't walk through it linearly.
- **One topic per file.** A comparison stays focused on a single decision.
- **Table-first.** Open with the decision table; details after.
- **Built as concepts appear**, not all in advance. Real confusion (from labs or practice tests) drives what gets written.

## File naming

`<topic-or-concepts>.md`, lowercase, dash-separated. Likely candidates for this exam:

- `count-vs-for-each.md`
- `plan-vs-apply-vs-refresh.md`
- `local-vs-remote-backend.md`
- `cli-workspaces-vs-hcp-workspaces.md`
- `module-source-types.md`
- `local-exec-vs-remote-exec.md`
- `variables-vs-locals-vs-outputs.md`
- `validate-vs-plan.md`

## Suggested skeleton for a comparison file

```markdown
# X vs Y vs Z

> One-line summary of when each wins.

## Decision table

| | X | Y | Z |
|---|---|---|---|
| What it does | ... | ... | ... |
| When to use | ... | ... | ... |
| Gotcha | ... | ... | ... |

## When the exam picks each
- **X:** trigger / phrasing
- **Y:** trigger / phrasing

## Common traps
- ...

## Linked cards
- [X card](../cards/NN-block/...)
- [Y card](../cards/NN-block/...)
```

## Index of comparisons

| File | Topic | Block |
|---|---|---|
| [terraform-vs-other-tools.md](./terraform-vs-other-tools.md) | Terraform vs IaC peers vs config management | 02 |
| [count-vs-for-each.md](./count-vs-for-each.md) | Multiple instances: numeric index vs stable keys | 04 |
| [module-source-types.md](./module-source-types.md) | Module `source`: registry vs Git vs local | 05 |
| [validation-mechanisms.md](./validation-mechanisms.md) | variable validation vs pre/postcondition vs check | 04 |
| [secret-protection-techniques.md](./secret-protection-techniques.md) | sensitive vs env vars vs external vs ephemeral (logs/state/git) | 04/06 |
| [refactoring-moved-removed-import.md](./refactoring-moved-removed-import.md) | State refactoring blocks + CLI vs declarative | 06/07 |
| [local-vs-remote-backend.md](./local-vs-remote-backend.md) | Where state lives: local disk vs remote (S3/azurerm/HCP) | 06 |
| [cli-workspaces-vs-hcp-workspaces.md](./cli-workspaces-vs-hcp-workspaces.md) | Same word, two features: CLI workspaces vs HCP workspaces | 06/08 |
| [sentinel-vs-opa.md](./sentinel-vs-opa.md) | HCP policy frameworks: Sentinel vs OPA + enforcement levels | 08 |

---

[![](https://img.shields.io/badge/<_Terraform-7B42BC?style=for-the-badge)](../README.md)
