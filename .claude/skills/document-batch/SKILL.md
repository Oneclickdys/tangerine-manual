---
name: document-batch
description: Documenta todas las features pendientes de una aplicación delegando cada una a un subagente. Uso: /document-batch frontoffice-teacher
---

Documenta todas las features pendientes de una aplicación, delegando cada una a un subagente para no saturar el contexto del coordinador.

**Input:** Nombre de app = $ARGUMENTS (ej: `frontoffice-teacher`, `backoffice`, `frontoffice-student`, `admin-school`). Si no se proporciona, pregunta al usuario.

## Proceso

### 1. Construir la cola de trabajo

Lee el inventario de la app indicada (`inventario/{app}.json`). Filtra features con `status: "pending"`.

Agrupa features relacionadas que pertenezcan a la **misma sección** (usa `relatedFeatures` y `section` para agrupar). Cada grupo se documenta en una sola invocación de subagente.

Ordena los grupos por prioridad (P1 > P2 > P3 > P4).

Muestra al usuario la cola completa antes de empezar:

```
Cola de documentación para {app}:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. [P1] gradebook/main — Libro de calificaciones (complex-workflow)
2. [P2] course/resources — Recursos del curso (simple-list)
3. [P2] lesson/power-view — Vista Power (interactive-view)
4. [P2] calendar/view — Calendario general (interactive-view)
5. [P2] tasks/list — Gestor de tareas (simple-list)
6. [P3] ai/tools — Herramientas de IA (interactive-view)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total: 6 grupos, 7 features
```

Espera confirmación del usuario antes de continuar. El usuario puede:
- Confirmar para documentar todas
- Indicar un rango (ej: "solo las P1" o "de la 1 a la 3")
- Excluir features específicas

### 2. Iterar por cada grupo

Para cada grupo en la cola, lanza **un subagente** (usando la herramienta Agent) con el siguiente prompt. Es CRÍTICO que el prompt del subagente sea autocontenido — incluye toda la información que necesita:

```
Documenta la(s) siguiente(s) feature(s) del inventario de Tangerine LMS.

## Feature(s) a documentar
{JSON completo de cada feature del grupo, copiado del inventario}

## App
{nombre de la app y su baseUrl del inventario/index.json}

## Instrucciones
Sigue EXACTAMENTE el proceso de /document-feature:

1. **Exploración de código**: Explora los codeFiles en el monorepo (/Users/camilolopez/DEVS/repos/tangerine-global/). Extrae campos, obligatoriedad, acciones, permisos, flags, variantes.

2. **Screenshots**: Lee credenciales de /Users/camilolopez/DEVS/repos/tangerine-global/manual/.credentials.json. Usa Chrome DevTools MCP para navegar, autenticarte y capturar screenshots. Guarda en assets/screenshots/{app}/{feature-slug}/.

3. **Decidir estructura**: Si la feature es demasiado grande para el rango de páginas de su complexity, subdivídela en subsecciones (ver _guidelines/AGENT_GUIDELINES.md sección 2).

4. **Generar MDX**: Lee _guidelines/AGENT_GUIDELINES.md y _templates/PLANTILLA.mdx. Genera las páginas MDX siguiendo las directrices. Tercera persona descriptiva, tono Stripe/Twilio, acentos correctos. NO incluir validaciones técnicas (límites de caracteres, min/max, debounce) — solo restricciones de negocio relevantes.

5. **Actualizar navegación e inventario**:
   - Actualiza docs.json con las nuevas páginas
   - Actualiza inventario/{app}.json: status="documented", docFile, lastDocumented="{fecha de hoy}"
   - Actualiza inventario/index.json contadores

6. **Devuelve un resumen** con: páginas creadas, screenshots capturados, features documentadas.

NO hagas commit. Solo genera archivos y actualiza inventario/navegación.
```

**IMPORTANTE para el coordinador:**
- Usa `subagent_type: "general-purpose"` para cada subagente
- Espera a que cada subagente termine antes de lanzar el siguiente (secuencial)
- El inventario se actualiza dentro de cada subagente, así el siguiente lee el estado correcto
- Si un subagente falla o reporta un problema, anótalo y continúa con el siguiente

### 3. Progreso

Después de cada subagente, muestra el progreso:

```
✓ [1/6] gradebook/main — 3 páginas, 5 screenshots
✓ [2/6] course/resources — 1 página, 2 screenshots
⏳ [3/6] lesson/power-view — en curso...
```

### 4. Resumen final

Al terminar toda la cola, muestra:

```
Documentación completada para {app}:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Features documentadas: 7
Páginas creadas: 12
Screenshots capturados: 25
Features con problemas: 0
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

NO hagas commit automáticamente. El usuario decidirá cuándo hacer commit.
