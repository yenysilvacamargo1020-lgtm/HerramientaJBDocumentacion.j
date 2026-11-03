# Agregar una operación a la API

Ejemplo: `GET /api/productos/stock-bajo`, que devuelve los productos en o por debajo de su mínimo.

## 1. Método en el servicio

`app/Services/ProductoService.php`:

```php
public function stockBajo()
{
    return \App\Models\Producto::stockBajo()->get();
}
```

`stockBajo()` es un *scope* que ya existe en el modelo `Producto`.

## 2. Método en el controlador

`app/Http/Controllers/ProductoController.php`:

```php
public function stockBajo()
{
    return response()->json($this->productoService->stockBajo(), 200);
}
```

## 3. Ruta

En `routes/api.php`, **antes** del `apiResource` de productos (si no, `stock-bajo` se tomaría como un `{id}`):

```php
Route::get('/productos/stock-bajo', [ProductoController::class, 'stockBajo']);
Route::apiResource('productos', ProductoController::class);
```

## 4. Comprobar

```bash
php artisan route:list --path=productos
curl http://localhost:8000/api/productos/stock-bajo
```

## 5. Si la operación cambia stock

**No** escribas `cantidad` directamente. Usa `InventarioService::registrarEntrada()` o `registrarSalida()` para no romper la [trazabilidad](../decisiones/0004-stock-por-movimientos.md).
