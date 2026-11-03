# API REST

!!! danger "La API no tiene autenticación"
    Hoy cualquier persona que llegue al servidor puede usar estos endpoints para leer, crear, editar y borrar datos. **No la publiques en internet** sin protegerla antes. Ver [Riesgos y deuda técnica](arquitectura/11-riesgos.md).

## Cómo funciona

- Base: `http://localhost:8000/api`
- Formato: JSON. Envía `Accept: application/json` para recibir los errores de validación como JSON.
- Cada recurso es un `apiResource` de Laravel con sus cinco operaciones, más las rutas `PUT` y `PATCH` para editar.
- Los identificadores son los de cada tabla (`id_producto`, `id_venta`…).

## Operaciones de cada recurso

| Método          | Ruta                  | Qué hace                | Respuesta típica       |
|-----------------|-----------------------|-------------------------|------------------------|
| `GET`           | `/api/{recurso}`      | Lista todos             | `200` y un arreglo     |
| `POST`          | `/api/{recurso}`      | Crea uno                | `201` con `message` y `data` |
| `GET`           | `/api/{recurso}/{id}` | Muestra uno             | `200`                  |
| `PUT` / `PATCH` | `/api/{recurso}/{id}` | Actualiza               | `200` con `message` y `data` |
| `DELETE`        | `/api/{recurso}/{id}` | Elimina (borrado lógico)| `200` con `message`    |

## Recursos disponibles

| Recurso                  | Ruta base                        | Tabla                    |
|--------------------------|----------------------------------|--------------------------|
| Usuarios                 | `/api/usuarios`                  | `usuarios`               |
| Roles                    | `/api/roles`                     | `roles`                  |
| Historial de actividades | `/api/historial-actividades`     | `historial_actividades`  |
| Categorías               | `/api/categorias`                | `categorias`             |
| Proveedores              | `/api/proveedores`               | `proveedores`            |
| Productos                | `/api/productos`                 | `productos`              |
| Entradas                 | `/api/entradas`                  | `entradas`               |
| Movimientos de inventario| `/api/movimientos-inventario`    | `movimiento_inventario`  |
| Configuración de alertas | `/api/configuracion-alertas`     | `configuracion_alertas`  |
| Alertas de inventario    | `/api/alertas-inventario`        | `alertas_inventario`     |
| Catálogo de búsquedas    | `/api/catalogo-busquedas`        | `catalogo_busquedas`     |
| Productos favoritos      | `/api/productos-favoritos`       | `productos_favoritos`    |
| Cotizaciones             | `/api/cotizaciones`              | `cotizaciones`           |
| Ventas                   | `/api/ventas`                    | `ventas`                 |
| Máquinas                 | `/api/maquinas`                  | `maquinas`               |
| Hojas de vida            | `/api/hojas-vida`                | `hoja_vidas`             |
| Mantenimientos           | `/api/mantenimientos`            | `mantenimientos`         |
| Registros de conectividad| `/api/registros-conectividad`    | `registros_conectividad` |

## Ejemplo

```bash
curl -X POST http://localhost:8000/api/categorias \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{"nombre": "Soldadura", "descripcion": "Electrodos y accesorios"}'
```

```json
{
  "message": "Categoría creada correctamente",
  "data": { "id_categoria": 7, "nombre": "Soldadura", "descripcion": "Electrodos y accesorios" }
}
```

!!! note
    El texto exacto de `message` depende de cada controlador.

## Documentación automática (Scribe)

El proyecto usa [Scribe](https://scribe.knuckles.wtf) para generar la referencia completa de campos y ejemplos de cada endpoint.

```bash
php artisan scribe:generate
```

Con la aplicación encendida, la referencia queda en **<http://localhost:8000/docs>**. La configuración está en `config/scribe.php` y los textos de apoyo en `.scribe/`.

!!! warning
    La página `/docs` no pide sesión (`middleware` vacío en `config/scribe.php`). Protégela o desactívala fuera de desarrollo.

## Esta API frente a la interfaz web

| Tema                        | Interfaz web                     | API REST                            |
|-----------------------------|----------------------------------|-------------------------------------|
| Autenticación               | Sesión + roles                   | Ninguna                             |
| Descuenta stock al vender   | Sí, con transacción              | No, es un CRUD simple               |
| Calcula precios             | En el servidor                   | Recibe lo que se envíe              |
| Deja huella en el historial | Sí                               | No                                  |
