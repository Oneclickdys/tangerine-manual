# Hallazgos de usabilidad pendientes de revisión

Listado de problemas de usabilidad detectados durante la documentación del producto. Cada entrada describe el comportamiento actual, por qué es problemático y una posible mejora. Revisar con Product/UX para decidir si se corrige, se documenta como limitación, o se acepta.

---

## Vista Power — Botón de edición de contenidos del docente

**Ubicación:**
- Vista Power > Barra lateral > Cabecera
- `tangerine-frontoffice/src/core/lite/views/LessonViewPowerV2/`

**Comportamiento actual:**
El botón para editar/reorganizar los contenidos creados por el docente usa un **icono de filtros**, lo cual no comunica claramente que su función es activar el modo de edición (mover, editar, eliminar contenidos). Además, el botón **solo aparece si existen contenidos creados por el docente** (`is_customized = true`), lo que significa que es invisible hasta que el docente haya creado al menos un contenido propio.

**Por qué es problemático:**
1. **Icono confuso**: un icono de filtros sugiere filtrar u ordenar la lista, no entrar en modo de edición. El docente puede no descubrir esta funcionalidad o confundirla con otra cosa.
2. **Aparición condicional sin indicación**: el botón aparece y desaparece según el estado de los contenidos. Un docente que acaba de crear su primer contenido no tiene forma de saber que ahora dispone de un nuevo control en la cabecera, ni un docente sin contenidos propios sabe que esa funcionalidad existe.

**Posible mejora:**
- Cambiar el icono a uno que sugiera edición (lápiz, engranaje, o similar).
- Mostrar siempre el botón pero deshabilitado (con tooltip explicativo) cuando no hay contenidos del docente, para que el usuario sepa que la funcionalidad existe.

**Fecha de detección:** 2026-03-29

---

## Cómo usar este documento

Al documentar features, si se detectan problemas de usabilidad (iconos confusos, flujos poco intuitivos, estados ocultos, inconsistencias visuales), añadir una entrada con:

1. **Título** descriptivo
2. **Ubicación** en la UI y archivos de código
3. **Comportamiento actual** explicando qué ocurre
4. **Por qué es problemático** desde la perspectiva del usuario
5. **Posible mejora** con sugerencia concreta
6. **Fecha de detección**
