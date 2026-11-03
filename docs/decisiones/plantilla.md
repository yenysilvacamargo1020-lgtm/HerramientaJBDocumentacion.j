# Plantilla para un ADR nuevo

Copia este archivo como `decisiones/NNNN-titulo-corto.md` (con el siguiente número libre), llénalo y agrégalo a `mkdocs.yml` y a la tabla de [Decisiones](index.md).

```markdown
# ADR-NNNN · Título corto en forma de decisión

**Estado:** Propuesta | Aceptada | Reemplazada por ADR-XXXX

## Contexto

¿Qué problema hay? ¿Qué fuerzas empujan en distintas direcciones?
Escribe en 3 a 6 líneas, sin asumir que el lector conoce el proyecto.

## Opciones

| Opción | Ventaja | Problema |
|--------|---------|----------|
|        |         |          |

## Decisión

Lo que se escogió, en una o dos frases. Si ayuda, un ejemplo de código.

## Consecuencias

- ✅ Lo bueno que trae.
- ⚠️ Lo que cuesta o lo que queda pendiente.
```

!!! tip "Reglas de oro"
    - Un ADR **no se reescribe**: si cambias de opinión, escribe otro y marca el anterior como *Reemplazada*.
    - Corto es mejor. Si pasa de una pantalla, probablemente son dos decisiones.
