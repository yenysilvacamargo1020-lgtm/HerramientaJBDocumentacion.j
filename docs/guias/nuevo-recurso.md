# Crear un recurso nuevo

Ejemplo: **Herramienta prestada** (`prestamos`). Sigue el mismo patrón de los otros 18 recursos.

## Los archivos que hay que crear

| #  | Archivo                                              | Contenido                                           |
|----|------------------------------------------------------|-----------------------------------------------------|
| 1  | `database/migrations/19_prestamos.php`               | La tabla (con `softDeletes()`)                      |
| 2  | `app/Models/Prestamo.php`                            | `$table`, `$primaryKey`, `$fillable`, relaciones    |
| 3  | `app/Interfaces/PrestamoInterface.php`               | `extends BaseInterface`                             |
| 4  | `app/Repositories/PrestamoRepository.php`            | `extends BaseRepository implements PrestamoInterface` |
| 5  | `app/Services/PrestamoService.php`                   | Métodos `all`, `show`, `store`, `update`, `destroy` |
| 6  | `app/Http/Requests/StorePrestamoRequest.php` y `UpdatePrestamoRequest.php` | Validaciones de la API  |
| 7  | `app/Http/Controllers/PrestamoController.php`        | Controlador de la API                               |
| 8  | `app/Providers/RepositoryServiceProvider.php`        | Un `bind` de la interfaz al repositorio             |
| 9  | `routes/api.php`                                     | `apiResource` y las rutas `PUT`/`PATCH`             |

Si además quieres pantallas, agrega la [pantalla](nueva-pantalla.md).

## Pasos clave

**Modelo** (copia el estilo de `Categoria`):

```php
class Prestamo extends Model
{
    use SoftDeletes;

    protected $table = 'prestamos';
    protected $primaryKey = 'id_prestamo';
    protected $fillable = ['id_maquina', 'prestado_a', 'fecha'];
}
```

**Enlace en el proveedor:**

```php
$this->app->bind(PrestamoInterface::class, PrestamoRepository::class);
```

**Ruta de la API:**

```php
Route::apiResource('prestamos', PrestamoController::class);
Route::put('/prestamos/{prestamo}', [PrestamoController::class, 'update']);
Route::patch('/prestamos/{prestamo}', [PrestamoController::class, 'update']);
```

!!! warning "Nombres de migración"
    Las migraciones se ejecutan por orden alfabético. Si la tabla tiene llaves foráneas, su número debe ser **mayor** que el de las tablas a las que apunta.

## Lista de comprobación

- [ ] Migración con `softDeletes()`
- [ ] Interfaz enlazada en `RepositoryServiceProvider`
- [ ] Ruta en `routes/api.php`
- [ ] `php artisan route:list --path=prestamos` muestra los endpoints
- [ ] Regenerar la documentación de la API (ver [API REST](../api.md))
