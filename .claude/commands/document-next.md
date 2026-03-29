Selecciona y documenta la siguiente feature pendiente del inventario según prioridad.

## Proceso

### 1. Leer inventario

Lee todos los archivos de inventario en `inventario/*.json` (backoffice.json, frontoffice-teacher.json, frontoffice-student.json, admin-school.json).

### 2. Seleccionar siguiente feature

Filtra las features con `status: "pending"` y ordénalas por:
1. **Prioridad**: P1 > P2 > P3 > P4
2. **App**: backoffice > frontoffice-teacher > frontoffice-student > admin-school (salvo que el usuario haya indicado preferencia)

Muestra al usuario la feature seleccionada:

```
Siguiente feature pendiente:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
App:        backoffice
Key:        collections/list
Nombre:     Listar colecciones
Prioridad:  P2
Complejidad: simple-list
Sección:    Colecciones
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Busca también features relacionadas pendientes en la misma sección (ej: si selecciona `collections/list`, incluye también `collections/create` y `collections/detail` si están pendientes).

### 3. Ejecutar documentación

Ejecuta el proceso completo de `/document-feature` con la feature seleccionada, incluyendo las features relacionadas de la misma sección.
