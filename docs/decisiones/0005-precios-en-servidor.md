# ADR-0005 · Precios y totales calculados en el servidor

**Estado:** Aceptada

## Contexto

Un formulario web puede ser manipulado desde el navegador. Si el total o el precio unitario viajan desde ahí, alguien podría vender a precio cero.

## Decisión

El formulario de venta y de cotización solo envía **producto y cantidad**. `FacturacionService::calcular()` busca los precios en la base de datos y calcula:

- `subtotal = precio_venta × cantidad`
- `total = suma de subtotales`
- `ganancia = (precio_venta − precio_compra) × cantidad`, sumada

Si el mismo producto está repetido en varias filas, las cantidades se agrupan.

La cotización guarda en `productos_seleccionados` (JSON) el **detalle completo** (nombre, cantidad, precio y subtotal) para que no cambie aunque después suba el precio del producto.

## Consecuencias

- ✅ Nadie puede alterar un precio desde el navegador.
- ✅ Cotizaciones antiguas conservan los precios con que se hicieron.
- ⚠️ Las ventas no tienen descuento ni precio especial: siempre se cobra el precio de lista.
