[![](https://img.shields.io/badge/<_Errors-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/<_Terraform-175074?style=for-the-badge)](../README.md)

# Terraform Practice Test 1 — Weak Points

**Result:** 85% (49/57). Passed (70% required). 2026-07-25 · Exam mode · 1h06m.

| Objective | Score | Priority |
|---|---|---|
| 1 · Infrastructure as Code (IaC) | 2/5 (40%) | 🔴 |
| 4 · Terraform Configuration | 9/12 (75%) | 🟡 |
| 5 · Terraform Modules | 5/6 (83%) | 🟢 |
| 8 · HCP Terraform | 5/6 (83%) | 🟢 |
| 2 · Core workflow · 3 · CLI · 6 · State · 7 · Maintain | 100% | ✅ |

> **Takeaway:** the "soft" concepts (IaC, patterns, T/F) were the weak spot, not the syntax. 5 of 8 misses are definition/pattern questions, not code.

**Legend:** **Correct:** right option · **My answer:** what I actually selected (✅ or ❌) · **Distractor:** wrong option I did not select.

---

# 🔴 1. Infrastructure as Code (IaC)

## 1.1 Multi-cloud benefit = **consistent workflow**, not the state (Q2)
**Sample question:** *"An org runs on AWS, Azure and GCP; the team struggles with different tools/interfaces/workflows. Which Terraform capability solves this?"*
- ✅ **Correct:** "Consistent workflow to manage multiple providers" — a single language/flow (`init→plan→apply`) over any provider is what standardizes the deployment.
- ❌ **My answer:** "State files provide centralized tracking" — state **tracks** resources, it does not **standardize** the process. Sounds right but doesn't address the stated problem (different tools/interfaces).
- ❌ **Distractor:** "Reusable modules" — they help, but the keyword in the stem is *standardize the deployment process* → workflow.
> [!TIP]
> When the stem says *standardize / consistent process across clouds* → answer **provider-agnostic workflow**. State = tracking; modules = reuse; neither one "standardizes the process".
> 📇 [Card: IaC benefits](../cards/01-iac/02-iac-benefits.md)

## 1.2 Immutable infrastructure **eliminates** config drift → True (Q37)
**Sample question:** *"T/F: the immutable pattern (replacing servers instead of modifying them in place) eliminates config drift because resources never change after provisioning."*
- ✅ **Correct:** True — immutable = treat infra as disposable; an update **replaces** the resource with a new version → there is no window for drift to accumulate. It also simplifies rollback (redeploy the previous version).
- ❌ **My answer:** False — I read "resources never change" as "it is impossible for them to ever change". Immutable means *not modified in place*, not *never replaced*.
> [!TIP]
> **Mutable** = patch in place → drift accumulates. **Immutable** = replace → **eliminates** drift. Terraform supports immutable naturally (declarative approach + `replace`).
> 📇 [Card: What is IaC](../cards/01-iac/01-what-is-iac.md)

## 1.3 Terraform shows the changes in the **Plan** stage (Q51)
**Sample question:** *"At which point of the workflow does Terraform show you what changes it will make before modifying infrastructure?"*
- ✅ **Correct:** Plan stage — it compares current state vs desired and shows the **execution plan** (create/update/delete) before touching anything.
- ❌ **My answer:** Apply stage — apply **executes** the plan; the "final confirmation" apply shows is the approval, not the **preview** of changes.
> [!TIP]
> **`plan` = preview** (what it would do). **`apply` = execute** (and ask for approval of the plan). The question "when does Terraform *show* changes" → **plan**, always.
> 📇 [Card: Declarative vs imperative](../cards/01-iac/03-declarative-vs-imperative.md) · workflow in [03-core-workflow](../cards/03-core-workflow/README.md)

---

# 🟡 4. Terraform Configuration

## 4.1 `depends_on` — when to use it (select two) (Q19)
**Sample question:** *"When should you use `depends_on` instead of implicit detection? (select two)"*
- ✅ **Correct:** "Dependencies outside Terraform's resource graph" — external systems that must be operational first.
- ✅ **My answer:** "B needs A operational but doesn't reference it" — hidden dependency, no `A.id` in the config.
- ❌ **My answer:** "Prevent parallel creation" — `depends_on` **only orders** create/destroy according to dependencies; it is **not** a parallelism control.
> [!TIP]
> `depends_on` = **hidden / external dependencies** Terraform can't infer from references. It does **not** control parallelism and is not a substitute for a direct reference (that's the implicit one, always preferred).
> 📇 [Card: depends_on](../cards/07-maintain/02-depends-on.md)

## 4.2 `for_each` over a list → must convert with `toset()` (Q27)
**Sample question:** *"Given `variable type = list(string)`, you want one `kubernetes_namespace` per entry with `for_each`. Which config handles the type conversion?"*
- ✅ **Correct:** `for_each = toset(var.namespaces)` — `for_each` requires a **set** or a **map**; `toset()` converts the list → set. (`tomap()` would also work for a map.)
- ❌ **My answer:** `for_each = var.namespaces` (list directly) → **error**: `for_each` does not accept a `list`.
- ❌ **Distractor:** `tolist(...)` — still a list, `for_each` won't accept it.
```hcl
resource "kubernetes_namespace" "ns" {
  for_each = toset(var.namespaces)   # list(string) -> set; each.key == each.value
  metadata { name = each.value }
}
```
> [!TIP]
> `for_each` **never** accepts a `list` directly → wrap it in **`toset()`** (set) or `tomap()` (map). With a set: `each.key == each.value`.
> 📇 [Card: for_each](../cards/04-configuration/13-for_each-meta-argument.md) · [comparison: count vs for_each](../comparativas/count-vs-for-each.md)

## 4.3 Securing the state (select two) (Q49)
**Sample question:** *"Which statements about securing state files are true? (select two)"*
- ✅ **Correct:** "Remote backends are recommended over local" for production — security, access, collaboration.
- ✅ **My answer:** "State locking prevents concurrent modifications" that would corrupt the state.
- ❌ **My answer:** "Write-only arguments prevent ALL secrets from being stored in state" — a write-only argument keeps **that specific** secret out of state, **not every** secret in it. "All" is the trap.
- ❌ **Distractor:** "Encrypted state removes the need for the `sensitive` flag" — false; encryption (at rest) and marking `sensitive` (accidental exposure in output/logs) are distinct, complementary layers.
> [!TIP]
> State security = **remote backend + locking + encryption**. `sensitive` and encryption **don't replace each other**. Write-only/ephemeral keep **one** secret out of state, they don't make it "secret-free".
> 📇 [Card: Securing state files](../cards/06-state/08-securing-state-files.md) · [Ephemeral & write-only](../cards/04-configuration/18-ephemeral-values-and-write-only-arguments.md) · [comparison: secret-protection](../comparativas/secret-protection-techniques.md)

---

# 🟢 5. Terraform Modules

## 5.1 GitHub as a module source: valid shorthand (Q44)
**Sample question:** *"T/F: you can use a GitHub repo as a source with `source = \"github.com/hashicorp/example\"`."*
- ✅ **Correct:** True — Terraform detects GitHub via the **shorthand** `github.com/...` (no protocol prefix), or with the explicit git prefix `git::https://github.com/hashicorp/example.git`.
- ❌ **My answer:** False — assumes the protocol prefix is always required. The `github.com/...` shorthand is a recognized special case.
> [!TIP]
> Valid Git sources: `github.com/org/repo` (shorthand), `git::https://...`, `git::ssh://...`. Plain HTTPS works too. Pin a version with `?ref=v1.2.0`, or `//subdir` for a subfolder.
> 📇 [Card: Module sources & versioning](../cards/05-modules/03-module-sources-and-versioning.md) · [comparison: module-source-types](../comparativas/module-source-types.md)

---

# 🟢 8. HCP Terraform

## 8.1 The three run workflows: VCS / API / CLI (Q21)
**Sample question:** *"Which workflow types exist for managing runs in HCP Terraform? (select three)"*
- ✅ **My answer:** VCS-driven — integrates Git → store/track/version.
- ✅ **My answer:** API-driven — programmatic interaction via API calls → automation.
- ✅ **Correct:** CLI-driven — the third one, which I failed to select.
- ❌ **My answer:** Agent-driven — "Agent" is an **execution mode** (Remote / Local / **Agent** for isolated networks), **not** one of the three *run workflows*. Close distractor.
> [!TIP]
> **3 run workflows** = **VCS · CLI · API**. **3 execution modes** = **Remote · Local · Agent**. Don't confuse *how the run is triggered* (workflow) with *where it executes* (mode). "SSH-driven" and "Agent-driven" are decoys.
> 📇 [Card: HCP workspaces (workflows)](../cards/08-hcp-terraform/03-hcp-workspaces.md)

---

## Summary — what to review

| Priority | Topic | Objective | Card |
|---|---|---|---|
| 🔴 | IaC benefits (consistent workflow vs state/modules) | 1 | [01-iac/02](../cards/01-iac/02-iac-benefits.md) |
| 🔴 | Mutable vs immutable (drift) | 1 | [01-iac/01](../cards/01-iac/01-what-is-iac.md) |
| 🔴 | plan = preview / apply = execute | 1 | [01-iac/03](../cards/01-iac/03-declarative-vs-imperative.md) |
| 🟡 | `depends_on` (does not control parallelism) | 4 | [07-maintain/02](../cards/07-maintain/02-depends-on.md) |
| 🟡 | `for_each` list → `toset()` | 4 | [04-config/13](../cards/04-configuration/13-for_each-meta-argument.md) |
| 🟡 | Securing state (remote+lock; write-only ≠ "all secrets") | 4 | [06-state/08](../cards/06-state/08-securing-state-files.md) |
| 🟢 | GitHub module source shorthand | 5 | [05-modules/03](../cards/05-modules/03-module-sources-and-versioning.md) |
| 🟢 | HCP: 3 workflows (VCS/CLI/API) vs 3 modes (Remote/Local/Agent) | 8 | [08-hcp/03](../cards/08-hcp-terraform/03-hcp-workspaces.md) |

**Test pattern:** syntax (state, core workflow, maintain) scored 100%; the misses were **conceptual/definitional** (IaC at 40%, T/F, "select N" with nuance distractors). Focus review on **IaC benefits/patterns** and on **telling close concepts apart** (workflow vs mode, encryption vs sensitive, depends_on vs parallelism).

---

[![](https://img.shields.io/badge/<_Errors-7B42BC?style=for-the-badge)](./README.md)
[![](https://img.shields.io/badge/<_Terraform-175074?style=for-the-badge)](../README.md)
