[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./03-hcp-workspaces.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_>-7B42BC?style=for-the-badge)](./05-policy-enforcement-sentinel-opa.md)

# HCP Terraform Private Registry

> **Pitch (1 line):** share modules/providers **privately within your org** (plus curated public ones), published from VCS and versioned by Git tags — same experience as the public registry, different audience.

## 🎯 What the exam tests

- What it holds: **private modules/providers** (org-only) **+ curated public** modules approved for the org.
- That private modules are **published from VCS** and **versioned via Git tags** (`v1.0.0`, `v1.1.0`).
- The private **source address format**: `app.terraform.io/<ORG>/<NAME>/<PROVIDER>` (includes the hostname).
- That **publishing** needs the **Manage Private Registry** permission (all members can *view/use*).

## 🧠 Core (non-obvious bits)

- **Private vs public registry:** same UX and semantic versioning; private is scoped to **your org**, hosted inside HCP.
- **Source format includes the host:** `app.terraform.io/<ORG>/<NAME>/<PROVIDER>` — vs public `NAMESPACE/NAME/PROVIDER`. → [module sources & versioning](../05-modules/03-module-sources-and-versioning.md)
- **New versions auto-created from Git tags** — tag the repo, the registry picks it up.
- **Curated public modules**: you can add approved public modules/providers to the private registry so teams build from vetted components.
- View/use = any org member; **publish** = Manage Private Registry permission.

## 💻 Syntax / Example

```hcl
module "webapp" {
  source  = "app.terraform.io/my-org/webapp/gcp"   # host/ORG/NAME/PROVIDER
  version = "~> 1.2"
}
```

## 🚩 Flags & values to memorize

- Private source = **`app.terraform.io/<ORG>/<NAME>/<PROVIDER>`** (host prefix distinguishes it from public).
- Versions from **Git tags**; supports `version` constraints (`~>`) like the public registry.
- Publish needs **Manage Private Registry**; everyone can view/use.

## 🔄 Easily confused with

- **Public vs private registry** & source formats → [module source types](../../comparativas/module-source-types.md)

---

[![](https://img.shields.io/badge/<_Prev-7B42BC?style=for-the-badge)](./03-hcp-workspaces.md)
[![](https://img.shields.io/badge/Block-175074?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Next_>-7B42BC?style=for-the-badge)](./05-policy-enforcement-sentinel-opa.md)
