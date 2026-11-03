# Decisiones (ADR)

Un **ADR** (*Architecture Decision Record*) es una nota corta que explica **por qué** se tomó una decisión importante: qué problema había, qué opciones se miraron y qué se escogió. Sirve para que nadie en el futuro (incluido tú) deshaga algo sin entender la razón.

| ADR                                                          | Decisión                                                    | Estado   |
|--------------------------------------------------------------|-------------------------------------------------------------|----------|
| [ADR-0001](0001-arquitectura-en-capas.md)                    | Arquitectura en capas                                       | Aceptada |
| [ADR-0002](0002-dos-roles.md)                                | Dos roles con middleware                                    | Aceptada |
| [ADR-0003](0003-borrado-logico.md)                           | Borrado lógico                                              | Aceptada |
| [ADR-0004](0004-stock-por-movimientos.md)                    | Stock solo por movimientos                                  | Aceptada |
| [ADR-0005](0005-precios-en-servidor.md)                      | Precios calculados en el servidor                           | Aceptada |
| [ADR-0006](0006-web-y-api-separadas.md)                      | Interfaz web y API separadas                                | Aceptada |
| [Plantilla](plantilla.md)                                    | Para escribir un ADR nuevo                                  | —        |

!!! tip "Cuándo escribir uno"
    Cuando la decisión sea difícil de revertir, afecte a varias partes del sistema o alguien pueda preguntar «¿por qué lo hicieron así?».
