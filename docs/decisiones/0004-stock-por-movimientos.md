# ADR-0004 · El stock solo cambia por movimientos

**Estado:** Aceptada

## Contexto

Si cualquiera puede escribir el número de unidades en el formulario de un producto, el inventario se descuadra y nadie sabe por qué.

## Decisión

El campo `productos.cantidad` **no se edita a mano**. Solo cambia mediante `InventarioService`:

| Operación            | Efecto en `cantidad` | Registro que deja                         |
|----------------------|----------------------|-------------------------------------------|
| Entrada (compra)     | Suma                 | `entradas` + movimiento `ENTRADA`         |
| Salida (reparación, daño…) | Resta          | Movimiento `SALIDA`                       |
| Venta                | Resta                | Movimiento `SALIDA` con motivo `Venta #id`|
| Producto nuevo con stock | —                | Movimiento `ENTRADA` «Inventario inicial» |

Cada operación corre en una **transacción** con `lockForUpdate()` sobre el producto. En el formulario de edición del producto, la regla de validación de `cantidad` solo existe al crear.

## Consecuencias

- ✅ Trazabilidad completa: siempre se sabe de dónde salió cada unidad.
- ✅ No hay descuadres por dos personas trabajando a la vez.
- ⚠️ Corregir un error exige una salida o entrada de ajuste, no editar el número.
- ⚠️ La API genérica todavía permite saltarse esta regla. Ver [Riesgos](../arquitectura/11-riesgos.md).
