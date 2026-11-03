# Cambiar permisos o roles

## Quién puede qué

| Módulo                                   | Administrador | Técnico |
|------------------------------------------|:-------------:|:-------:|
| Panel                                    | ✅ con ventas y ganancias | ✅ sin datos de dinero |
| Ver productos y stock                    | ✅ | ✅ |
| Crear, editar y eliminar productos       | ✅ | ❌ |
| Categorías y proveedores                 | ✅ | ❌ |
| Registrar entradas (compras)             | ✅ | ❌ |
| Registrar salidas de repuestos           | ✅ | ✅ |
| Ver movimientos                          | ✅ | ✅ |
| Alertas de stock mínimo                  | ✅ | ✅ |
| Ver máquinas y hoja de vida              | ✅ | ✅ |
| Crear, editar y eliminar máquinas        | ✅ | ❌ |
| Editar hoja de vida                      | ✅ | ✅ |
| Registrar y editar mantenimientos        | ✅ | ✅ |
| Eliminar mantenimientos                  | ✅ | ❌ |
| Cotizaciones y ventas                    | ✅ | ❌ |
| Usuarios e historial                     | ✅ | ❌ |

## Cómo está armado en el código

`routes/web.php` tiene dos grupos de rutas con sesión:

```php
// Solo administrador
Route::middleware(VerificarRol::class . ':' . Rol::ADMINISTRADOR)->group(...);

// Administrador y técnico
Route::middleware(VerificarRol::class . ':' . Rol::ADMINISTRADOR . ',' . Rol::TECNICO)->group(...);
```

## Dar un permiso a los técnicos

Ejemplo: que los técnicos puedan **ver las entradas** (`/entradas`).

1. En `routes/web.php`, mueve `Route::get('/entradas', ...)` del grupo de solo administrador al grupo compartido.
2. En `layouts/app.blade.php`, cambia `'admin' => true` por `false` en el enlace *Entradas*.
3. Si hay botones que solo el administrador debe ver dentro de la vista, protégelos con `@if(auth()->user()->esAdministrador())`.
4. `php artisan optimize:clear`.

## Quitar un permiso

Haz lo contrario: mueve la ruta al grupo de solo administrador y pon `'admin' => true` en el menú.

## Agregar un rol nuevo

1. **Constante** en `app/Models/Rol.php`: `public const BODEGUERO = 'bodeguero';`
2. **Fila** en la tabla `roles`: agrégala en `RolesYAdministradorSeeder` con `Rol::firstOrCreate(['nombre' => 'Bodeguero']);` o créala desde la base de datos. El slug debe coincidir con la constante.
3. **Rutas**: agrega la constante donde corresponda, por ejemplo `':' . Rol::ADMINISTRADOR . ',' . Rol::BODEGUERO`.
4. **Método de comodidad** (opcional) en `Usuario`: `esBodeguero()`.
5. **Menú**: si el rol necesita enlaces propios, ajusta la lógica de `$visibles` en `layouts/app.blade.php`, que hoy solo distingue entre administrador y los demás.
6. **Documentación**: actualiza esta página y [Inicio](../index.md).

!!! warning "Un usuario sin rol no entra"
    `LoginController` cierra la sesión si `usuarios.id_rol` es nulo. Los roles eliminados dejan a sus usuarios sin rol (`onDelete('set null')`).
