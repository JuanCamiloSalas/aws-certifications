# S15 — Debugging / Troubleshooting Terraform (resumen de slides)

> Outline de la sección 15 del curso (Bryan Krausen). Tema: **logging para depurar** Terraform. 5 slides (~12 min).
> **No es transcripción** del PDF (`tf_004_15_debugging_terraform.pdf`, no versionado).
> **Sin cards nuevas** — la sección es **enteramente `TF_LOG`**, ya cubierto en [`03/09 environment-variables`](../cards/03-core-workflow/09-environment-variables.md). Se registra aquí para dejar constancia de que fue revisada (como los labs de S8).

## Lectures — por tema

- **Debugging Terraform:** por qué logs detallados (investigar comportamiento inesperado, bug reports a HashiCorp/providers, entender qué hace por dentro).
  - **`TF_LOG`** activa logging detallado. Niveles de más a menos verboso: **`TRACE` → `DEBUG` → `INFO` → `WARN` → `ERROR`** (+ formato **`JSON`**). `export TF_LOG=TRACE` (bash) / `$env:TF_LOG="TRACE"` (PowerShell).
  - **`TF_LOG_CORE`** / **`TF_LOG_PROVIDER`**: logging separado para el core de Terraform vs los providers (mismos niveles). Útil para dar logs al equipo correcto.
  - **`TF_LOG_PATH`**: persiste los logs a un archivo. Se **anexan**, el archivo se **crea solo** si no existe, y hay que tener `TF_LOG` (o CORE/PROVIDER) también seteado.

## Cards generadas desde esta sección

**Ninguna.** Todo el contenido ya vive en `03/09 environment-variables` (niveles de `TF_LOG`, `TF_LOG_PATH`, `TF_LOG_CORE`/`PROVIDER`). Se añadieron a esa card los 2 únicos detalles net-new de la S15: el formato **`JSON`** y que `TF_LOG_PATH` **anexa/autocrea** el archivo.

> Decisión (regla "el mapeo no es camisa de fuerza"): la S15 refuerza el bloque `03`, no genera cards propias. El mapeo la enviaba a `07-maintain`, pero su único contenido (`TF_LOG`) es un env var ya cardeado en `03`.
