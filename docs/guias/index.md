# ¿Dónde hago el cambio?

Mapa rápido para saber **qué archivos tocar** según lo que quieras cambiar.

| Quiero…                                                    | Guía                                                      |
|------------------------------------------------------------|-----------------------------------------------------------|
| Agregar una columna a una tabla que ya existe              | [Agregar un campo](agregar-campo.md)                      |
| Crear una tabla y su CRUD completo                         | [Crear un recurso nuevo](nuevo-recurso.md)                |
| Agregar un endpoint o acción a la API                      | [Agregar una operación a la API](nueva-operacion.md)      |
| Crear una pantalla nueva en la interfaz                    | [Crear una pantalla](nueva-pantalla.md)                   |
| Cambiar qué ve o hace cada rol                             | [Cambiar permisos o roles](permisos.md)                   |
| Cambiar cómo se calcula algo (ganancia, alertas, stock)    | [Cambiar una regla de negocio](regla-de-negocio.md)       |

## Dónde vive cada cosa

| Si cambia…                       | Toca…                                                                 |
|----------------------------------|-----------------------------------------------------------------------|
| La estructura de una tabla       | `database/migrations/`                                                |
| Qué campos se pueden guardar     | `$fillable` en `app/Models/`                                          |
| Qué datos se aceptan             | `app/Http/Requests/`                                                  |
| Una regla del inventario o ventas| `app/Services/InventarioService.php` o `FacturacionService.php`       |
| Una consulta                     | `app/Repositories/` o el controlador web                              |
| Una pantalla                     | `resources/views/<módulo>/`                                           |
| Un enlace del menú               | `resources/views/layouts/app.blade.php`                               |
| Quién puede entrar               | `routes/web.php`                                                      |
| Colores y estilos                | `resources/css/app.css`                                               |
| Los datos de ejemplo             | `database/seeders/DatabaseSeeder.php`                                 |

!!! warning "Regla de oro"
    Después de cualquier cambio de código de rutas o configuración ejecuta `php artisan optimize:clear`.
