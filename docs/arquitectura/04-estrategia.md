# 4. Estrategia de solución

| Reto                                                  | Idea de solución                                                                       | Dónde está                                     |
|-------------------------------------------------------|----------------------------------------------------------------------------------------|------------------------------------------------|
| Que el stock nunca se descuadre                       | El stock **no se edita a mano**: solo cambia con entradas, salidas y ventas            | `InventarioService`                            |
| Que una venta no deje datos a medias                  | Transacción de base de datos: si un producto no alcanza, no se guarda nada             | `FacturacionService::registrarVenta`           |
| Que nadie altere un precio desde el navegador         | El servidor ignora precios del formulario y usa los de la base de datos                | `FacturacionService::calcular`                 |
| Que dos personas no saquen el mismo stock a la vez    | Bloqueo de fila con `lockForUpdate()` dentro de la transacción                          | `InventarioService`                            |
| Avisar a tiempo de la falta de repuestos              | Se crea una alerta al llegar al mínimo, sin duplicar las no leídas                     | `InventarioService::verificarStockMinimo`      |
| Cada persona ve solo lo suyo                          | Middleware `VerificarRol` en grupos de rutas + menú filtrado por rol                   | `routes/web.php`, `layouts/app.blade.php`      |
| No perder información al "eliminar"                   | Borrado lógico en todas las tablas                                                     | Trait `SoftDeletes` en los modelos             |
| Saber quién hizo qué                                  | Cada acción importante llama a `RegistroActividad::registrar()`                        | `app/Services/RegistroActividad.php`           |
| Mantener el código ordenado                           | Capas: Controlador → Servicio → Repositorio → Modelo (API); servicios de negocio compartidos | [ADR-0001](../decisiones/0001-arquitectura-en-capas.md) |

## Dos entradas, una misma base

El sistema tiene **dos formas de entrar** a los datos:

1. **Interfaz web** (`routes/web.php`, controladores en `app/Http/Controllers/Web/`): usada por las personas, con sesión y roles.
2. **API REST** (`routes/api.php`, controladores en `app/Http/Controllers/`): un CRUD genérico por cada tabla, pensado para integraciones y pruebas.

Las dos escriben en las mismas tablas. Ver [ADR-0006](../decisiones/0006-web-y-api-separadas.md).
