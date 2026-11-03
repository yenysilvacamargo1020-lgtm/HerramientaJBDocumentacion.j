# Agregar un campo

Ejemplo: agregar `ubicacion` (el estante donde está el producto) a **productos**.

## 1. Migración nueva

No edites las migraciones antiguas. Crea una nueva:

```bash
php artisan make:migration agregar_ubicacion_a_productos --table=productos
```

```php
public function up(): void
{
    Schema::table('productos', function (Blueprint $table) {
        $table->string('ubicacion', 100)->nullable()->after('marca');
    });
}

public function down(): void
{
    Schema::table('productos', function (Blueprint $table) {
        $table->dropColumn('ubicacion');
    });
}
```

```bash
php artisan migrate
```

## 2. Lista de campos del modelo

En `app/Models/Producto.php`:

```php
protected $fillable = [
    'nombre', 'descripcion', 'marca', 'ubicacion', /* ... */
];
```

## 3. Validación

Hay **tres** archivos según por dónde entre el dato:

| Archivo                                       | Para…                       |
|-----------------------------------------------|-----------------------------|
| `app/Http/Requests/ProductoRequest.php`       | Formularios de la web       |
| `app/Http/Requests/StoreProductoRequest.php`  | Crear por la API            |
| `app/Http/Requests/UpdateProductoRequest.php` | Editar por la API           |

```php
'ubicacion' => ['nullable', 'string', 'max:100'],
```

## 4. Pantallas

- Formulario: `resources/views/productos/form.blade.php`
- Listado: `resources/views/productos/index.blade.php`

## 5. Datos de ejemplo (opcional)

Agrega el campo en `database/seeders/DatabaseSeeder.php`.

## Lista de comprobación

- [ ] Migración creada y ejecutada
- [ ] `$fillable` actualizado
- [ ] Los tres `Request` actualizados
- [ ] Formulario y listado muestran el campo
- [ ] Probado creando y editando desde la web
