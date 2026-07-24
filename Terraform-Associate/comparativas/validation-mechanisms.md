[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)

# Validation mechanisms: variable validation vs precondition vs postcondition vs check

> One-line summary: four defensive layers at different stages — **variable validation** (input), **precondition** (before create), **postcondition** (after create), **check** (last, non-blocking). The first three **stop** the run; `check` only **warns**.

## Decision table

| | Variable validation | Precondition | Postcondition | `check` block |
|---|---|---|---|---|
| Written in | `variable { validation {} }` | resource/data `lifecycle {}` | resource/data `lifecycle {}` | top-level `check {}` |
| Runs | before plan (on input) | before create/update | after create/update | last step of plan/apply |
| On failure | ❌ stops (error) | ❌ stops (error) | ❌ stops (error) | ⚠️ warning, **continues** |
| Checks | input format/range | assumptions before building | results after building | infra health as a whole |
| Nested? | inside `variable` | inside a resource/data | inside a resource/data | **standalone, top-level** |

All four use the same pair: **`condition`** (must be `true`) + **`error_message`**.

## Order of operations

`variable validation` → `precondition` → **[create/update resource]** → `postcondition` → `check` (last)

## When the exam picks each (decision tree)

- Validate **user input** before planning? → **variable validation**
- Verify **assumptions before** creating a resource? → **precondition**
- Verify **results after** creating a resource? → **postcondition**
- **Monitor** infra health **without blocking** operations? → **`check`**

## Common traps

- **`check` does not block** — a failed assertion is a warning and the apply proceeds. The other three abort the operation.
- **`check` is top-level**, not nested in a resource/variable.
- Not the same as **`terraform validate`** (CLI syntax/consistency check, no provider calls).
- `precondition`/`postcondition` live in `lifecycle`, but they're validation — distinct from the behavior options (`create_before_destroy`, etc.).

## Linked cards

- [Custom conditions & validation](../cards/04-configuration/14-custom-conditions-and-validation.md)
- [Variable block & types](../cards/04-configuration/05-variable-block.md)
- [`lifecycle` meta-argument](../cards/07-maintain/03-lifecycle.md)
- [`terraform validate`](../cards/03-core-workflow/07-terraform-validate.md)

---

[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)
