# Directrices para Generación de Documentación Funcional

Este documento define las reglas que el agente debe seguir al generar o actualizar documentación funcional de Tangerine LMS.

---

## 0. Flujo de trabajo del agente

### Entrada: Inventario de features

El agente recibe como input una o más features del inventario ubicado en `/manual/inventario/`. Cada archivo JSON contiene features en este formato:

```json
{
  "feature-key": {
    "name": "Nombre visible",
    "description": "Descripción breve",
    "section": "Sección en la documentación",
    "route": "Ruta en la aplicación",
    "priority": "P1|P2|P3|P4",
    "status": "pending|in_progress|documented|needs_review|blocked",
    "complexity": "simple-list|simple-form|crud-complete|multi-step-form|complex-workflow|interactive-view|dashboard|editor",
    "docFile": "ruta/al/archivo.mdx o null",
    "codeFiles": {
      "frontend": ["rutas/al/código/"],
      "backend": ["rutas/al/código/"]
    },
    "tenantVariations": ["Variaciones por cliente"],
    "featureFlags": ["flags.que.controlan.feature"],
    "relatedFeatures": ["otras-features-relacionadas"],
    "lastDocumented": "fecha o null",
    "notes": "Notas adicionales del PM"
  }
}
```

### Archivos de inventario disponibles

| Archivo | Aplicación |
|---------|------------|
| `/manual/inventario/backoffice.json` | Panel de administración |
| `/manual/inventario/frontoffice-teacher.json` | App de docentes |
| `/manual/inventario/frontoffice-student.json` | App de estudiantes |
| `/manual/inventario/admin-school.json` | Admin de escuela |

### Proceso de documentación

```
┌─────────────────────────────────────────────────────────────────┐
│ 1. ANÁLISIS DEL INVENTARIO                                      │
│    - Leer la feature del inventario                             │
│    - Identificar complexity para determinar profundidad         │
│    - Revisar tenantVariations y featureFlags                    │
│    - Consultar relatedFeatures para enlaces contextuales        │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│ 2. EXPLORACIÓN DEL CÓDIGO (cuando sea necesario)                │
│    - Navegar a codeFiles.frontend para entender el flujo        │
│    - Identificar estados, validaciones, casos edge              │
│    - Buscar textos/labels en el código para precisión           │
│    - Revisar codeFiles.backend para entender reglas de negocio  │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│ 3. GENERACIÓN DE DOCUMENTACIÓN                                  │
│    - Aplicar template según complexity                          │
│    - Seguir directrices de este documento                       │
│    - Incluir variaciones de tenant si aplican                   │
│    - Documentar comportamiento de feature flags                 │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│ 4. GENERACIÓN DE SCREENSHOTS                                    │
│    - Cargar credenciales de /manual/.credentials.json           │
│    - Navegar a route en entorno de QA                           │
│    - Capturar pantallas según flujo documentado                 │
│    - Guardar en /manual/assets/screenshots/[app]/[feature]/     │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│ 5. ACTUALIZACIÓN DE INVENTARIO Y NAVEGACIÓN                     │
│    - Actualizar status y docFile en el JSON del inventario      │
│    - Actualizar /manual/mint.json si es necesario               │
│    - Commit y push al repositorio                               │
└─────────────────────────────────────────────────────────────────┘
```

### Cuándo consultar el código fuente

**IMPORTANTE:** NO asumas comportamientos que no puedas verificar. Documentar información incorrecta es peor que no documentarla. Ante la duda, consulta el código fuente o pregunta al usuario.

**Siempre consultar el código cuando:**

- La feature tiene `complexity: "complex-workflow"`, `"multi-step-form"` o `"editor"`
- El inventario tiene `notes` que indican comportamiento especial
- Hay `featureFlags` que modifican el comportamiento
- Hay `tenantVariations` que necesitas entender en detalle
- No está claro qué validaciones o restricciones aplican
- Necesitas los textos exactos de botones, labels o mensajes de error

**Rutas del código fuente:**

| Aplicación | Ruta base |
|------------|-----------|
| Backoffice frontend | `/tangerine-backoffice/src/app/pages/` |
| Frontoffice frontend | `/tangerine-frontoffice/src/core/lite/views/` |
| Backend APIs | `/tangerine-api-v3/` y `/i2c_v2/` |

**Archivos relacionados a buscar:**
- Hooks: `use*.js`
- Servicios: `*.service.js`
- CRUDs: `*.crud.js`
- Validaciones: `schema`, `validator`, `onSubmit`

**Qué buscar en el código:**

| Necesitas saber... | Busca en... |
|-------------------|-------------|
| Flujo de navegación | Componentes de vista (`*View/`) |
| Validaciones de formulario | Schemas, validators, o `onSubmit` handlers |
| Textos y labels | Archivos de i18n o strings en componentes |
| Casos edge y estados de error | Bloques `catch`, estados `error`, `empty` |
| Permisos y roles | Guards, middlewares, checks de `permissions` |
| Reglas de negocio | `codeFiles.backend`, servicios, CRUDs |

**Qué NO documentar sin verificar:**

- Mensajes de error específicos
- Límites o restricciones (ej: "máximo 10 caracteres")
- Comportamientos condicionales por rol o configuración
- Integraciones con otros sistemas

---

## 1. Estructura de cada página

### Empieza con contexto de decisión, no solo con instrucciones

Antes de explicar *cómo* hacer algo, explica brevemente *cuándo* y *por qué* elegir esa opción.

**Incorrecto:**
```
Haz clic en Guardar como borrador.
```

**Correcto:**
```
Usa Guardar como borrador cuando quieras revisar la evaluación antes
de que los estudiantes la vean, o si planeas terminarla más tarde.
Es la opción más segura si es tu primera evaluación.
```

### Termina con casos edge, no los omitas

Cada página debe incluir una sección **"Situaciones especiales"** que cubra:

- Qué pasa si el usuario ya hizo algo (editó, publicó, recibió respuestas)
- Qué pasa si quiere deshacer o modificar
- Limitaciones conocidas del sistema
- Permisos necesarios si varían por rol

**Preguntas que esta sección debe responder:**

- ¿Puedo editar después de publicar?
- ¿Qué pasa con los datos existentes si hago cambios?
- ¿Puedo duplicar/copiar esto completo?
- ¿Cómo afecta a otros usuarios (estudiantes, otros docentes)?

---

## 2. Templates por complexity

### `simple-list`
- Documentación breve (1-2 páginas)
- Descripción + cómo acceder + listado de acciones disponibles
- Screenshots mínimos (1-2)

### `simple-form` / `crud-complete`
- Documentación media (2-3 páginas)
- Descripción de campos y validaciones
- Screenshots del formulario
- Casos de error comunes

### `interactive-view` / `dashboard`
- Documentación media (2-3 páginas)
- Descripción de cada sección/tab de la vista
- Interacciones principales
- Screenshots de estados importantes

### `multi-step-form`
- Documentación detallada (3-5 páginas)
- Cada paso como sección separada
- Validaciones y mensajes de error
- Screenshots de cada paso
- Casos edge y situaciones especiales

### `complex-workflow` / `editor`
- Documentación extensa (4-6 páginas)
- Diagrama de flujo del proceso completo
- Múltiples escenarios documentados
- Integración con otras features
- Troubleshooting detallado

---

## 3. Profundidad consistente

### Todas las opciones paralelas deben tener el mismo nivel de detalle

Si documentas dos o más formas de hacer algo, cada una necesita:

- El mismo número aproximado de pasos
- El mismo nivel de explicación
- Screenshots equivalentes si una los tiene

**Regla práctica:** Si la Opción A tiene 5 pasos con capturas, la Opción B no puede tener 3 pasos sin capturas.

---

## 4. Gestión de contenido extenso

### Mueve el contenido de referencia a páginas separadas

**Contenido de referencia** = más de 3 tablas O más de 15 items en lista.

**Estructura recomendada:**

En la página principal:
```markdown
## Tipos de preguntas

Los tipos más usados son:

- **Selección única**: Para preguntas de test rápidas
- **Texto largo**: Para respuestas tipo ensayo
- **Ordenar**: Para evaluar comprensión de secuencias

👉 [Ver catálogo completo de tipos de preguntas](/referencia/tipos-preguntas)
```

### Usa acordeones para contenido opcional

```markdown
<Accordion title="Ver todos los tipos de preguntas disponibles">
  [Contenido extenso aquí]
</Accordion>
```

---

## 5. Screenshots y anotaciones

### Cada screenshot debe tener un propósito claro

**Incluye una captura cuando:**

- El elemento a clickear no es obvio desde la descripción textual
- La interfaz tiene múltiples opciones y necesitas señalar una específica
- El resultado de una acción no es predecible
- El usuario podría confundir elementos similares

**No incluyas capturas para:**

- Pantallas que el usuario ya conoce (Home, lista de cursos)
- Confirmaciones simples o mensajes de éxito estándar
- Cada paso de un flujo lineal obvio

### Añade anotaciones cuando la interfaz es densa

**Regla:** Si el screenshot muestra más de 5 elementos interactivos, usa anotaciones.

**Tipos de anotaciones:**

| Situación | Anotación recomendada |
|-----------|----------------------|
| Señalar un botón específico | Flecha + círculo |
| Mostrar secuencia de clics | Números (1, 2, 3) |
| Destacar área importante | Rectángulo con borde |
| Indicar qué ignorar | Área oscurecida/difuminada |

### Ubicación de screenshots

```
/manual/assets/screenshots/
├── backoffice/
│   ├── usuarios/
│   │   ├── lista-usuarios.png
│   │   ├── crear-usuario.png
│   │   └── editar-usuario-detalles.png
│   └── licencias/
│       ├── lista-licencias.png
│       └── crear-licencia.png
├── frontoffice-docente/
│   └── evaluaciones/
│       ├── 01-acceso-menu.png
│       └── 02-formulario-crear.png
└── frontoffice-estudiante/
    └── tareas/
        └── entregar-tarea.png
```

---

## 6. Contexto pedagógico

### Conecta la funcionalidad con la práctica docente

Cuando sea natural, añade una línea que ayude al profesor a entender cuándo usar cada opción en su contexto real de enseñanza.

**Incorrecto:**
```
El tipo Ordenar permite configurar elementos en secuencia.
```

**Correcto:**
```
El tipo Ordenar permite configurar elementos en secuencia. Funciona bien
para evaluar comprensión de procesos (ciclo del agua, pasos de un algoritmo)
o cronologías históricas.
```

### Dónde incluir contexto pedagógico

- **Siempre:** En tipos de preguntas principales
- **Siempre:** En opciones de configuración que afectan la experiencia del estudiante
- **Cuando aplique:** En decisiones de publicación/programación
- **No forzar:** En pasos puramente mecánicos (guardar, cancelar, navegar)

---

## 7. Navegación y enlaces

### Los enlaces deben ser contextuales, no genéricos

**Incorrecto:**
```markdown
## Relacionado
- Ver resultados
- Libro de calificaciones
```

**Correcto:**
```markdown
Una vez publicada tu evaluación, puedes seguir el progreso de los
estudiantes en [Ver resultados de evaluación](/evaluaciones/ver-resultados).
```

### Incluye breadcrumbs en los primeros pasos

**Incorrecto:**
```
1. Navega a un curso
```

**Correcto:**
```
1. Desde el Home (la pantalla inicial después de iniciar sesión),
   selecciona uno de tus cursos en la sección "Mis clases"
```

---

## 8. Formato y tono

### Usa voz activa y segunda persona

```
❌ "La evaluación puede ser guardada como borrador"
✅ "Guarda la evaluación como borrador"

❌ "El usuario debe hacer clic en el botón"
✅ "Haz clic en el botón"
```

### Sé específico en las instrucciones

```
❌ "Haz clic en el botón de la esquina"
✅ "Haz clic en el botón azul 'Publicar' en la esquina superior derecha"

❌ "Completa el formulario"
✅ "Completa los campos obligatorios: Nombre (máx. 100 caracteres) y Fecha de entrega"
```

### Usa callouts para información importante

```markdown
<Note>
Los borradores no son visibles para los estudiantes hasta que se publiquen.
</Note>

<Warning>
Editar una evaluación con respuestas existentes puede afectar las calificaciones ya registradas.
</Warning>

<Tip>
Recomendamos guardar como borrador primero si es tu primera evaluación.
</Tip>
```

---

## 9. Manejo de tenantVariations y featureFlags

### Variaciones por tenant

Si la feature tiene variaciones por tenant:

```markdown
<Note>
Esta funcionalidad puede variar según tu institución.
</Note>

## Flujo estándar
[Documentar el comportamiento por defecto]

## Variaciones por institución

<Accordion title="UNOi">
En lugar de Stream, verás un Panel con [descripción de diferencias].
</Accordion>

<Accordion title="Richmond">
[Descripción de variación específica]
</Accordion>
```

### Feature flags

Si la feature está controlada por flags:

```markdown
<Note>
Esta funcionalidad puede no estar disponible en todas las instituciones.
Si no ves esta opción, contacta a tu administrador.
</Note>
```

---

## 10. Checklist de validación

Antes de considerar una página completa, verifica:

### Pre-documentación
- [ ] ¿Revisé el `complexity` para determinar la profundidad adecuada?
- [ ] ¿Consulté el código fuente si `complexity` es `complex-workflow` o `multi-step-form`?
- [ ] ¿Identifiqué todas las `tenantVariations` que debo documentar?
- [ ] ¿Verifiqué si hay `featureFlags` que afectan la disponibilidad?
- [ ] ¿Revisé las `relatedFeatures` para enlaces contextuales?
- [ ] ¿Leí las `notes` del PM para contexto adicional?

### Estructura
- [ ] ¿Explica cuándo usar esta funcionalidad vs alternativas?
- [ ] ¿Los casos edge están documentados en "Situaciones especiales"?

### Consistencia
- [ ] ¿Todas las opciones paralelas tienen el mismo nivel de detalle?
- [ ] ¿El tono es consistente con otras páginas de la documentación?

### Visual
- [ ] ¿Los screenshots tienen propósito claro?
- [ ] ¿Las interfaces densas tienen anotaciones?
- [ ] ¿El alt text es descriptivo?

### Contexto
- [ ] ¿Hay contexto pedagógico en funcionalidades clave?
- [ ] ¿Los enlaces son contextuales, no genéricos?
- [ ] ¿Se mencionan las variaciones de tenant si aplican?
- [ ] ¿Se indica si la feature puede no estar disponible (feature flags)?

### Calidad
- [ ] ¿Se usa voz activa y segunda persona?
- [ ] ¿Las instrucciones son específicas?
- [ ] ¿Los callouts destacan información importante?

### Post-documentación
- [ ] ¿Actualicé el inventario JSON con `status: "documented"`?
- [ ] ¿Actualicé `docFile` con la ruta al archivo creado?
- [ ] ¿Registré la fecha en `lastDocumented`?
- [ ] ¿Actualicé `mint.json` si agregué nuevas páginas?
- [ ] ¿Hice commit y push al repositorio?

---

## 11. Estructura de archivos

### Organización de carpetas

```
/manual/
├── mint.json                          # Configuración Mintlify
├── .credentials.json                  # Credenciales (NO commitear)
├── .credentials.example.json          # Template de credenciales
├── inventario/                        # Inventario de features (JSON)
│   ├── schema.json
│   ├── index.json
│   ├── backoffice.json
│   ├── frontoffice-teacher.json
│   ├── frontoffice-student.json
│   └── admin-school.json
├── _templates/                        # Templates internos
│   └── PLANTILLA.mdx
├── _guidelines/                       # Guías para el agente
│   └── AGENT_GUIDELINES.md           # Este archivo
├── introduccion/
│   └── que-es-tangerine.mdx
├── backoffice/
│   ├── usuarios/
│   │   └── gestion-usuarios.mdx
│   ├── licencias/
│   │   └── gestion-licencias.mdx
│   └── ...
├── frontoffice-docentes/
│   ├── gestion-aula/
│   ├── evaluaciones/
│   └── ...
├── estudiantes/
│   └── ...
└── assets/
    └── screenshots/
        ├── backoffice/
        ├── frontoffice-docente/
        └── frontoffice-estudiante/
```

### Convención de nombres

**Archivos MDX:**
- Kebab-case: `crear-evaluacion.mdx`, `ver-resultados.mdx`
- Usar verbos para acciones: `crear-`, `ver-`, `editar-`, `configurar-`
- Usar sustantivos para vistas: `gradebook.mdx`, `calendario.mdx`

**Screenshots:**
- Formato: `[descripcion-breve].png` o `[numero]-[descripcion].png`
- Descripción en kebab-case: `lista-usuarios.png`, `05-menu-anadir.png`

### Frontmatter requerido

```yaml
---
title: "Crear Evaluación"
description: "Crea evaluaciones y tareas personalizadas para tus estudiantes"
icon: "clipboard-check"  # Icono de Lucide (opcional)
---
```

---

## 12. Credenciales y acceso

### Archivo de credenciales

Las credenciales para acceder a los entornos de QA están en `/manual/.credentials.json`:

```json
{
  "environments": {
    "qa": {
      "backoffice": {
        "url": "https://backoffice.tangerine-qa1.oneclicklabs.es",
        "users": {
          "admin": { "username": "...", "password": "..." }
        }
      },
      "frontoffice": {
        "url": "https://tangerine-latest.oneclick.es",
        "users": {
          "teacher": { "username": "...", "password": "..." },
          "student": { "username": "...", "password": "..." }
        }
      }
    }
  }
}
```

### Proceso de captura de screenshots

1. Leer credenciales del archivo `.credentials.json`
2. Usar Chrome DevTools MCP para navegar
3. Autenticarse con el usuario apropiado según el rol
4. Navegar a la ruta de la feature
5. Capturar screenshots con nombres descriptivos
6. Guardar en `/manual/assets/screenshots/[app]/[feature]/`

---

## 13. Actualización del inventario

Después de documentar una feature, actualizar el JSON correspondiente:

```json
{
  "feature-key": {
    "status": "documented",
    "docFile": "backoffice/usuarios/gestion-usuarios.mdx",
    "lastDocumented": "2025-12-20",
    "notes": "Notas originales + descubrimientos durante documentación"
  }
}
```

También actualizar `/manual/INVENTARIO_FUNCIONALIDADES.md` para mantener sincronizado el tracking visual.

---

## 14. Ejemplo de flujo completo

### Feature a documentar

```json
{
  "users/list": {
    "name": "Listado de usuarios",
    "section": "Usuarios",
    "route": "/users",
    "priority": "P1",
    "status": "pending",
    "complexity": "crud-complete",
    "codeFiles": {
      "frontend": ["tangerine-backoffice/src/app/pages/users/"]
    }
  }
}
```

### Pasos del agente

1. **Análisis:** `complexity: "crud-complete"` → documentación media
2. **Código:** Revisar componentes para entender filtros y acciones
3. **Screenshots:**
   - Navegar a `https://backoffice.tangerine-qa1.oneclicklabs.es/users`
   - Capturar: lista, filtros, detalle
4. **Documentación:** Crear `/manual/backoffice/usuarios/gestion-usuarios.mdx`
5. **Actualización:**
   - Actualizar `backoffice.json` con `status: "documented"`
   - Actualizar `mint.json` si es necesario
   - Commit y push
