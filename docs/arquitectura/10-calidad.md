# 10. Calidad y pruebas

## Qué tenemos hoy

| Mecanismo                                | Qué protege                                                                  |
|------------------------------------------|------------------------------------------------------------------------------|
| Validaciones en `Request`                | Datos vacíos, tipos incorrectos, precios inválidos, referencias inexistentes |
| Transacciones de base de datos           | Ventas y salidas que no se guardan a medias                                  |
| `lockForUpdate()`                        | Dos salidas simultáneas sobre el mismo producto                              |
| `StockInsuficienteException`             | Vender o sacar más de lo que hay                                             |
| Middleware `VerificarRol`                | Acceso a pantallas según el rol                                              |
| Límite de intentos de login              | Fuerza bruta de contraseñas                                                  |
| Laravel Pint (`composer.json`)           | Estilo de código consistente                                                 |

## Pruebas automatizadas

El proyecto trae PHPUnit 11 configurado (`phpunit.xml`), pero en `tests/` solo existen los dos `ExampleTest.php` que vienen con Laravel. **Todavía no hay pruebas propias.**

```bash
php artisan test
```

### Pruebas que más valor darían

| Prueba                                                         | Qué verifica                                                   |
|----------------------------------------------------------------|----------------------------------------------------------------|
| Una salida mayor al stock lanza `StockInsuficienteException`   | La regla central del inventario                                |
| Una venta con un producto sin stock no crea ninguna fila       | La transacción hace *rollback*                                 |
| Un técnico recibe 403 en `/ventas`                             | El middleware de roles                                         |
| Al llegar al mínimo se crea **una sola** alerta                | No se duplican alertas                                         |
| El total de la venta ignora el precio enviado por el navegador | Precios calculados en el servidor                              |

## Revisión manual rápida

1. Entra como técnico e intenta abrir `/usuarios`: debe salir «Zona restringida».
2. Crea un producto con stock 5 y mínimo 5: debe aparecer la alerta de inmediato.
3. Vende más unidades de las que hay: debe rechazar y dejar el stock intacto.
