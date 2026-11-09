# Registro de Cambios, Control de Versiones y Procedimiento de Reversión

> **Navegación:** [[Índice de información]] | [[Transcripción original]] | [[Aclaración adicional del entrevistado]] | [[Inventario documental]] | [[Matriz de inconsistencias]] | [[Informe final de revisión]]

---

## 1. Política de Cambios Seguros y Criterios de Edición

Para proteger la integridad de los datos documentales del proyecto de Arquitectura Empresarial, se definieron las siguientes políticas de trabajo:

### A. Archivos Intocables (Solo Lectura Permanente)
- **`00 - Fuente/Transcripción original.md`**: Prohibida taxativamente cualquier edición, movimiento, eliminación o cambio de nombre. Debe coincidir siempre con el hash SHA256 original:
  `AB6FC60641C5497835BE2ED188FB9CE857C137AD4656DCEE67C0A634547B1B26`
- **`trascrito.text`**: Archivo de texto plano original conservado como respaldo histórico. Hash SHA256:
  `3864A5C82BC05EBA0FB9134F676FC9BC3EC037E6A89F9EFDB3F281ACFE2B2C59`

### B. Criterios de Edición Directa
Se procedió a la edición directa en los siguientes casos:
1. Corrección de erratas materiales o tipográficas evidentes (ej. «kDistribución»).
2. Alineación del contenido con las afirmaciones textuales de [[Transcripción original]] y la [[Aclaración adicional del entrevistado]].
3. Supresión de contradicciones manifiestas con la fuente (ej. exclusión indebida de la entrega al cliente final).
4. Eliminación de problemas o supuestos inventados sin sustento (ej. el supuesto cuello de botella PR-06).
5. Corrección de enlaces internos rotos en Obsidian para restaurar el grafo de navegación.

### C. Criterios para Proponer Cambios (Sin Imponerlos)
Se documentan como propuestas o dudas abiertas (❓), requiriendo validación del usuario o del entrevistado:
1. Decisiones metodológicas de alcance del equipo académico no respaldadas por la fuente (ej. cláusula «H-16» que restringe el prototipo a B2B troncal).
2. Definición formal de términos no precisados (ej. alcance exacto de «distribuidoras» en la aclaración `[AC]`).
3. Asignación definitiva de cuáles 3 sistemas son propios y cuáles 4 son de proveedores externos.
4. Unificación de los términos fonéticos *NSDG* y *ENSDY*.

---

## 2. Puntos de Restauración y Respaldo Generados

Antes de iniciar cualquier modificación se generaron dos mecanismos redundantes de respaldo íntegro:

### Respaldo 1: Archivo ZIP Comprimido
- **Ubicación:** `c:\Users\joses\Obsidian\informe\ARQ-EMP\_respaldos\Entrevista_antes_de_revision_2026-10-10.zip`
- **Contenido:** Copia exacta de toda la estructura de la carpeta `Entrevista` en su estado previo a la revisión.
- **Tamaño:** ~52,292 bytes.

### Respaldo 2: Snapshot Completo en Scratchpad
- **Ubicación:** `C:\Users\joses\.gemini\antigravity-ide\brain\7c43df94-b2aa-4361-853f-8fd32165a3b0\scratch\snapshot_antes\`
- **Manifiesto de hashes SHA256:** Registrado en `scratch\manifiesto_antes.tsv`.

---

## 3. Repositorio de Control de Versiones Git

Se inicializó un repositorio Git local dedicado para registrar cada paso de la intervención con trazabilidad total:
- **Directorio Git:** `C:\Users\joses\.gemini\antigravity-ide\brain\7c43df94-b2aa-4361-853f-8fd32165a3b0\scratch\entrevista.git`
- **Worktree:** `c:\Users\joses\Obsidian\informe\ARQ-EMP\Entrevista`

### Historial de Commits:
1. `commit de81035` — **BASELINE: estado original antes de la revisión (2026-10-10)**
   Registra el estado intacto de los 12 archivos originales.
2. `commit [REVISION]` — **AUDITORIA Y CORRECCION: alineación exhaustiva con la transcripción y aclaración AC**
   Registra la reestructuración del Índice, la creación de `Aclaración adicional del entrevistado.md`, la corrección de los módulos 01 al 08 y la incorporación de la carpeta `09 - Revisión documental`.

---

## 4. Guía Detallada para Revertir los Cambios

Si el usuario deseara revertir total o parcialmente la revisión a su estado inicial, puede ejecutar cualquiera de las siguientes opciones desde una consola PowerShell:

### Opción A: Reversión Completa Mediante Git (Instantánea)
```powershell
$gd = "C:\Users\joses\.gemini\antigravity-ide\brain\7c43df94-b2aa-4361-853f-8fd32165a3b0\scratch\entrevista.git"
$wt = "c:\Users\joses\Obsidian\informe\ARQ-EMP\Entrevista"

# Restaurar todos los archivos al commit BASELINE
git --git-dir=$gd --work-tree=$wt checkout de81035 -- .

# Eliminar los archivos nuevos creados durante la revisión
git --git-dir=$gd --work-tree=$wt clean -fd
```

### Opción B: Restauración Completa desde el Respaldo ZIP
```powershell
$zip = "c:\Users\joses\Obsidian\informe\ARQ-EMP\_respaldos\Entrevista_antes_de_revision_2026-10-10.zip"
$dest = "c:\Users\joses\Obsidian\informe\ARQ-EMP\Entrevista"

# Eliminar carpeta actual y descomprimir estado original
Remove-Item -LiteralPath $dest -Recurse -Force
Expand-Archive -LiteralPath $zip -DestinationPath "c:\Users\joses\Obsidian\informe\ARQ-EMP"
```

### Opción C: Reversión Selectiva de un Módulo Específico
Para volver al estado original de un solo archivo (por ejemplo, el módulo de transporte):
```powershell
$gd = "C:\Users\joses\.gemini\antigravity-ide\brain\7c43df94-b2aa-4361-853f-8fd32165a3b0\scratch\entrevista.git"
$wt = "c:\Users\joses\Obsidian\informe\ARQ-EMP\Entrevista"

git --git-dir=$gd --work-tree=$wt checkout de81035 -- "02 - Transporte/Transporte y logística.md"
```
