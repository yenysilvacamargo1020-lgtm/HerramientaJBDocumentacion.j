# ADR-0003 · Borrado lógico

**Estado:** Aceptada

## Contexto

Si se borra un producto, un usuario o una máquina de verdad, el historial, las ventas y los movimientos quedan apuntando a algo que ya no existe. Además, un borrado accidental no se podría deshacer.

## Decisión

Todas las tablas del negocio tienen `deleted_at` (`$table->softDeletes()`) y sus modelos usan el trait `SoftDeletes`. «Eliminar» solo marca la fila.

Las relaciones que se muestran en historiales usan `withTrashed()` (por ejemplo `Venta::usuario()` o `Entrada::producto()`), para que se siga viendo el nombre aunque el registro esté eliminado.

Las validaciones `exists` incluyen `deleted_at,NULL` para no aceptar registros eliminados.

## Consecuencias

- ✅ No se pierde información histórica.
- ✅ Un registro se puede restaurar desde la base de datos.
- ⚠️ La base de datos solo crece. Hay que pensar en una limpieza periódica.
- ⚠️ El correo de un usuario eliminado sigue ocupado, porque `correo` es `unique` y la fila existe.
