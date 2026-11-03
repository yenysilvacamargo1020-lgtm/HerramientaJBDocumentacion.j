# ADR-0002 · Dos roles controlados por middleware

**Estado:** Aceptada

## Contexto

En el taller trabajan dos tipos de personas: quien administra el negocio (compras, ventas, dinero) y quienes reparan (usan repuestos y mantienen máquinas). No hay otras funciones que necesiten un menú distinto.

## Opciones

| Opción                                         | Ventaja                         | Problema                                   |
|------------------------------------------------|---------------------------------|--------------------------------------------|
| Un paquete de permisos (por ejemplo Spatie)    | Muy flexible                    | Demasiado grande para dos roles            |
| Una columna `es_admin` en `usuarios`           | Muy simple                      | No crece si aparece un tercer rol          |
| **Tabla `roles` + middleware propio**          | Simple y ya existe la tabla     | Hay que mantener las constantes a mano     |

## Decisión

Usar la tabla `roles` y un middleware, `VerificarRol`, que recibe los roles permitidos:

```php
Route::middleware(VerificarRol::class . ':' . Rol::ADMINISTRADOR)->group(...);
Route::middleware(VerificarRol::class . ':' . Rol::ADMINISTRADOR . ',' . Rol::TECNICO)->group(...);
```

Existen solo dos roles: **Administrador** y **Técnico**. El rol se compara por *slug* (`Técnico` → `tecnico`).

## Consecuencias

- ✅ Permisos visibles en un solo archivo (`routes/web.php`).
- ✅ Agregar un rol es agregar una constante, una fila y una línea en las rutas.
- ⚠️ Las rutas de **solo administrador** deben ir **antes** que las compartidas, para que `/maquinas/create` no se confunda con `/maquinas/{id}`.
- ⚠️ El menú tiene su propia lista (`admin => true/false`) que hay que mantener alineada con las rutas.
