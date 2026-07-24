[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./08-terraform-cli.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_block_>-7B42BC?style=for-the-badge)](../04-configuration/README.md)

# Environment variables (`TF_VAR_*`, `TF_LOG`, provider creds)

> **Pitch (1 line):** environment variables feed input values, credentials and debug settings to Terraform **without hardcoding** them in `.tf` files.

## 🎯 What the exam tests

- **`TF_VAR_<name>`** sets the input variable `<name>` — and where env vars sit in variable **precedence** (low).
- **`TF_LOG`** for debug logging, its levels, and **`TF_LOG_PATH`** to persist logs.
- That provider credentials (e.g. `AWS_ACCESS_KEY_ID`) belong in env vars, not in committed `.tf`.

## 🧠 Core (non-obvious bits)

- **`TF_VAR_<name>`** → sets input variable `name` (e.g. `TF_VAR_region=us-east-1` fills `var.region`). The part after the prefix must match the variable name **exactly** (case-sensitive).
- **Precedence:** env vars are **near the bottom** — beaten by `terraform.tfvars`, `*.auto.tfvars`, and `-var`/`-var-file`. A CLI `-var` always wins over `TF_VAR_*`.
- **`TF_LOG`** = verbosity: `TRACE` (most detail) → `DEBUG` → `INFO` → `WARN` → `ERROR` (also `JSON` for machine-readable output). **`TF_LOG_PATH`** writes the log to a file (**appended**, auto-created; needs `TF_LOG`/`TF_LOG_CORE`/`TF_LOG_PROVIDER` also set). `TF_LOG_CORE` / `TF_LOG_PROVIDER` target just one side. _(S15 debugging is fully covered by this card.)_
- **Provider creds** via env (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, …) keep secrets out of `.tf` and out of version control.
- **`TF_CLI_ARGS`** (or `TF_CLI_ARGS_<name>`) injects default flags into every / one command; **`TF_INPUT=0`** disables interactive prompts (useful in CI).

## 💻 Syntax / Example

```bash
export TF_VAR_region="us-east-1"       # sets var.region (low precedence)
export TF_LOG=DEBUG                    # enable debug logging
export TF_LOG_PATH=./terraform.log     # persist logs to a file
export AWS_ACCESS_KEY_ID="AKIA..."     # provider creds — never in .tf
export AWS_SECRET_ACCESS_KEY="..."
```

```powershell
$Env:TF_LOG = "DEBUG"                  # PowerShell   (cmd: setx TF_LOG "DEBUG")
```

## 🚩 Flags & values to memorize

- **`TF_VAR_<name>`** → input variable; **low precedence** (`-var` / `*.tfvars` override it).
- **`TF_LOG`** levels: `TRACE | DEBUG | INFO | WARN | ERROR` (TRACE = most detail); **`TF_LOG_PATH`** = log file.
- **`TF_INPUT=0`** = no prompts; **`TF_CLI_ARGS`** = default CLI args.

## ⚠️ Common traps

- `TF_VAR_*` is **overridden** by `*.tfvars` and `-var` — don't assume the environment wins.
- The name is **case-sensitive**: `TF_VAR_Region` does **not** fill `var.region`.
- Env vars live only in the shell/run — they're never stored in state or config, so each run needs them present.

## 🔄 Easily confused with

- The full **variable precedence ladder** (defaults → `TF_VAR_*` → `terraform.tfvars` → `*.auto.tfvars` → `-var`/`-var-file`) is a block 04 (configuration) topic — this card only covers the env-var rung.
- Using `TF_VAR_*` / provider-cred env vars to **keep secrets out of files/git** → [secrets via env vars & external sources](../04-configuration/17-secrets-env-vars-and-external-sources.md) (note: env vars don't keep secrets out of **state**).

---

[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./08-terraform-cli.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_block_>-7B42BC?style=for-the-badge)](../04-configuration/README.md)
