# Inventario de Funcionalidades - Tangerine LMS

Este documento lista todas las funcionalidades de Tangerine que requieren documentacion de usuario.

## Leyenda de Prioridad
- **P1** - Critica (funcionalidad core, usada diariamente)
- **P2** - Alta (funcionalidad importante, uso frecuente)
- **P3** - Media (funcionalidad complementaria)
- **P4** - Baja (funcionalidad avanzada o poco usada)

## Leyenda de Estado
- [ ] Pendiente
- [x] Documentado
- [-] En progreso
- [~] Parcialmente documentado

---

## BACKOFFICE (Administradores)

### Usuarios y Permisos

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Listar usuarios | `/users` | P1 | [x] | `backoffice/usuarios/gestion-usuarios.mdx` |
| Crear usuario | `/user/create` | P1 | [x] | `backoffice/usuarios/gestion-usuarios.mdx` |
| Editar usuario | `/user/:guid` | P1 | [x] | `backoffice/usuarios/gestion-usuarios.mdx` |
| Usuarios backoffice | `/users-admin` | P2 | [ ] | |
| Crear usuario admin | `/user/create-admin` | P2 | [ ] | |

### Licencias

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Listar licencias | `/licenses` | P1 | [x] | `backoffice/licencias/gestion-licencias.mdx` |
| Crear licencia | `/license/create` | P1 | [x] | `backoffice/licencias/gestion-licencias.mdx` |
| Detalle licencia | `/license/:guid` | P2 | [x] | `backoffice/licencias/gestion-licencias.mdx` |
| Lotes de licencias | `/licenses-batch` | P2 | [x] | `backoffice/licencias/gestion-licencias.mdx` |
| Crear lote | `/license-batch/create` | P2 | [x] | `backoffice/licencias/gestion-licencias.mdx` |

### Escuelas

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Listar escuelas | `/schools` | P1 | [ ] | |
| Crear escuela | `/school/create` | P1 | [ ] | |
| Detalle escuela | `/school/:guid` | P2 | [ ] | |

### Contenidos

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Listar contenidos | `/contents` | P1 | [ ] | |
| Crear contenido | `/contents/create` | P2 | [ ] | |
| Detalle contenido | `/content/:guid` | P2 | [ ] | |
| Carga por lotes | `/contents/batch-upload` | P3 | [ ] | |

### Biblioteca

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Biblioteca | `/library` | P2 | [ ] | |

### Colecciones

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Listar colecciones | `/collections` | P2 | [ ] | |
| Crear coleccion | `/collection/create` | P3 | [ ] | |
| Detalle coleccion | `/collection/:guid` | P3 | [ ] | |

### Publicaciones

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Listar publicaciones | `/publications` | P2 | [ ] | |
| Crear publicacion | `/publication/create` | P2 | [ ] | |
| Detalle publicacion | `/publication/:guid` | P3 | [ ] | |

### Programas Digitales

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Listar programas | `/programs` | P1 | [ ] | |
| Crear programa | `/program/create` | P1 | [ ] | |
| Detalle programa | `/program/:guid` | P2 | [ ] | |
| Editor Mint | `/program/mint-stand-alone/:guid/:role` | P3 | [ ] | |

### Grupos/Clases

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Listar grupos | `/groups` | P2 | [ ] | |
| Crear grupo | `/group/create` | P2 | [ ] | |

### Configuracion del Sistema

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Configuracion general | `/settings` | P2 | [ ] | |
| Paises | `/settings-countries` | P2 | [ ] | |
| Objetivos de aprendizaje | `/settings-learning-objectives` | P3 | [ ] | |
| Categorias de evaluacion | `/settings-categories` | P2 | [ ] | |
| Escalas de evaluacion | `/settings-scales` | P2 | [ ] | |
| Periodos de evaluacion | `/settings-periods` | P1 | [ ] | |

### Personalizacion

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Personalizar programas | `/personalize` | P3 | [ ] | |

### Mensajes

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Sistema de mensajes | `/messages` | P3 | [ ] | |

### Analiticas

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Analytics externos | `/analytic/external` | P3 | [ ] | |
| Analytics de programas | `/analytic/programs` | P2 | [ ] | |
| Analytics de juegos | `/analytic/game` | P4 | [ ] | |
| Analytics de licencias | `/analytic/licenses` | P2 | [ ] | |
| Analytics de uso | `/analytic/usage` | P2 | [ ] | |

### Herramientas

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Normalizador SCORM | `/scormnormalize` | P4 | [ ] | |
| Editor Fixed Layout | `/fixedlayout-editor/:id` | P4 | [ ] | |

---

## FRONTOFFICE - DOCENTES

### Mis Aulas

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Ver mis aulas | `/home` | P1 | [ ] | |
| Crear nueva aula | `/new-classroom` | P1 | [ ] | |
| Configurar curso | `/configure-course/:guid` | P2 | [ ] | |

### Curso/Aula

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Vista del curso | `/course/:guid` | P1 | [ ] | |
| Stream (muro social) | `/course/:guid` (tab Stream) | P1 | [ ] | |
| Programa del curso | `/course/:guid` (tab Program) | P1 | [ ] | |
| Recursos del curso | `/course/:guid` (tab Resources) | P2 | [ ] | |
| Calificaciones | `/course/:guid` (tab Grades) | P1 | [ ] | |

### Lecciones

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Vista de leccion | `/course/:courseGuid/lesson/:lessonGuid` | P1 | [ ] | |
| Vista Power (presentacion) | `/course/:courseGuid/lesson/:lessonGuid/power` | P2 | [ ] | |
| Agregar contenido | `/course/:courseGuid/lesson/:lessonGuid/new-content` | P2 | [ ] | |
| Editar contenido | `/course/:courseGuid/lesson/:lessonGuid/edit/:itemGuid` | P2 | [ ] | |

### Evaluaciones

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Crear evaluacion | `/new-assessment` | P1 | [ ] | |
| Ver resultados evaluacion | `/assessment-results/:guid` | P1 | [ ] | |
| Resultados SCORM | `/scorm-results/:guid` | P3 | [ ] | |
| Resultados xAPI | `/xapi-results/:guid` | P3 | [ ] | |
| Resultados videoleccion | `/videolesson-results/:guid` | P3 | [ ] | |

### Libro de Calificaciones

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Libro de calificaciones | `/course/:guid/grades` | P1 | [ ] | |
| Calificar tareas | `/course/:guid/grades` | P1 | [ ] | |
| Exportar calificaciones | `/course/:guid/grades` | P2 | [ ] | |
| Periodos de evaluacion | `/course/:guid/grades/periods` | P2 | [ ] | |

### Gestion de Estudiantes

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Ver estudiantes del aula | `/classroom-users/:guid` | P1 | [ ] | |
| Importar estudiantes | `/classroom-users/:guid` | P2 | [ ] | |

### Comunicacion

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Muro social (Stream) | `/course/:guid` (tab Stream) | P1 | [ ] | |
| Publicar mensaje | `/course/:guid` (tab Stream) | P1 | [ ] | |

### Calendario

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Calendario general | `/calendar` | P2 | [ ] | |
| Calendario del curso | `/calendar/:course` | P2 | [ ] | |

### Tareas

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Gestor de tareas | `/tasks` | P2 | [ ] | |
| Vista Kanban | `/kanban/:guid` | P3 | [ ] | |

### Herramientas de IA (si aplica)

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Herramientas de IA | `/ai/tools` | P3 | [ ] | |
| Generador de planes | `/ai/tools/setup/lesson-plan-generator` | P3 | [ ] | |
| Generador de rubricas | `/ai/tools/setup/rubric-generator` | P3 | [ ] | |
| Generador de preguntas | `/ai/tools/setup/question-generator` | P3 | [ ] | |

### Proyeccion en Aula

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Proyeccion Mint | `/projection-mint/:guid` | P3 | [ ] | |
| Proyeccion Power | `/projection-power/:guid` | P3 | [ ] | |

---

## FRONTOFFICE - ESTUDIANTES

### Mis Aulas

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Ver mis aulas | `/home` | P1 | [ ] | |

### Curso/Aula

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Acceder al curso | `/course/:guid` | P1 | [ ] | |
| Stream (muro social) | `/course/:guid` (tab Stream) | P1 | [ ] | |
| Programa del curso | `/course/:guid` (tab Program) | P1 | [ ] | |
| Mis calificaciones | `/course/:guid` (tab Grades) | P1 | [ ] | |

### Contenidos

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Ver leccion | `/course/:courseGuid/lesson/:lessonGuid` | P1 | [ ] | |
| Acceder a recursos | `/resource/:guid` | P1 | [ ] | |
| Biblioteca | `/library` | P2 | [ ] | |

### Evaluaciones

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Realizar evaluacion | `/answer-test/:guid` | P1 | [ ] | |
| Ver resultados | (desde Stream/Grades) | P1 | [ ] | |

### Tareas

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Ver tareas pendientes | `/tasks` | P1 | [ ] | |
| Entregar tarea | (desde leccion) | P1 | [ ] | |

### Comunicacion

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Participar en muro social | `/course/:guid` (tab Stream) | P2 | [ ] | |

### Calendario

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Ver calendario | `/calendar` | P2 | [ ] | |

### Perfil

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Ver/editar perfil | `/profile` | P2 | [ ] | |

---

## ADMIN ESCUELA

*Por documentar - Requiere exploracion adicional*

| Funcionalidad | Ruta | Prioridad | Estado | Archivo |
|--------------|------|-----------|--------|---------|
| Dashboard | `/` | P1 | [ ] | |
| Gestion de usuarios | | P1 | [ ] | |
| Reportes | | P2 | [ ] | |

---

## RESUMEN ESTADISTICO

| Aplicacion | Total | P1 | P2 | P3 | P4 | Documentado |
|------------|-------|----|----|----|----|-------------|
| Backoffice | 45 | 12 | 18 | 12 | 3 | 0 |
| Frontoffice Docente | 35 | 15 | 12 | 8 | 0 | 0 |
| Frontoffice Estudiante | 15 | 10 | 4 | 1 | 0 | 0 |
| Admin Escuela | 3+ | - | - | - | - | 0 |
| **TOTAL** | **98+** | **37** | **34** | **21** | **3** | **0** |

---

## PLAN DE DOCUMENTACION SUGERIDO

### Fase 1 - Funcionalidades Core (P1)
Documentar primero las funcionalidades criticas que usan todos los usuarios diariamente.

**Orden sugerido:**
1. Docentes: Mis aulas, Vista del curso, Calificaciones
2. Estudiantes: Mis aulas, Acceder al curso, Realizar evaluacion
3. Backoffice: Usuarios, Licencias, Escuelas, Programas

### Fase 2 - Funcionalidades Frecuentes (P2)
Una vez completada la documentacion P1, continuar con P2.

### Fase 3 - Funcionalidades Complementarias (P3-P4)
Documentar segun demanda y feedback de clientes.

---

*Ultima actualizacion: 2024-12-18*
