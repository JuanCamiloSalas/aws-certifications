[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./01-what-is-hcp-terraform.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_>-7B42BC?style=for-the-badge)](./03-hcp-workspaces.md)

# Connecting & Authenticating to HCP Terraform

> **Pitch (1 line):** the **`cloud`** block points Terraform at your org/workspaces; **`terraform login`** (or the `TF_TOKEN_*` env var) authenticates the CLI.

## 🎯 What the exam tests

- The **`cloud`** block: `organization` + a `workspaces` block targeting by **`name`/`project`** or by **`tags`**.
- **`terraform login`** stores an API token (plaintext) in `~/.terraform.d/credentials.tfrc.json`.
- The env-var alternative **`TF_TOKEN_app_terraform_io`** for CI/CD (no interactive browser).
- The **four API token types** and what each can do.

## 🧠 Core (non-obvious bits)

- **`cloud` block** lives in `terraform {}`; `workspaces` either names a specific workspace/project or matches by **tags** (multiple workspaces).
- **The `cloud` block REPLACES the `backend` block — you can't use both.** If you're on HCP Terraform, use `cloud`, not `backend`. (Common exam trap.)
- **`terraform login`** → browser flow → token saved **in plain text** in `credentials.tfrc.json`.
- **`TF_TOKEN_app_terraform_io=<token>`** — set the token via env var where interactive login isn't possible (pipelines).
- **API token types:**
  - **User** — same access as your account; the **only** type that spans **multiple orgs**.
  - **Team** — for automation/CI; **can run** plan/apply on the team's workspaces.
  - **Organization** — manage teams/membership/workspaces; **cannot** run plans/applies.
  - **Audit** — **read-only**, for the audit-trail API.

## 💻 Syntax / Example

```hcl
terraform {
  cloud {
    organization = "my-org"
    workspaces {
      name = "app-prod"          # or: project = "prod"
      # tags = ["prod", "app"]   # alternative: match by tags
    }
  }
}
```

```bash
terraform login                              # browser → token in credentials.tfrc.json
export TF_TOKEN_app_terraform_io="<token>"   # CI/CD alternative
```

## 🚩 Flags & values to memorize

- `cloud` block = `organization` + `workspaces` (`name`/`project` **or** `tags`).
- Token file: `~/.terraform.d/credentials.tfrc.json` (plaintext).
- **Organization token can't run plan/apply**; **User token is the only multi-org** one; **Audit** is read-only.

## ⚠️ Common traps

- **`cloud` and `backend` are mutually exclusive** — the `cloud` block replaces `backend`; using both is an error.
- **Organization tokens can NOT run plans/applies** — they're for managing teams/workspaces. Use a **Team** (or User) token to run.
- Token is stored **in plain text** — treat `credentials.tfrc.json` as a secret.

---

[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./01-what-is-hcp-terraform.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_>-7B42BC?style=for-the-badge)](./03-hcp-workspaces.md)
