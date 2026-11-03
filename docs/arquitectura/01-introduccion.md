# 1. Introducción y objetivos

## Qué es HerramientaJB

Una aplicación web para un **taller electromecánico** que vende y usa repuestos y herramientas. Reemplaza el control manual del inventario por un sistema donde cada unidad que entra o sale queda registrada.

## Objetivos

| # | Objetivo                                                                 | Cómo se cumple                                                   |
|---|--------------------------------------------------------------------------|------------------------------------------------------------------|
| 1 | Saber cuánto stock hay de cada producto, en todo momento                 | Campo `cantidad` en `productos`, que solo cambia por movimientos |
| 2 | Avisar antes de quedarse sin repuestos                                   | Alertas automáticas cuando `cantidad <= stock_minimo`            |
| 3 | Tener trazabilidad de cada cambio de stock                               | Tabla `movimiento_inventario` con tipo, motivo y fecha           |
| 4 | Cotizar y vender sin errores de precio                                   | Los totales se calculan en el servidor con precios de la BD      |
| 5 | Conocer la ganancia de cada venta                                        | `ganancia_total = (precio_venta − precio_compra) × cantidad`     |
| 6 | Llevar la hoja de vida y el mantenimiento de las máquinas del taller     | Tablas `maquinas`, `hoja_vidas` y `mantenimientos`               |
| 7 | Saber quién hizo cada acción                                             | Tabla `historial_actividades`                                    |
| 8 | Que cada persona vea solo lo que le corresponde                          | Dos roles y el middleware `VerificarRol`                         |

## Quién lo usa

| Persona            | Interés principal                                                          |
|--------------------|----------------------------------------------------------------------------|
| **Administrador**  | Controlar compras, ventas, ganancias, usuarios y ver todo el historial.    |
| **Técnico**        | Sacar repuestos para reparar, consultar stock y mantener las máquinas.     |
| **Equipo de desarrollo** | Entender el código para mantenerlo y ampliarlo.                      |
