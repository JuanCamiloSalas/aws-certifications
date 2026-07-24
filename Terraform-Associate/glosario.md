[![](https://img.shields.io/badge/<_Terraform-7B42BC?style=for-the-badge)](./README.md)

# Glosario — términos y distinciones de examen

> Referencia única de Terraform. A diferencia del SAA (EN→ES + siglas), aquí pesan los **pares que se confunden** y la **terminología precisa** del lenguaje y la CLI. Búscalo con Ctrl+F durante el repaso.

## ⚠️ Pares que más se confunden

| A | B | Diferencia en una línea |
|---|---|---|
| **`count`** | **`for_each`** | `count` = índice numérico (lista, recrea al reordenar) · `for_each` = mapa/set con **claves estables** (no recrea al reordenar) |
| **`plan`** | **`apply`** | `plan` = previsualiza cambios, no toca infra · `apply` = ejecuta los cambios |
| **`validate`** | **`plan`** | `validate` = sintaxis/consistencia, **no** contacta providers · `plan` = sí consulta el estado real y los providers |
| **`refresh`** | **`plan`** | `refresh` = reconcilia state con la infra real (drift) · `plan` = calcula el diff hacia el estado deseado |
| **Backend local** | **Backend remoto** | local = `.tfstate` en disco (solo tú) · remoto (S3 con `use_lockfile` / HCP) = compartido + **locking** + cifrado. ⚠️ DynamoDB locking **deprecated** |
| **CLI workspaces** | **HCP workspaces** | CLI = varios states de **una misma config** (`terraform workspace`) · HCP = unidad de trabajo en la nube (state + variables + runs propios) |
| **Root module** | **Child module** | root = el directorio actual donde corres Terraform · child = el que invocas con un bloque `module` |
| **Providers (heredados)** | **Variables (pasadas)** | el child **hereda** las configs de provider del padre automáticamente · las variables **NO** se heredan: hay que pasarlas explícitas en el `module` block y declararlas en el child |
| **`map` / `list`** | **`object` / `tuple`** | map/list = nº variable de elementos, **mismo tipo** · object = **schema fijo** con atributos nombrados de tipos distintos · tuple = **longitud fija**, un tipo por posición |
| **`cloud` block** | **`backend` block** | HCP: el `cloud` block **reemplaza** al `backend` — **no** se pueden usar ambos |
| **Input variable** | **Local value** | variable = entrada parametrizable (desde fuera) · local = valor calculado **dentro** de la config, no se pasa desde fuera |
| **`local-exec`** | **`remote-exec`** | `local-exec` = comando en **tu** máquina · `remote-exec` = comando **en el recurso** creado (vía SSH/WinRM) |
| **`terraform`** | **HCP Terraform** | `terraform` = la CLI/binario open source · HCP Terraform = plataforma SaaS (remote runs, state, registry, Sentinel) |
| **`prevent_destroy`** | **`create_before_destroy`** | `prevent_destroy` = bloquea el destroy del recurso · `create_before_destroy` = crea el nuevo antes de borrar el viejo |
| **`moved`** | **`removed`** | `moved` = reubica/renombra en el state (sigue gestionado) · `removed` = lo saca del state (deja de gestionarse; `lifecycle { destroy = false }` mantiene la infra viva) |
| **CLI de state** (`state mv`/`rm`, `import`) | **Bloques declarativos** (`moved`/`removed`/`import`) | CLI = imperativo, fuera de la config, sin plan, arriesgado · bloques = en los `.tf`, versionados, revisables en PR, parte de plan/apply |
| **`check` block** | **precondition/postcondition** | `check` = top-level, corre al final de plan/apply, **solo avisa** (no bloquea) · pre/postcondition = dentro de `lifecycle`, **bloquean** la operación al fallar |
| **`terraform validate`** | **Custom conditions** | `validate` = sintaxis/consistencia (CLI, sin providers) · variable validation/pre-post/check = reglas de negocio sobre valores reales (en la config) |

## 🧱 Términos del lenguaje (HCL)

| Término | Qué es |
|---|---|
| **HCL** | HashiCorp Configuration Language — el lenguaje declarativo de Terraform |
| **Provider** | Plugin que habla con una API (AWS, Azure, random, local…) |
| **Resource** | Un objeto de infra gestionado por Terraform (bloque `resource`) |
| **Data source** | Lee info existente/externa sin gestionarla (bloque `data`) |
| **Module** | Conjunto reutilizable de archivos `.tf` (carpeta) invocable con `module`. Root = el directorio donde corres Terraform |
| **`module` block** | Llama a un módulo hijo: `module "name" { source … }`. `source` obligatorio · `version` opcional (**solo registry**) · el resto = inputs |
| **Module source** | De dónde viene el módulo: **registry** (`NS/NAME/PROVIDER`, versionable) · **Git** (`?ref=`) · **local** (`./path`, sin versión) |
| **`module.<name>.<output>`** | Cómo el root/parent lee un `output` de un módulo hijo (por defecto no tiene acceso a su interior) |
| **Variable** | Entrada parametrizable (`variable` block) |
| **Output** | Valor expuesto por una config/módulo (`output` block) |
| **Local** | Valor calculado dentro de la config. Se **define** en `locals {}` (plural) y se **referencia** con `local.<name>` (singular) |
| **Built-in function** | Función nativa `nombre(args)` (string/numeric/collection/type-conversion/CIDR). ⚠️ **No hay funciones definidas por el usuario**; probar en `terraform console` |
| **Meta-argument** | Argumento especial en cualquier recurso: `count` (`count.index`), `for_each` (`each.key`/`each.value`), `depends_on`, `lifecycle`, `provider` |
| **Expression** | Referencias, condicionales `? :`, `for`, splat `[*]`, funciones |
| **Interpolation** | `${...}` para insertar expresiones en strings (fuera de un string, la referencia va "pelada", sin `${}`) |
| **`moved` block** | Refactor declarativo: renombra/reubica un recurso en el state (`from`/`to`) sin recrearlo |
| **`removed` block** | Refactor declarativo: saca un recurso del state sin destruirlo (`from` + `lifecycle { destroy = false }`); hay que borrar el `resource` |
| **`import` block** | Refactor declarativo: adopta infra existente (`to` = dirección, `id` = ID del provider); escribir el `resource` antes o `-generate-config-out` |
| **`lifecycle` options** | `create_before_destroy` (cero downtime) · `prevent_destroy` (bloquea destroy; literal, no variable) · `ignore_changes = [attr]`/`all` (ignora drift) · `replace_triggered_by` (fuerza recreación) |
| **Custom conditions** | 4 mecanismos de validación: variable `validation`, `precondition`, `postcondition` (**bloquean**) y `check` block (top-level, corre al final, **solo warning**). Todos usan `condition` + `error_message` |
| **`sensitive = true`** | En `variable`/`output`: **enmascara** el valor en output/logs/errores. ⚠️ **NO** cifra ni lo saca del **state** (sigue en texto plano) — solo "hide from logs" |
| **Ephemeral value** | (TF **1.10+**) valor **solo en memoria**, nunca escrito a state ni plan; se recalcula cada run. Marcar con `ephemeral = true`. **No** válido en args de un `resource` normal |
| **Ephemeral resource** | Bloque nuevo (`ephemeral`, ≠ `data`/`resource`): fetch en runtime, en memoria, nunca persistido. Ideal para leer secretos/creds de Vault |
| **Write-only argument** | Arg de un recurso gestionado que acepta un valor **sin** persistirlo a state/plan (lo define el provider; suele llevar un `*_version`) |
| **Dynamic credentials** | Creds **short-lived** generadas on-demand (p.ej. Vault, TTL de minutos), auto-revocadas. Alternativa a las estáticas long-lived; con ephemeral resources no tocan el state |

## ⚙️ Términos de la CLI / estado

| Término | Qué es |
|---|---|
| **State (`.tfstate`)** | Mapa entre tu config y la infra real; guarda **secretos en texto plano** |
| **Backend** | Dónde vive el state y cómo se opera (local / remoto) |
| **State locking** | Bloqueo que impide ejecuciones concurrentes sobre el mismo state |
| **Drift** | Diferencia entre el state y la infra real (cambios fuera de Terraform) |
| **Refresh-only mode** | `plan -refresh-only` (preview) / `apply -refresh-only` (actualiza **state**, no infra) — para **aceptar** drift |
| **Workspace (CLI)** | Contenedor con nombre para un state aislado dentro de una misma config (`terraform workspace`) |
| **`terraform.tfstate.d/`** | Directorio donde viven los states de los workspaces no-`default` (`<name>/terraform.tfstate`) |
| **`use_lockfile`** | State locking nativo de S3 (`= true`); reemplaza el locking por DynamoDB (deprecated) |
| **Lock file (`.terraform.lock.hcl`)** | Fija versiones de providers; se commitea |
| **Plan file (`-out`)** | Plan guardado para aplicar exactamente lo previsto |
| **Provisioner** | Ejecuta scripts al crear/destruir un recurso (**último recurso**) |
| **Taint / `-replace`** | Marca un recurso para recrearlo (`-replace=ADDR` es lo moderno) |
| **Import** | Trae un recurso existente bajo gestión de Terraform. Moderno: **`import` block** (previsualizable en plan) · legacy: `terraform import <addr> <id>` (CLI, sin plan, modifica el state al instante) |
| **Sentinel** | Policy as code de HashiCorp (HCP Terraform). Corre **tras el plan, antes del apply**. 3 niveles: advisory / soft mandatory / hard mandatory (este último **no** se puede override) |
| **OPA** | Open Policy Agent (CNCF), lenguaje **Rego**. Alternativa a Sentinel; 2 niveles: advisory / mandatory |
| **Policy set** | Grupo de políticas de **un solo framework** (todo Sentinel o todo OPA); scope global/project/workspace |
| **`cloud` block** | En `terraform {}`, conecta con HCP: `organization` + `workspaces` (por `name`/`project` o por `tags`) |
| **`terraform login`** | Autentica la CLI con HCP; guarda el token en `~/.terraform.d/credentials.tfrc.json` (texto plano). Alternativa CI/CD: env `TF_TOKEN_app_terraform_io` |
| **Execution mode** (HCP) | Por workspace: **Remote** (default, corre en HCP) · **Local** (solo state, NO evalúa variables del workspace) · **Agent** (self-hosted, infra privada) |
| **HCP token types** | User (multi-org) · Team (corre plan/apply) · Organization (gestiona, **no** corre) · Audit (read-only) |

## 🔢 Operadores de versión

| Operador | Significado |
|---|---|
| `= 1.2.0` | Exactamente esa versión |
| `>= 1.2.0` | Esa o superior |
| `~> 1.2.0` | ≥1.2.0 y <1.3.0 (permite el último dígito) — "pessimistic constraint" |
| `~> 1.2` | ≥1.2 y <2.0 |
| `>= 1.1, < 2.0` | Rango compuesto |

---

[![](https://img.shields.io/badge/<_Terraform-7B42BC?style=for-the-badge)](./README.md)
