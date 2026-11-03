# Crear una pantalla

Ejemplo: una pantalla de **solo lectura** que lista las categorías.

## 1. Controlador web

`app/Http/Controllers/Web/` (copia el estilo de `CategoriaController`):

```php
public function index(): View
{
    return view('categorias.index', [
        'categorias' => Categoria::orderBy('nombre')->paginate(15),
    ]);
}
```

Si la acción cambia datos, llama a `RegistroActividad::registrar("...")` para dejar huella en el historial.

## 2. Vista

`resources/views/categorias/index.blade.php`. Extiende el diseño general:

```blade
@extends('layouts.app')
@section('titulo', 'Categorías')

@section('contenido')
    <x-encabezado titulo="Categorías" />
    {{-- tu contenido --}}
@endsection
```

Componentes y parciales disponibles:

| Pieza                                  | Para qué                                   |
|----------------------------------------|--------------------------------------------|
| `<x-encabezado>`                       | Título de la pantalla                      |
| `<x-dato>`                             | Un dato con etiqueta                       |
| `<x-vacio>`                            | Mensaje cuando no hay registros            |
| `<x-icono nombre="...">`               | Iconos                                     |
| `partials/mensajes`                    | Avisos de éxito y de error                 |
| `partials/paginacion`                  | Paginador                                  |
| `partials/lineas-productos`            | Filas de productos en cotizaciones y ventas|

## 3. Ruta

En `routes/web.php`, dentro del grupo del rol que corresponda (ver [permisos](permisos.md)):

```php
Route::get('/categorias', [CategoriaController::class, 'index'])->name('categorias.index');
```

## 4. Enlace en el menú

En el arreglo `$menu` de `resources/views/layouts/app.blade.php`:

```php
['ruta' => 'categorias.index', 'activo' => 'categorias.*', 'texto' => 'Categorías', 'icono' => 'categoria', 'admin' => true],
```

`'admin' => true` oculta el enlace a los técnicos.

## 5. Estilos

Las clases propias (`panel`, `btn`, `btn-primario`, `nav-link`, `franja-peligro`…) están en `resources/css/app.css`. Recuerda tener `npm run dev` encendido.
