[![](https://img.shields.io/badge/<_Errors-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/<_Terraform-175074?style=for-the-badge)](../README.md)

# Terraform Practice Test 2 — Weak Points

**Result:** 80% (46/57). Passed (70% required). 2026-07-28 · Exam mode · 1h.

| Objective | Score | Priority |
|---|---|---|
| 8 · HCP Terraform | 3/5 (60%) | 🔴 |
| 5 · Terraform Modules | 5/8 (63%) | 🔴 |
| 3 · Core Terraform Workflow | 7/9 (78%) | 🟡 |
| 7 · Maintain Infrastructure | 3/4 (75%) | 🟡 |
| 2 · Terraform Fundamentals | 6/7 (86%) | 🟢 |
| 6 · State Management | 6/7 (86%) | 🟢 |
| 4 · Terraform Configuration | 12/13 (92%) | 🟢 |
| 1 · Infrastructure as Code (IaC) | 4/4 (100%) | ✅ |

> **Takeaway:** the weak spots **moved**. Test #1's problem area (IaC, 40%) is now **100%** — the conceptual gap closed. The two objectives now **below the 70% line** are **Modules** (source format, inputs-vs-outputs, `version`) and **HCP run semantics** (where CLI-driven runs execute, run triggers). 9 of 11 misses are **mechanics/definition recall**, not HCL you write.

**Legend:** ✅ **Correct:** right option · ✅/❌ **My answer:** what I actually selected · ❌ **Distractor:** wrong option I did not select.

---

# 🔴 5. Terraform Modules

## 5.1 A 3-part `source` string = the **public registry**, not a local path (Q30)
**Sample question:** *"Where is this module stored? `source = "terraform-aws-modules/transit-gateway/aws"`, `version = "3.0.3"`."*
- ✅ **Correct:** "the Terraform public registry" — `NAMESPACE/NAME/PROVIDER` (three slash-separated parts, no prefix) is the **registry address format**.
- ❌ **My answer:** "a local file under a directory named `terraform-aws-modules/transit-gateway/aws`" — **local sources must start with `./` or `../`**; a bare path is never local.
- ❌ **Distractor:** "a local code repository on your network" — a Git source needs a `git::` or `github.com/…` prefix; no prefix + 3 parts ⇒ registry.
> [!TIP]
> Decode `source` by its **shape**: `./` or `../` → local · `github.com/…` or `git::…` → Git · `[HOST/]NS/NAME/PROVIDER` → registry (default host `registry.terraform.io`). The `version` arg is only legal on the last one.
> 📇 [Card: Module sources & versioning](../cards/05-modules/03-module-sources-and-versioning.md) · [comparison: module-source-types](../comparativas/module-source-types.md)

## 5.2 Module-block arguments are **inputs into** the child (Q18)
**Sample question:** *"In `module "vpc"`, what do `name`, `cidr`, `azs` represent? (`name = var.vpc_name`, …)"*
- ✅ **Correct:** "module-specific **inputs passed into** the child module for resource creation" — they set the child's `variable` values from the caller.
- ❌ **My answer:** "the values will be obtained from values created **within the child** module" — **backwards**: values flow **caller → child**, not child → caller.
- ❌ **Distractor:** "where the variable **declarations** are created" — declarations live in the child's own `variable` blocks; here they are **assignments**. · "the **outputs** the child returns" — outputs go the *other* direction (child → caller, read as `module.vpc.<out>`).
> [!TIP]
> In a `module` block: **left side = the child's input variable name**, right side = value from the caller. Inputs go **in**; `output` blocks (read via `module.<name>.<output>`) come **out**. Don't swap the direction.
> 📇 [Card: Module inputs/outputs/scope](../cards/05-modules/04-module-inputs-outputs-scope.md) · [Module block](../cards/05-modules/02-module-block.md)

## 5.3 `version` on a registry module is **optional** (Q23)
**Sample question:** *"Using a module from the Terraform registry — is a `version` argument in the `module` block necessary?"*
- ✅ **Correct:** "No — `version` is **optional** but **recommended** for consistent/reproducible deployments." Omit it and Terraform pulls the **latest** available version each `init`.
- ❌ **My answer:** "Yes — `version` is **always required** for registry modules or Terraform errors" — false; it never errors on a missing `version`.
- ❌ **Distractor:** "No, Terraform auto-**pins to the first version** you init with, permanently" — false, it re-resolves to latest. · "Yes, but only for **public** registry modules" — false.
> [!TIP]
> `version` = optional-but-recommended (registry only). No pin ⇒ **latest each init** ⇒ surprise upgrades. Pin with `~> 4.0` etc. (Contrast: a **provider** version pin lives in `required_providers`, not the module block.)
> 📇 [Card: Module sources & versioning](../cards/05-modules/03-module-sources-and-versioning.md)

---

# 🔴 8. HCP Terraform

## 8.1 CLI-driven ≠ local — the run executes **on HCP infra** (Q21)
**Sample question:** *"HCP Terraform, CLI-driven workflow. You run `terraform plan` locally. Where does the plan execute?"*
- ✅ **Correct:** "on HCP Terraform infrastructure, with **results streamed back to your terminal**" — the default **Remote** execution mode runs plan/apply on HCP even when you *trigger* them from the CLI.
- ❌ **My answer:** "locally on your machine, then results uploaded to HCP for storage" — that's **Local** execution mode (HCP only stores/syncs state), not the default.
- ❌ **Distractor:** "on a self-hosted **agent** you must configure" — that's **Agent** mode (private networks). · "on your local machine with **no** HCP interaction" — no.
> [!TIP]
> Separate **workflow** (how a run is *triggered*: CLI / VCS / API) from **execution mode** (where it *runs*: **Remote** default / Local / Agent). "CLI-driven" only means you triggered it from the CLI — with Remote mode it still runs on HCP and streams output back.
> 📇 [Card: What is HCP Terraform (execution modes)](../cards/08-hcp-terraform/01-what-is-hcp-terraform.md) · [HCP workspaces (workflows)](../cards/08-hcp-terraform/03-hcp-workspaces.md)

## 8.2 A **run trigger** chains workspace → workspace (Q45)
**Sample question:** *"In HCP Terraform, what is the purpose of a run trigger?"*
- ✅ **Correct:** "to automatically **queue a new run in one workspace after another workspace applies successfully**" — orchestrates cross-workspace dependencies (e.g. network → app).
- ❌ **My answer:** "to force a workspace to always run from the newest commit of its VCS branch" — that's **VCS integration**, not a run trigger.
- ❌ **Distractor:** "to provide **temporary cloud credentials** with least privilege" — that's **dynamic provider credentials**. · "to require **human approval** before apply" — that's apply approval / manual runs / Sentinel.
> [!TIP]
> **Run trigger** = "when workspace A applies, kick off a run in workspace B." It's about **workspace-to-workspace ordering**, not VCS, not auth, not approvals.
> 📇 [Card: Run triggers](../cards/08-hcp-terraform/06-run-triggers.md)

---

# 🟡 3. Core Terraform Workflow

## 3.1 `terraform show` vs `terraform state show` (select three) (Q12)
**Sample question:** *"Difference between `terraform show` and `terraform state show`? (select three)"*
- ✅ **My answer:** "`terraform state show` **requires a resource address** to view that specific resource" — correct, selected.
- ✅ **Correct:** "`terraform show` displays the **entire state file** without additional arguments" — correct, but I **missed** it.
- ✅ **My answer:** "`terraform show` is useful for a **complete overview** of all managed infrastructure" — correct, selected.
- ❌ **My answer:** "`terraform state show` **automatically formats output as JSON** for machine parsing" — false; output is **human-readable**. Machine format is opt-in: `terraform show -json`.
- ❌ **Distractor:** "`terraform state show` can display **multiple resources separated by commas**" — false, one resource only. · "`terraform show` only displays resources with **pending changes**" — false, it shows all.
> [!TIP]
> `show` (no `state`) = **whole state / a plan file**, no args. `state show <addr>` = **one** resource, address required. Neither is JSON by default — add **`-json`**. `state list` = addresses only.
> 📇 [Card: Inspecting state](../cards/06-state/03-inspecting-state.md)

## 3.2 `terraform plan` is the **team-review** step (Q42)
**Sample question:** *"Aside from code reviews, which command lets teammates review each other's work before deployment?"*
- ✅ **Correct:** "`terraform plan`" — the execution plan **previews** create/update/delete so the team can catch mistakes before apply.
- ❌ **My answer:** "`terraform apply`" — apply **executes**; changes hit infra immediately, no pre-deployment review window.
- ❌ **Distractor:** "`terraform output`" — reads output values from state. · "`terraform validate`" — checks syntax/config validity, doesn't show the change set.
> [!TIP]
> Same trap as test #1: **`plan` = preview** (what would change) → the review artifact. **`apply` = execute**. When the stem says "review changes / before deployment" → **plan**.
> 📇 [Card: terraform plan](../cards/03-core-workflow/03-terraform-plan.md)

---

# 🟡 7. Maintain Infrastructure

## 7.1 To **decommission** specific resources: delete the blocks + `apply` (Q46)
**Sample question:** *"Project with 50 resources. Decommission only the Cloud SQL instance + its backup policy, keep everything else running. Best approach?"*
- ✅ **Correct:** "**Remove** the database and backup `resource` blocks from config, then `terraform apply`" — a resource that leaves the config gets **destroyed**; the other 48 are untouched.
- ❌ **My answer:** "Add two **`removed`** blocks pointing at the DB and the backup policy" — a `removed` block **stops managing without destroying** (keeps the real infra alive) — the **opposite** of decommissioning.
- ❌ **Distractor:** "`terraform destroy` then `apply` to recreate everything except the DB" — destroys all 50, disruptive. · "`terraform destroy -target=… -target=…`" — works but is a **risky imperative escape hatch**, not the recommended declarative flow.
> [!TIP]
> Want the real resource **gone** → delete its block + `apply`. Want it **kept but unmanaged** (hand-off / manual control) → `removed` block with `lifecycle { destroy = false }`. The question's verb ("decommission" = destroy) decides which.
> 📇 [Card: removed block](../cards/06-state/07-removed-block.md) · [comparison: moved/removed/import](../comparativas/refactoring-moved-removed-import.md)

---

# 🟢 2. Terraform Fundamentals

## 2.1 What establishes a **provider dependency** (select three) (Q26)
**Sample question:** *"How can a dependency on a provider be established? (select three)"*
- ✅ **Correct:** "Using a **`resource` or `data` block** that belongs to that provider" — referencing its resources creates the dependency. I **missed** this one.
- ✅ **My answer:** "Declaring a **`provider` block**, or adding `required_providers` version constraints" — correct, selected.
- ✅ **My answer:** "Having **existing resource instances** for that provider recorded in the current **state**" — correct, selected.
- ❌ **My answer:** "Existence of any **provider plugins found locally** in the working directory" — false; a downloaded plugin binary is not a *configuration* dependency.
> [!TIP]
> Terraform needs a provider when it's **referenced in config** (`resource`/`data`), **declared** (`provider` / `required_providers`), or **already in state**. Having the plugin cached in `.terraform/` is a *result* of `init`, not the *cause* of the dependency.
> 📇 [Card: Provider block](../cards/04-configuration/03-provider-block.md) · [required_providers in terraform block](../cards/04-configuration/08-terraform-block.md)

---

# 🟢 4. Terraform Configuration

## 4.1 Region → image-ID lookup ⇒ `map(string)`, not `object` (Q39)
**Sample question:** *"Look up a unique image ID per region by region name. Which variable type fits best?"*
- ✅ **Correct:** `type = map(string)` — dynamic key→value pairs (`region name → image ID`); look up with `var.image["us-east-1"]`. Any number of keys, all same type.
- ❌ **My answer:** `type = object({ region = string, image_id = string })` — a **fixed schema for a single** region/id pair, not a lookup table across many regions.
```hcl
variable "image" {
  type = map(string)          # { "us-east-1" = "ami-123", "eu-west-1" = "ami-456" }
}                             # var.image[var.region]  → the right AMI
```
> [!TIP]
> "**Look up** a value **by** a key, N entries" → **`map(...)`**. **`object({...})`** = one record with a **fixed set of named fields**. Keyword *"by the region name"* ⇒ map keyed on region.
> 📇 [Card: Variable block & types](../cards/04-configuration/05-variable-block.md)

---

## Summary — what to review

| Priority | Topic | Objective | Card |
|---|---|---|---|
| 🔴 | Module `source` shape: registry vs local (`./`) vs Git | 5 | [05-modules/03](../cards/05-modules/03-module-sources-and-versioning.md) |
| 🔴 | Module-block args = **inputs into** child (not outputs/declarations) | 5 | [05-modules/04](../cards/05-modules/04-module-inputs-outputs-scope.md) |
| 🔴 | `version` optional (defaults to latest), recommended to pin | 5 | [05-modules/03](../cards/05-modules/03-module-sources-and-versioning.md) |
| 🔴 | CLI-driven run executes on HCP (Remote mode) ≠ local | 8 | [08-hcp/01](../cards/08-hcp-terraform/01-what-is-hcp-terraform.md) |
| 🔴 | Run trigger = workspace→workspace chaining | 8 | [08-hcp/06](../cards/08-hcp-terraform/06-run-triggers.md) |
| 🟡 | `show` (all, no args) vs `state show <addr>`; JSON is opt-in | 3 | [06-state/03](../cards/06-state/03-inspecting-state.md) |
| 🟡 | `plan` = review preview / `apply` = execute | 3 | [03-core/03](../cards/03-core-workflow/03-terraform-plan.md) |
| 🟡 | Decommission = delete block+apply; `removed` = keep alive | 7 | [06-state/07](../cards/06-state/07-removed-block.md) |
| 🟢 | Provider dependency: config ref / declaration / state (not local plugin) | 2 | [04-config/03](../cards/04-configuration/03-provider-block.md) |
| 🟢 | `map(string)` for key-lookup vs `object` fixed schema | 4 | [04-config/05](../cards/04-configuration/05-variable-block.md) |

**Test pattern:** IaC (last test's 40%) recovered to **100%** — that review worked. The misses clustered in **Modules** (`source`/`version`/inputs — 3 of 8 wrong) and **HCP run semantics** (where runs execute, run triggers). Recurring meta-trap across both tests: **telling near-identical concepts apart** — inputs vs outputs, `plan` vs `apply`, delete-block vs `removed`, workflow vs execution mode. Fix Modules + HCP mechanics next.

---

[![](https://img.shields.io/badge/<_Errors-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/<_Terraform-175074?style=for-the-badge)](../README.md)
