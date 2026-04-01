---
name: document-feature
description: Documenta una feature del inventario de Tangerine LMS generando páginas MDX con screenshots reales. Uso: /document-feature publications/list
---

Documenta una feature del inventario de Tangerine LMS generando páginas MDX con screenshots reales.

**Input:** Feature key = $ARGUMENTS (ej: `publications/list`, `collections/list`, `settings/general`)

## Proceso

### 1. Localizar la feature

Busca la key `$ARGUMENTS` en los archivos de inventario (`inventario/*.json`). Si no la encuentras, lista las features pendientes disponibles y pide al usuario que elija.

Lee del inventario:
- `name`, `description`, `section`, `route`
- `complexity` → determina la profundidad de la documentación
- `codeFiles` → rutas del código fuente a explorar
- `featureFlags`, `tenantVariations` → variantes a documentar
- `relatedFeatures` → para enlaces contextuales
- `notes` → contexto adicional del PM

### 2. Exploración en paralelo

Lanza DOS agentes en paralelo:

**Agente A — Código fuente:**
Explora los `codeFiles` del inventario en el monorepo (`/Users/camilolopez/DEVS/repos/tangerine-global/`). Debe extraer:
- Campos del formulario (nombres, obligatoriedad, propósito)
- Columnas de la tabla (si es listado)
- Acciones/botones y permisos por rol
- Feature flags que afectan el comportamiento
- Variantes multipaís u otras configuraciones condicionales
- Relaciones con otras entidades

**Agente B — Código fuente (rutas y menú):**
Busca en el router y el servicio de menú del backoffice las rutas y configuración de navegación de la feature. Verifica en qué sección del menú aparece.

### 3. Navegación UI y screenshots

Lee las credenciales de `/Users/camilolopez/DEVS/repos/tangerine-global/manual/.credentials.json`.

Usa Chrome DevTools MCP para:
1. Navegar a la URL del entorno (`publisher.tangerine-latest.oneclick.es` para backoffice, `tangerine-latest.oneclick.es` para frontoffice)
2. Iniciar sesión con el usuario apropiado si no hay sesión activa
3. Navegar a la `route` de la feature
4. Capturar screenshots de:
   - Vista principal (listado o dashboard)
   - Filtros desplegados (si aplica)
   - Formulario de creación (si aplica)
   - Detalle/edición (si aplica)
   - Estados especiales relevantes

Guarda los screenshots en `assets/screenshots/{app}/{feature-slug}/` con nombres descriptivos en kebab-case.

### 4. Decidir estructura de páginas

Según el tamaño y complejidad de la feature:

- **Feature simple** (simple-list, simple-form): Una sola página MDX
- **Feature media** (crud-complete, interactive-view): 1-2 páginas (ej: gestión + crear)
- **Feature compleja** (complex-workflow, editor, multi-step-form): 2-3 páginas

**Subdivisión de features grandes:** Después de la exploración de código y UI (pasos 2-3), si una feature resulta más extensa de lo que su `complexity` sugiere, puedes dividirla en más subsecciones. Criterios para subdividir:

- La feature tiene **más de 3 flujos independientes** (ej: crear, editar, duplicar, importar, configurar)
- Un solo paso o tab del formulario tiene **complejidad suficiente para una página propia** (ej: un editor embebido, un configurador con múltiples opciones)
- Documentar todo en el rango estándar de páginas produciría páginas **excesivamente largas** (más de ~400 líneas de MDX) o difíciles de navegar
- La feature tiene **variantes significativas** que merecen documentación separada (ej: Planeador Avanzado vs Essential como páginas independientes en lugar de Tabs)

Cuando subdividas:
1. Crea una **página índice** de la feature con descripción general y enlaces a las subpáginas
2. Cada subpágina debe ser **autocontenida** (no requiere leer las demás para entenderse)
3. Organiza en una **subcarpeta** con el slug de la feature (ej: `backoffice/programas-digitales/vista-general.mdx`, `backoffice/programas-digitales/editor-sesiones.mdx`)
4. Actualiza la navegación en `docs.json` creando un subgrupo si es necesario

Si hay features relacionadas pendientes en la misma sección (ej: `list` + `create` + `detail`), documentarlas juntas en las mismas páginas. Actualiza el inventario de TODAS las features cubiertas.

### 5. Generar documentación

Lee las directrices completas en `_guidelines/AGENT_GUIDELINES.md` y usa `_templates/PLANTILLA.mdx` como base.

**Reglas clave:**
- Tercera persona descriptiva ("El administrador accede...", "El sistema muestra...")
- Tono profesional tipo Stripe/Twilio
- NO enumerar filtros ni controles autoexplicativos en tablas descriptivas. Los filtros se entienden solos
- NO incluir validaciones técnicas (límites de caracteres, min/max, debounce). Solo restricciones de negocio relevantes
- Incluir sección "Configuración y variantes" con `<Tabs>` si hay comportamiento condicional
- Incluir sección "Situaciones especiales" con edge cases reales
- Los screenshots deben tener propósito claro — no capturar pantallas obvias
- Enlaces contextuales a features relacionadas, no genéricos
- Ortografía española impecable: tildes (á, é, í, ó, ú), eñes (ñ), diéresis (ü) y signos de apertura (¿, ¡) en TODO el contenido (frontmatter, encabezados, tablas, callouts, acordeones)

**Estructura de cada página:**
1. Frontmatter (title, description, icon)
2. Descripción + Rol requerido
3. Cómo acceder (si no es obvio)
4. Contenido principal (listado, formulario, flujo)
5. Configuración y variantes (si aplica)
6. Situaciones especiales
7. Relacionado (CardGroup)
8. Metadatos internos (comentario)

Crea los archivos MDX en el directorio correspondiente:
- Backoffice: `backoffice/{seccion}/{slug}.mdx`
- Docentes: `frontoffice-docentes/{seccion}/{slug}.mdx`
- Estudiantes: `estudiantes/{seccion}/{slug}.mdx`
- Admin Escuela: `admin-escuela/{seccion}/{slug}.mdx`

### 6. Actualizar navegación e inventario

**mint.json:**
- Añade las nuevas páginas al grupo correspondiente
- Si el grupo no existe, créalo en la posición lógica dentro de la navegación

**Inventario (`inventario/{app}.json`):**
- Actualiza `status: "documented"` para cada feature cubierta
- Añade `docFile` con la ruta al MDX
- Actualiza `lastDocumented` con la fecha de hoy

**Inventario (`inventario/index.json`):**
- Actualiza los contadores `byStatus.documented` y `byStatus.pending`

### 7. Resumen

Al terminar, muestra un resumen con:
- Páginas creadas
- Screenshots capturados
- Features marcadas como documentadas
- Archivos modificados

NO hagas commit automáticamente. El usuario decidirá cuándo hacer commit.
