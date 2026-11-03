# 11. Riesgos y deuda técnica

Problemas conocidos, ordenados por importancia.

| #  | Riesgo                                                                 | Impacto | Qué hacer                                                                 |
|----|------------------------------------------------------------------------|---------|---------------------------------------------------------------------------|
| 1  | **La API REST no tiene autenticación.** `routes/api.php` no usa `auth:sanctum` ni ningún middleware, y `.scribe/auth.md` lo confirma. Cualquiera que llegue al servidor puede leer, crear, editar y borrar datos | **Alto** | Proteger las rutas con Sanctum (ya está instalado) y aplicar `VerificarRol`  |
| 2  | **La API no pasa por las reglas de negocio.** Sus controladores usan servicios CRUD genéricos: se puede editar `cantidad` de un producto directamente o crear una venta sin descontar stock | **Alto** | Reutilizar `InventarioService` y `FacturacionService` en la API, o limitarla a lectura |
| 3  | Contraseñas de ejemplo conocidas (`Admin12345`, `12345678`) y MySQL con `root` / `1234` en `docker-compose.yml` | Alto | Cambiarlas antes de cualquier uso real; no subir `.env` al repositorio    |
| 4  | `.env.example` trae `DB_CONNECTION=sqlite` y `APP_LOCALE=en`, que no coinciden con el resto de la documentación | Medio | Actualizar el ejemplo a `mysql` y `es`                                    |
| 5  | **Casi no hay pruebas automatizadas** (solo los `ExampleTest`)         | Medio   | Empezar por las cinco pruebas sugeridas en [Calidad](10-calidad.md)        |
| 6  | **Dos proveedores de servicios** (`AppServiceProvider` y `RepositoryServiceProvider`) enlazan interfaces con repositorios; el primero cubre solo 10 y el segundo las 18 | Bajo | Dejar solo `RepositoryServiceProvider`                                    |
| 7  | La carpeta `config/app/` contiene copias de `Controller.php`, `User.php` y `AppServiceProvider.php` que no se usan | Bajo | Eliminarla                                                                |
| 8  | La tabla `sessions` se creó con `softDeletes()`, que Laravel no necesita ahí | Bajo | Quitarlo en una migración nueva si da problemas                            |
| 9  | Las ventas no tienen tabla de detalle: las líneas se reconstruyen buscando `Venta #id` en el texto de `movimiento_inventario.motivo` | Medio | Crear una tabla `detalle_ventas` (hay una interfaz `DetalleVentaInterface` sin usar) |
| 10 | Rutas API con parámetros con guion (por ejemplo `{movimiento-inventario}`) mientras los controladores reciben `$id_movimiento` | Medio | Revisar con `php artisan route:list` y probar cada `PUT`/`PATCH`          |
| 11 | Algunos controladores de la API reciben `$id_movimiento` en métodos de otros recursos (por ejemplo, `ConfiguracionAlertaController::update`) | Bajo | Renombrar el parámetro para que coincida con el recurso                   |
| 12 | Cuatro tablas solo se pueden usar desde la API: `productos_favoritos`, `catalogo_busquedas`, `configuracion_alertas` y `registros_conectividad` | Bajo | Crear pantallas o retirarlas si no se van a usar                           |
| 13 | No hay recuperación de contraseña ni cambio de contraseña propio       | Medio   | Hoy solo un administrador puede cambiarla desde **Usuarios**              |

!!! tip "Cómo usar esta tabla"
    Antes de entregar o desplegar, resuelve al menos los riesgos 1 a 3. Los demás pueden ir en el tablero de tareas.
