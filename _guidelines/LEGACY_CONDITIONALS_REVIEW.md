# Condicionales legacy pendientes de revisión

Listado de lógica condicional en el código del backoffice que se mantiene por retrocompatibilidad pero dificulta el mantenimiento y la documentación. Revisar para decidir si se elimina, se oculta tras feature flag, o se documenta como variante.

---

## Programas Digitales — Tipos de submódulo (lesson types)

**Archivos afectados:**
- `tangerine-backoffice/src/app/modules/programs/ProgramForm/ProgramFormContents/ProgramFormContentsLessons/ProgramFormContentsLessons.jsx`
- `tangerine-backoffice/src/app/modules/programs/ProgramForm/ProgramFormContents/ProgramFormContentsLessons/NewLesson.jsx`

**Situación:**
El código mantiene un sistema de tipos de submódulo (`teacher`/Clásico, `mint`/Página html, `game`/Juego, `powerLesson`) que se ofrece como selector al crear un nuevo submódulo. Sin embargo, con la opción **"Habilitar experiencia Power Lesson"** activada en el programa (que es el estándar actual), esta selección de tipo pierde sentido:

- **Clásico vs Power Lesson**: con Power Lesson activo, todos los submódulos son Power Lesson. El tipo "Clásico" es legacy.
- **Página html**: con Power Lesson, las páginas html se añaden como contenido dentro del submódulo (botón "+ Añadir" > "Página html"), no como un tipo de submódulo separado.
- **Juego**: residual de un cliente específico. No debería mostrarse en la documentación genérica.
- **Convertir a Power Lesson**: acción que transforma un submódulo Clásico en Power Lesson. Solo relevante en programas legacy sin Power Lesson activado.

**Impacto en documentación:**
No se documenta la selección de tipo de submódulo ni la acción "Convertir a Power Lesson" porque aplican solo a programas legacy. Si se decide mantener soporte para programas sin Power Lesson, habría que documentar estas variantes explícitamente.

**Decisión pendiente:**
- ¿Eliminar los tipos de submódulo del código y forzar siempre Power Lesson?
- ¿Ocultar el selector de tipo tras un feature flag para que solo se active en tenants legacy?
- ¿Documentar ambas variantes (con y sin Power Lesson)?

**Fecha de detección:** 2026-03-29

---

## Programas Digitales — Apariencia básica vs actual (`showProgramLookAdvanced`)

**Archivos afectados:**
- `tangerine-backoffice/src/app/modules/programs/ProgramForm/ProgramForm.jsx` (líneas 120-148)
- `tangerine-backoffice/src/app/modules/programs/ProgramForm/ProgramFormLook/ProgramFormLook.jsx`
- `tangerine-backoffice/src/app/modules/programs/ProgramForm/ProgramFormLookAdvanced/ProgramFormLookAdvanced.jsx`
- `tangerine-backoffice/src/app/modules/programs/ProgramForm/ProgramFormTemplate/ProgramFormTemplate.jsx`

**Situación:**
El código mantiene dos versiones de la pestaña Apariencia, controladas por el feature flag `showProgramLookAdvanced`:

- **Apariencia actual** (`ProgramFormLookAdvanced`): tipografía (encabezados + cuerpo), colores (primario + secundario con sólido/degradado), bordes (0-100%), portada y cabecera. Es el estándar para todos los programas nuevos.
- **Apariencia legacy** (`ProgramFormTemplate` + `ProgramFormLook`): selector de plantilla + tres imágenes (miniatura, portada, fondo). Solo se renderiza cuando `showProgramLookAdvanced` está desactivado.

**Impacto en documentación:**
Solo se documenta la versión actual (tipografía, colores, bordes, imágenes). La versión legacy se omite de la documentación. No se usa la terminología "avanzada" vs "básica" — la versión actual es simplemente "Apariencia".

**Decisión pendiente:**
- ¿Eliminar `ProgramFormLook` y `ProgramFormTemplate` del código y activar siempre la versión actual?
- ¿Hay tenants que aún usen la apariencia legacy? Si no, se puede limpiar.

**Fecha de detección:** 2026-03-29

---

## Programas Digitales — Importar desde Mint (`haveImportMint`)

**Archivos afectados:**
- `tangerine-backoffice/src/app/modules/programs/ProgramForm/ProgramFormContents/ProgramFormContentsLessons/ProgramFormContentsLessons.jsx`
- `tangerine-backoffice/src/app/modules/programs/ProgramForm/ProgramFormContents/DialogUploadImportMint/`

**Situación:**
El código incluye una acción "Importar desde Mint" en los submódulos, condicionada a `setting.cms.haveImportMint`. Permite importar contenido desde el editor Mint como un archivo subido. No aparece en la UI estándar — probablemente es específico de un tenant o un flujo legacy.

**Impacto en documentación:**
Omitido de la documentación de contenidos del programa. Las páginas Mint se crean directamente desde el menú "+ Añadir" > "Página html", que es el flujo estándar.

**Decisión pendiente:**
- ¿Qué tenants usan `haveImportMint`? ¿Sigue siendo necesario?
- ¿Se puede eliminar o unificar con el flujo estándar de crear página html?

**Fecha de detección:** 2026-03-29

---

## Inventario de feature flags encontrados en Programas Digitales

Referencia completa de los feature flags que controlan el comportamiento de la vista de edición de programas digitales (`ProgramForm.jsx`). Útil para auditar qué flags siguen siendo necesarios y cuáles podrían eliminarse o activarse por defecto.

| Feature flag | Qué controla | ¿Debería ser el default? | Notas |
|---|---|---|---|
| `showProgramming` | Muestra pestaña Programación (básica) | Revisar | No activo en latest |
| `showProgrammingAdvanced` | Activa programación avanzada (contenido atómico por sesión) | Revisar | No activo en latest |
| `showProgrammingTeachingLoad` | Muestra pestaña Carga docente | Revisar | No activo en latest |
| `showProgramArticles` | Muestra sección Artículos en pestaña Datos | Revisar | |
| `showProgramResources` | Muestra pestaña Recursos anuales (y oculta Caja de herramientas) | Revisar | |
| `showProgramLookAdvanced` | Apariencia actual (tipografía, colores, bordes) vs legacy (plantilla + imágenes) | **Sí — ya es el estándar** | Legacy documentado arriba |
| `showProgramHomeCustomization` | Muestra pestaña Inicio (bloques configurables) | Revisar | Solo con plantilla Primary Fixed/Hybrid |
| `showProgramType` | Selector de tipo de programa en pestaña Datos | Revisar | |
| `showExportXapi` | Botón Exportar xAPI en pestaña Contenidos | Revisar | |
| `enablePowerLesson` | Toggle "Habilitar experiencia Power Lesson" en creación | **Sí — ya es el estándar** | Legacy documentado arriba |
| `showStructureLevels` | Selector de estructura editorial (1 o 2 niveles) | Revisar | |

**Otros condicionales (no feature flags):**

| Condicional | Qué controla | Notas |
|---|---|---|
| `setting.cms.haveImportMint` | Acción "Importar desde Mint" en submódulos | Legacy documentado arriba |
| `isAccessProgramFormResources()` | Pestaña Recursos (editor JSON) | Control de acceso por política, no por flag |
| `isPrimaryFixedOrHybridTemplate()` | Requisito adicional para pestaña Inicio | Depende de la plantilla del programa |
| `isEdelvives()` | Funcionalidades específicas de Edelvives (evaluaciones, template editor) | Condicional por tenant |
| `isKanban()` | Funcionalidades de tablero Kanban en módulos | Condicional por plantilla |
| `canCreateMoreTheOneUnit()` | Permite crear más de un módulo | Política de acceso |
| `canUnitCopy()` | Permite copiar/mover módulos entre programas | Política de acceso |

**Fecha de recopilación:** 2026-03-29

---

## Cómo usar este documento

Al documentar nuevas features, si se encuentran condicionales similares (código que se mantiene por retrocompatibilidad, features de clientes específicos, o lógica que confunde al intentar documentar el comportamiento estándar), añadir una entrada siguiendo el mismo formato:

1. **Título** descriptivo
2. **Archivos afectados** en el código
3. **Situación** explicando qué hace el condicional y por qué es problemático
4. **Impacto en documentación** — qué se omitió o cómo se resolvió
5. **Decisión pendiente** — opciones para resolverlo
6. **Fecha de detección**
