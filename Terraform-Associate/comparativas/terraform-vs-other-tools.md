[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)

# Terraform vs other tools (IaC peers vs Config Management)

> Terraform is often confused with two families: other **IaC** tools (similar) and **configuration-management** tools (different but complementary).

## Decision table

| | Terraform | Other IaC (CloudFormation, Bicep, Pulumi) | Config Management (Ansible, Chef, Puppet) |
|---|---|---|---|
| Purpose | Provision & manage infrastructure | Provision & manage infrastructure | Configure *inside* existing machines |
| Scope | Cloud-agnostic (multi-provider) | Usually vendor-specific (CFN=AWS, Bicep=Azure); Pulumi is multi-cloud | Packages, files, services on a host |
| Model | Declarative, immutable, modular | Declarative | Often procedural/imperative; mutable |
| Typical jobs | Create VMs, networks, buckets | Same, per vendor | Install packages, manage files, orchestrate multi-step tasks |
| Relationship | — | **Competes** with Terraform | **Complements** Terraform (run after provisioning) |

## When the exam picks each

- **Terraform / IaC:** "provision infrastructure", "declarative", "immutable infrastructure", "modular", "multi-cloud".
- **Configuration management:** "install packages", "manage files/config on servers", "routine tasks", "orchestrate steps on a host".

## Common traps

- Ansible/Chef/Puppet are **complementary**, not competitors — Terraform builds the box, config mgmt sets it up inside.
- CloudFormation/Bicep are the direct IaC competitors, but they're **vendor-locked**; Terraform's edge is being platform-agnostic.
- Terraform favors **immutable infrastructure** (replace, don't mutate in place) — a distinguishing trait vs mutable config-mgmt.

## Linked cards

- [What is Terraform?](../cards/02-fundamentals/01-what-is-terraform.md)
- [Terraform vs other tools card](../cards/02-fundamentals/04-terraform-vs-other-tools.md)
</content>

---

[![](https://img.shields.io/badge/<_Comparativas-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/Terraform-175074?style=for-the-badge)](../README.md)
