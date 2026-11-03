# 12. Glosario

| Término                 | Significado                                                                                      |
|-------------------------|--------------------------------------------------------------------------------------------------|
| **Alerta de inventario**| Aviso automático que se crea cuando un producto llega a su stock mínimo.                         |
| **Borrado lógico**      | «Eliminar» un registro marcándolo con `deleted_at`, sin quitarlo de la base de datos.            |
| **Cotización**          | Lista de productos con precios y total para un cliente. **No toca el inventario.**               |
| **Entrada**             | Compra a un proveedor. Suma stock.                                                               |
| **Eloquent**            | Herramienta de Laravel para trabajar con tablas como si fueran objetos (los *Models*).           |
| **Ganancia**            | Por producto: `(precio_venta − precio_compra) × cantidad`. La venta guarda la suma.              |
| **Hoja de vida**        | Ficha técnica de una máquina: fecha de ingreso y especificaciones.                               |
| **Historial**           | Registro de acciones hechas por cada usuario.                                                    |
| **Mantenimiento**       | Intervención sobre una máquina. Puede ser **preventivo** (revisión) o **correctivo** (reparación). |
| **Middleware**          | Filtro que se ejecuta antes del controlador. Aquí revisa el rol.                                 |
| **Migración**           | Archivo PHP que crea o modifica una tabla.                                                       |
| **Movimiento**          | Registro de una `ENTRADA` o `SALIDA` de stock, con fecha y motivo.                               |
| **Repositorio**         | Clase que concentra las consultas a la base de datos de un recurso.                              |
| **Salida**              | Stock que sale por reparación, daño, venta u otro motivo. Resta stock.                           |
| **Scribe**              | Paquete que genera la documentación de la API a partir de las rutas.                             |
| **Seeder**              | Clase que llena la base de datos con datos iniciales o de ejemplo.                               |
| **Servicio**            | Clase con la lógica de negocio (por ejemplo, `InventarioService`).                               |
| **Slug**                | Texto normalizado: `Técnico` → `tecnico`.                                                        |
| **Stock mínimo**        | Cantidad a partir de la cual el sistema avisa que hay que reponer.                               |
| **Técnico**             | Rol que registra salidas, consulta stock y mantiene las máquinas.                                |
| **Trazabilidad**        | Poder rastrear qué se movió, cuándo, por qué y quién lo hizo.                                    |
