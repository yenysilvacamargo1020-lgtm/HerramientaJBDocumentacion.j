# ADR-0001 · Arquitectura en capas

**Estado:** Aceptada

## Contexto

El sistema tiene 18 tablas y muchas operaciones repetidas (crear, leer, actualizar, borrar). Si cada controlador habla directo con la base de datos, el código se repite y es difícil de cambiar.

## Decisión

Separar el código en capas con una responsabilidad cada una:

```
Controlador  →  Servicio  →  Repositorio (interfaz)  →  Modelo  →  Base de datos
```

- **Controlador:** recibe la petición y devuelve la respuesta.
- **Servicio:** la lógica de negocio.
- **Repositorio:** las consultas. Hay un `BaseRepository` con el CRUD común y una **interfaz** por recurso, enlazada en `RepositoryServiceProvider`.
- **Modelo:** la tabla y sus relaciones.

Las operaciones con reglas de negocio importantes (`InventarioService`, `FacturacionService`) se usan también desde los controladores web.

## Consecuencias

- ✅ Cada pieza se puede cambiar o probar por separado.
- ✅ El CRUD nuevo se copia con un patrón claro (ver [Crear un recurso nuevo](../guias/nuevo-recurso.md)).
- ⚠️ Los controladores web consultan algunos modelos directamente (por ejemplo, listados con `paginate`). No es una capa 100 % estricta.
- ⚠️ Para recursos simples hay varios archivos casi idénticos.
