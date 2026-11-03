# 2. Restricciones

## Técnicas

| Restricción                                   | Detalle                                                                                  |
|-----------------------------------------------|------------------------------------------------------------------------------------------|
| Laravel 12 y PHP 8.2+                         | Indicado en `composer.json`. La imagen Docker usa PHP 8.3.                               |
| Base de datos relacional                      | MySQL en Docker. `.env.example` trae SQLite por defecto; hay que cambiarlo.              |
| Nombres de tablas y columnas en español       | Por ejemplo `usuarios.contraseña` (con eñe) y llaves primarias como `id_producto`.       |
| Tailwind CSS 4 con Vite                       | Sin `tailwind.config.js` ni `postcss.config.js`: Tailwind 4 no los necesita.             |
| Sesiones guardadas en la base de datos        | `SESSION_DRIVER=database`; por eso existe la tabla `sessions`.                           |
| Migraciones numeradas                         | `01_roles.php` a `18_Registros_conectividad.php`, más la tabla de sesiones: 19 en total. |

## Organizativas

| Restricción                      | Detalle                                                                   |
|----------------------------------|---------------------------------------------------------------------------|
| Proyecto académico SENA          | Tecnólogo en Análisis y Desarrollo de Software.                           |
| Las 19 migraciones no se tocan   | El frontend se diseñó para encajar con ellas tal como están.              |
| Documentación en español         | Los nombres de rutas, vistas y mensajes también están en español.         |

## Convenciones

- Llaves primarias con nombre propio: `id_rol`, `id_usuario`, `id_producto`…
- Todas las tablas del negocio usan **borrado lógico** (`deleted_at`). Ver [ADR-0003](../decisiones/0003-borrado-logico.md).
- Los usuarios inician sesión con **correo** y contraseña (no con `email`/`password`).
