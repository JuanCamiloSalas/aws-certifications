[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./03-module-sources-and-versioning.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_block_>-7B42BC?style=for-the-badge)](../06-state/README.md)

# Module Inputs, Outputs & Variable Scope

> **Pitch (1 line):** each module has its **own isolated variable scope** — pass data **in** as inputs (`module` args → child `var.*`) and get data **out** via the child's `output`s (`module.<name>.<output>`).

## 🎯 What the exam tests

- That **every module has an isolated scope** — by default the root **cannot** see a child's variables/resources, and vice versa.
- **Inputs:** an argument in the `module` block sets the child's input **variable** (`env = var.environment` → child reads `var.env`).
- **Outputs:** a child value is only visible to the parent if the child **declares an `output`**; the parent reads it as **`module.<name>.<output>`**.
- That two modules can have same-named variables without colliding (separate scopes).

## 🧠 Core (non-obvious bits)

- **Isolation boundary:** a child can't reach into the root and the root can't reach into the child — data crosses **only** through declared inputs and outputs.
- **BUT providers are the exception:** a child module **inherits the parent's provider configurations automatically** — you do **not** pass providers like variables. (Exam trap: *providers are inherited, variables are not.*)
- **In (parent → child):** `module "net" { … env = var.environment }` — the child must declare `variable "env"`. → [variable block](../04-configuration/05-variable-block.md)
- **Out (child → parent):** child declares `output "subnet_id" { value = … }`; parent uses **`module.net.subnet_id`**. → [output block](../04-configuration/07-output-block.md)
- To bubble a child value **all the way up** (e.g. to CLI output), the **root** must re-declare its own `output` that references `module.net.subnet_id`.
- A child output **not declared** is simply invisible to the parent — no way to reach it.

## 💻 Syntax / Example

```hcl
# ROOT (parent) — passes input IN, reads output OUT
module "network" {
  source = "./modules/network"
  env    = var.environment          # input -> child's var.env
}

output "subnet_1" {
  value = module.network.subnet_1   # module.<name>.<output>
}
```

```hcl
# CHILD (./modules/network)
variable "env" {}                    # receives the input

output "subnet_1" {                  # exposes a value to the parent
  value = aws_subnet.this[0].id
}
```

## ⚠️ Common traps

- **Root has NO access to child data by default** — the child must `output` it, and you reference `module.<name>.<output>`.
- **Providers are inherited, variables are NOT** — don't assume a child sees the parent's variables (it doesn't), and don't try to "pass" the default provider (it's already inherited).
- Passing an argument the child didn't declare as a `variable` → error (inputs must match).
- A child `output` is visible to its **direct parent** only; to expose further up, re-output at each level.

## 🔄 Easily confused with

- **Input variable vs output** → [variable block](../04-configuration/05-variable-block.md) · [output block](../04-configuration/07-output-block.md)

---

[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./03-module-sources-and-versioning.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_block_>-7B42BC?style=for-the-badge)](../06-state/README.md)
