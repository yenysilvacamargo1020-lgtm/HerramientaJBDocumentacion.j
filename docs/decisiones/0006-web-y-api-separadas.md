# ADR-0006 · Interfaz web y API REST como dos entradas separadas

**Estado:** Aceptada (con deuda: ver riesgos 1 y 2)

## Contexto

El proyecto necesitaba pantallas para el equipo del taller, y también un CRUD por cada tabla que se pudiera consultar y documentar (con Scribe) para la entrega académica.

## Decisión

Mantener **dos grupos de rutas** sobre los mismos modelos:

| Entrada      | Archivo             | Controladores                     | Respuesta        | Seguridad                 |
|--------------|---------------------|-----------------------------------|------------------|---------------------------|
| Interfaz web | `routes/web.php`    | `app/Http/Controllers/Web/`       | Vistas Blade     | Sesión + `VerificarRol`   |
| API REST     | `routes/api.php`    | `app/Http/Controllers/`           | JSON             | **Ninguna por ahora**     |

La interfaz web contiene las reglas de negocio (stock, ventas, alertas). La API es un CRUD genérico por recurso.

## Consecuencias

- ✅ Las pantallas pueden evolucionar sin romper la API.
- ✅ La documentación de la API se genera sola con Scribe.
- ⚠️ La API no pasa por `InventarioService` ni `FacturacionService`, así que puede saltarse las reglas del inventario.
- ⚠️ La API no está protegida. **No debe publicarse en internet así.**
