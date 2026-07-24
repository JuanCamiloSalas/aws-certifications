[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)

# `count` vs `for_each`

> One-line summary: **`count`** = N copies by **numeric index** (list-backed, reorders/recreates) · **`for_each`** = one per **key** from a map/set (stable identity). Prefer `for_each` unless you just need N interchangeable copies or a 0/1 toggle.

## Decision table

| | `count` | `for_each` |
|---|---|---|
| Input | a **number** (`count = 3`) | a **map** or **set of strings** (`for_each = toset([...])`) |
| Instance identity | **index**: `res[0]`, `res[1]` | **key**: `res["dev"]`, `res["prod"]` |
| Iterator variable | `count.index` (0-based) | `each.key` / `each.value` |
| Add/remove in the middle | **reindexes** → later instances **recreated** | keys stable → **only that one** added/removed |
| List input | native | **not allowed** — wrap with `toset(list)` |
| Best for | N identical copies · conditional `cond ? 1 : 0` | distinct resources with meaningful, stable names |
| Splat all | `res[*].id` | (use a `for` expression / `values()`) |

## When the exam picks each

- **`count`:** "create N identical instances", "conditionally create a resource" (`count = var.enabled ? 1 : 0`), "how many", index-based naming.
- **`for_each`:** "one resource **per** item in this map/set", "avoid recreating the others when the list changes", "unique key/name per instance", "iterate a map".

## Common traps

- **Reindex on `count`:** removing a middle element shifts every later index → those get destroyed + recreated. If identity must survive edits, use **`for_each`**.
- **`for_each` needs a map/set, not a list** → `for_each = toset(var.names)`.
- **`for_each` keys must be known at plan time** — it can't depend on values only known after apply (computed IDs) → plan-time error; `count` tolerates this better.
- **Mutually exclusive:** a single resource can't declare both `count` and `for_each`.
- `count = 0` **destroys** the resource — it's removal, not a pause.

## Linked cards

- [`count` meta-argument](../cards/04-configuration/12-count-meta-argument.md)
- [`for_each` meta-argument](../cards/04-configuration/13-for_each-meta-argument.md)
- [Built-in functions](../cards/04-configuration/09-built-in-functions.md) (`toset()` to feed `for_each`)

---

[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)
