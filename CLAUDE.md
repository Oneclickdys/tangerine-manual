# Documentación de Producto - Tangerine LMS

## Qué es este proyecto

Documentación de producto de Tangerine LMS construida con [Mintlify](https://mintlify.com/). Dirigida a editoriales (para formar a sus equipos), Product Managers y Service Desk internos. El objetivo es documentar todas las funcionalidades del LMS con screenshots reales y escenarios realistas.

## Archivos clave

| Archivo | Propósito |
|---------|-----------|
| `.credentials.json` | Credenciales de acceso a los entornos (gitignored) |
| `.test-scenarios.json` | Registro de usuarios, licencias y escenarios de prueba (gitignored) |
| `_guidelines/AGENT_GUIDELINES.md` | Directrices completas para generar documentación |
| `_templates/PLANTILLA.mdx` | Template base para páginas MDX |
| `mint.json` | Configuración de Mintlify (navegación, tabs, colores) |
| `inventario/*.json` | Inventario de features por aplicación |

## Entornos

El entorno principal es **latest**. Las URLs están en `.test-scenarios.json` bajo `_environment`:

- **Backoffice**: `publisher.tangerine-latest.oneclick.es` — Admin editorial (gestión de usuarios, licencias, contenidos, programas digitales)
- **Frontoffice**: `tangerine-latest.oneclick.es` — App de docentes y estudiantes
- **Admin School**: `admin-school-qa.stn-neds.com` — Administración de escuela

## Modos de funcionamiento de Tangerine

Tangerine tiene **dos modos** de funcionamiento para docentes y estudiantes. Esto es fundamental para el manual:

### 1. Self-service (modelo Google Classroom)
- El estudiante y el profesor se registran con una o más licencias
- El **docente crea la clase** y obtiene un código de clase
- El docente **comparte el código** con sus estudiantes
- Los **estudiantes se unen** introduciendo el código
- Es el modelo usado en nuestros escenarios de prueba

### 2. Administrador de escuela (roster gestionado)
- Hay un **administrador de escuela** que gestiona todo
- El admin organiza **grupos, estudiantes, profesores**
- Los usuarios no necesitan códigos ni auto-registrarse
- Se usa la app **Admin School** (`admin-school-qa.stn-neds.com`)

La documentación debe cubrir ambos flujos donde aplique.

## Usuarios y escenarios de prueba

Todo está en `.test-scenarios.json`. **Siempre** lee este archivo antes de crear usuarios o hacer screenshots.

### Convenciones de nombres

Los usuarios deben tener **nombres reales en español** para que los screenshots parezcan un entorno real. Hay una lista de nombres reservados en `naming_conventions.reserved_names`. Consulta qué nombres ya están usados en `users.*` antes de crear uno nuevo.

- Email pattern: `nombre.apellido@oneclick.es`
- Password estándar: `Manual2025!`

### Licencias

Las licencias creadas para el manual están en `licenses[]`. Antes de crear una nueva, verifica si alguna existente ya cubre lo que necesitas. La licencia principal (`qPFGvx`) tiene 999 usos y cubre los programas English 26 y Math 26.

## Cómo crear usuarios en el backoffice

Hay un comando `/create-user` disponible. Pero si lo haces manualmente:

1. **Login** en backoffice con el admin de `.credentials.json`
2. Menú lateral **Usuarios** > botón **Añadir usuario**
3. **Paso 1 (Detalles):** Nombre, Apellidos, Email, Rol (combobox), País (combobox), Colegio (combobox), Grupo (combobox filtrado por colegio) > botón **Añadir**
4. **Paso 2 (Contraseña):** Rellenar ambos campos > botón **Guardar** (no "Siguiente")
5. **Paso 3 (Licencias):** Toggle Tangerine AI + Redimir licencia buscando por código
6. **Paso 4 (Programas Escuela):** Normalmente no se necesita
7. **Registrar** el usuario en `.test-scenarios.json` con su UUID (extraído de la URL)

### Detalles importantes del formulario

- El rol por defecto es "Estudiante"
- El botón **Guardar** solo se habilita cuando hay cambios pendientes en el paso actual
- La contraseña **se pierde si navegas a otra pestaña sin guardar** — rellenar y guardar en el mismo paso
- El UUID del usuario está en la URL: `/user/{UUID}/{step}`
- Al seleccionar un colegio, el combobox de Grupos se filtra automáticamente

## Cómo crear licencias

1. Menú lateral **Licencias** > botón **Crear licencia**
2. Rellenar: Número de usos, Disponible para (Estudiante/Docente), País, Duración (Fecha fija o Rango en días/semanas/meses/años)
3. Botón **Crear** > redirige a configuración
4. Pestaña **Publicaciones** > **Añadir publicaciones** > buscar y seleccionar
5. Los **programas digitales** se asocian a través de las publicaciones (no directamente en la licencia)

### Relación publicaciones-programas

Los programas digitales (English 26, Math 26) están vinculados a publicaciones (SU Inglés). Para que un usuario acceda a un programa, su licencia debe incluir la publicación asociada. Puedes ver qué programas tiene una publicación en la columna "Programas" de la pestaña Publicaciones de la licencia.

## Cómo asignar licencias a usuarios

1. Ir a la edición del usuario > pestaña **Licencias**
2. Botón **Redimir licencia** > buscar por código > seleccionar > **Redimir**

## Estructura de la documentación

```
manual/
├── introduccion/          # Páginas generales
├── backoffice/            # Admin editorial
│   ├── usuarios/
│   ├── licencias/
│   ├── contenidos/
│   ├── escuelas/
│   └── programas-digitales/
├── frontoffice-docentes/  # App de profesores
│   ├── mis-aulas/
│   ├── gestion-aula/
│   ├── evaluaciones/
│   ├── curso/
│   └── lecciones/
├── estudiantes/           # App de alumnos
│   ├── mis-clases/
│   ├── tareas/
│   └── calificaciones/
├── admin-escuela/         # Administración de escuela
└── assets/screenshots/    # Screenshots organizados por app/feature
```

## Screenshots

- Usar Chrome DevTools MCP para capturar
- Guardar en `assets/screenshots/{app}/{feature}/`
- Nombres descriptivos en kebab-case: `lista-usuarios.png`, `formulario-crear.png`
- Solo incluir screenshots cuando aporten valor (ver guidelines)

## Herramientas de autor

Tangerine incluye dos herramientas de autor integradas que son transversales a la plataforma:

### Lemonade (Editor de Actividades)

Editor para crear actividades evaluativas con múltiples tipos de preguntas (selección, arrastrar y soltar, completar texto, matemáticas, etc.). Accesible desde:
- **Backoffice**: al crear contenidos de tipo actividad
- **Frontoffice docente**: al crear evaluaciones/quizzes dentro de una lección

Repositorio: `lemonade2-editor-builder/` (editor en `src/editor/`, tipos de preguntas en `src/questions/`)

### Mint (Editor de Páginas HTML)

Editor para construir páginas HTML interactivas (similar a Articulate Rise). Permite crear contenido educativo con componentes visuales. Accesible desde:
- **Backoffice**: al crear contenidos de tipo página
- **Frontoffice docente**: al editar contenido de lecciones

Repositorios: `mint-components/` (librería de componentes) y `mint-viewer/` (visualizador)

**Ambas herramientas deben documentarse** como parte de la documentación de producto, cubriendo tanto la perspectiva del editor en backoffice como la del docente en frontoffice.

## Código fuente del LMS

Si necesitas verificar comportamientos, el código está en el monorepo padre (`/Users/camilolopez/DEVS/repos/tangerine-global/`):

| App | Ruta | Notas |
|-----|------|-------|
| Backoffice frontend | `tangerine-backoffice/src/app/pages/` | Panel de administración editorial |
| Frontoffice frontend | `tangerine-frontoffice/src/core/lite/views/` | App de docentes y estudiantes |
| Admin School frontend | `tangerine-admin-school/` | Panel de administrador de escuela |
| Backend API v3 | `tangerine-api-v3/` | API principal |
| Backend i2c | `i2c_v2/` | Backend legacy (escuelas, etc.) |
| Tangerine AI | `tangerine-ai/` | Tutor AI y herramientas IA |
| Editor Lemonade | `lemonade2-editor-builder/` | Herramienta de autor para actividades (preguntas, quizzes) |
| Mint componentes | `mint-components/` | Herramienta de autor para páginas HTML (tipo Rise) |
| Mint viewer | `mint-viewer/` | Visualizador de páginas Mint |
| Viewer | `tangerine-viewer/` | Viewer de contenidos |

## Comandos disponibles

- `/document-feature feature-key` — Documenta una feature del inventario (ej: `/document-feature collections/list`). Explora código, navega la UI, captura screenshots y genera MDX.
- `/document-next` — Selecciona la siguiente feature pendiente por prioridad y la documenta automáticamente.
- `/document-batch app-name` — Documenta todas las features pendientes de una app (ej: `/document-batch frontoffice-teacher`). Delega cada feature a un subagente para no saturar el contexto del coordinador. Ejecuta secuencialmente.
- `/create-user role [name] [school] [group]` — Crea usuario de prueba via Chrome DevTools

## Tono y audiencia

- **Tercera persona descriptiva**: "El docente accede al dashboard..." en vez de "Haz clic en..."
- **Profesional pero no académico** — referencia: documentación de Stripe o Twilio
- **Audiencia**: equipos de implantación y formación de editoriales, Product Managers, Service Desk internos

### Contexto white-label

Tangerine es un producto **white-label**: ningún profesor ni alumno sabe que "Tangerine" existe. Las editoriales (Santillana, Edelvives, UNOi, Macmillan) redistribuyen el producto con su propia marca y crean sus propios materiales de formación. La documentación debe:

- Usar la **terminología genérica de Tangerine** (no la de ninguna editorial)
- Indicar qué **elementos son renombrables** por tenant cuando sea relevante
- Screenshots con **branding genérico** de Tangerine

### Variantes principales por configuración

Hay dos configuraciones que cambian significativamente la experiencia:

| Variante | Qué cambia |
|----------|-----------|
| **Planeador Avanzado vs Essential** | El Avanzado permite que los programas digitales definan atómicamente el contenido de cada sesión (no solo el número). Afecta al calendario y la planificación. |
| **Cuaderno de calificaciones vs Cuaderno de evaluación** | El de evaluación activa escalas, modelos de evaluación y calificaciones completas. |

Cuando una funcionalidad se comporta diferente según estas variantes, documentarlo explícitamente. Puede haber otras variantes menores — documentar conforme se descubran.

## Reglas

- **No asumas comportamientos** — verifica en el código o en la UI
- **Nombres reales** en español para usuarios de prueba
- **Siempre actualiza `.test-scenarios.json`** después de crear usuarios, licencias o escenarios
- **No commitees** archivos con credenciales (`.credentials.json`, `.test-scenarios.json`)
- Lee `_guidelines/AGENT_GUIDELINES.md` antes de generar documentación
