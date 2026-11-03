# Cambiar una regla de negocio

Las reglas importantes están concentradas en dos servicios. **No las repitas en controladores ni vistas.**

| Regla                                              | Dónde está                                         |
|----------------------------------------------------|----------------------------------------------------|
| Cómo se suma y resta el stock                      | `InventarioService::registrarEntrada` y `registrarSalida` |
| Cuándo se crea una alerta                          | `InventarioService::verificarStockMinimo`          |
| Cómo se calcula total y ganancia                   | `FacturacionService::calcular`                     |
| Qué pasa al registrar una venta                    | `FacturacionService::registrarVenta`               |
| Precio de venta mínimo                             | `rules()` de `ProductoRequest`: `gte:precio_compra`|
| Fecha máxima de una venta                          | `rules()` de `VentaRequest`: `before_or_equal:today`|
| Intentos de login por minuto                       | `routes/web.php`: `throttle:5,1`                   |
| Mensaje de stock insuficiente                      | `StockInsuficienteException::para`                 |

## Ejemplos

### La alerta se debe crear antes de llegar al mínimo

Hoy salta cuando `cantidad <= stock_minimo` (`Producto::tieneStockBajo()` y el *scope* `stockBajo`). Para avisar con un margen, cambia **las dos** condiciones; si no, el Panel y las alertas no coincidirán.

### Aplicar un descuento en la venta

1. Agrega `descuento` a `ventas` con una [migración nueva](agregar-campo.md).
2. Recíbelo en `VentaRequest`.
3. Aplícalo dentro de `FacturacionService::calcular`, **antes** de calcular la ganancia.
4. Muestra el descuento en `ventas/show.blade.php` y en `partials/comprobante.blade.php`.

### Que las ventas permitan stock negativo

No lo hagas: rompe la trazabilidad. Si es necesario, escribe un ADR nuevo antes (ver [plantilla](../decisiones/plantilla.md)).

## Lista de comprobación

- [ ] El cambio está en el servicio, no en el controlador
- [ ] Si afecta dinero, se calcula en el servidor
- [ ] Sigue dentro de una transacción si toca varias tablas
- [ ] Se deja una línea en el historial con `RegistroActividad::registrar`
- [ ] Probado con stock suficiente **y** con stock insuficiente
- [ ] Se escribió un ADR si la decisión es difícil de revertir
