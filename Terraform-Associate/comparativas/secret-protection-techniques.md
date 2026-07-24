[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)

# Secret protection techniques: what protects what

> One-line summary: no single technique covers everything — **`sensitive`** hides logs, **env vars / external sources** keep secrets out of git, **ephemeral / write-only** keep them out of state. Stack them.

## Protection matrix

| Technique | Logs / output? | State file? | Git commits? | Best for |
|---|---|---|---|---|
| **Sensitive variables/outputs** (`sensitive = true`) | ✅ hides | ❌ still plaintext | ❌ no | masking output, marking intent |
| **Environment variables** (`TF_VAR_*`) | ⚠️ partial (pair w/ sensitive) | ❌ still in state | ✅ out of files/git | local dev, CI/CD |
| **External sources** (Secrets Mgr, Key Vault, Vault) | ⚠️ partial | 🔸 reduces (still lands unless ephemeral) | ✅ out of files/git | production, centralized mgmt |
| **Ephemeral / write-only** (1.10+) | ✅ redacted | ✅ **not written** | ✅ (no hardcode) | keeping secrets **out of state** |
| **Remote encrypted state** (obj 6) | — | 🔒 protects at rest | — | securing state that already holds secrets |

## When the exam picks each

- **`sensitive`**: "hide from `plan`/`apply` output / logs" — but note it's **not encryption** and **still in state**.
- **Env vars**: "keep secrets out of files/version control", "CI/CD", "no default value".
- **External sources**: "single source of truth", "centralized rotation", "read at runtime".
- **Ephemeral / write-only**: "never written to state or plan", "Terraform 1.10+", "in-memory only".

## Common traps

- **`sensitive` only masks output** — the value is still plaintext in state and still sent to the provider.
- **Env vars & external sources keep secrets out of git, NOT out of state** — the value still lands in `.tfstate`.
- **Only ephemeral values / write-only arguments keep a secret out of state.**
- Ephemeral / write-only require **Terraform 1.10+**; write-only args are **provider-defined**.

## Layered approach (they combine)

1. **Get** the secret in without files → env var or external source.
2. **Hide** it from logs → `sensitive = true`.
3. **Keep it out of state** → ephemeral value + write-only argument (Vault dynamic creds via an ephemeral resource).
4. **Protect the state** that still holds other secrets → remote, encrypted, access-controlled.

## Linked cards

- [Securing secrets (overview)](../cards/04-configuration/15-securing-secrets.md)
- [Sensitive variables & outputs](../cards/04-configuration/16-sensitive-variables-and-outputs.md)
- [Env vars & external sources](../cards/04-configuration/17-secrets-env-vars-and-external-sources.md)
- [Ephemeral values & write-only arguments](../cards/04-configuration/18-ephemeral-values-and-write-only-arguments.md)
- [Securing state files](../cards/06-state/08-securing-state-files.md)

---

[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)
