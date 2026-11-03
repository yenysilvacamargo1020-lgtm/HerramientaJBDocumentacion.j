# 6. Vista de ejecución

Tres recorridos que muestran cómo trabajan juntas las piezas.

## Escenario 1 · Iniciar sesión

```mermaid
sequenceDiagram
    actor U as Usuario
    participant L as LoginController
    participant A as Auth (guard web)
    participant R as RegistroActividad
    U->>L: POST /login (correo, contraseña)
    Note over L: throttle: 5 intentos por minuto
    L->>A: Auth::attempt
    alt credenciales incorrectas
        L-->>U: "El correo o la contraseña no son correctos."
    else usuario sin rol
        L->>A: Auth::logout
        L-->>U: "Tu usuario no tiene un rol asignado."
    else correcto
        L->>R: registrar("Inició sesión")
        L-->>U: redirige al Panel
    end
```

## Escenario 2 · Registrar una venta

```mermaid
sequenceDiagram
    actor Adm as Administrador
    participant VC as Web\VentaController
    participant FS as FacturacionService
    participant IS as InventarioService
    participant DB as Base de datos
    Adm->>VC: POST /ventas (cliente, fecha, items)
    VC->>FS: registrarVenta(...)
    activate FS
    FS->>DB: BEGIN
    FS->>FS: calcular() con precios de la BD
    FS->>DB: INSERT ventas (total, ganancia)
    loop por cada producto
        FS->>IS: registrarSalida(producto, cantidad, "Venta #id")
        IS->>DB: SELECT ... FOR UPDATE
        alt stock insuficiente
            IS-->>FS: StockInsuficienteException
            FS->>DB: ROLLBACK (no se guarda nada)
        else alcanza
            IS->>DB: UPDATE cantidad, INSERT movimiento SALIDA
            IS->>IS: verificarStockMinimo → ¿crear alerta?
        end
    end
    FS->>DB: COMMIT
    deactivate FS
    VC-->>Adm: Venta registrada y stock descontado
```

!!! info "Cómo se enlaza la venta con sus productos"
    La tabla `ventas` no tiene tabla de detalle. Cada salida de stock guarda el texto `Venta #<id>` en `movimiento_inventario.motivo`, y la pantalla de la venta reconstruye las líneas buscando ese texto.

## Escenario 3 · Alerta de stock bajo

```mermaid
flowchart TD
    A[Se registra una salida o se edita un producto] --> B{cantidad <= stock_minimo?}
    B -- No --> Z[No pasa nada]
    B -- Sí --> C{¿Ya hay una alerta<br/>sin leer de este producto?}
    C -- Sí --> Z
    C -- No --> D[Crea alerta en alertas_inventario:<br/>«Stock bajo: ... tiene N unidad(es) (mínimo M)»]
    D --> E[Aparece en Alertas y en el contador rojo del menú]
    E --> F[El usuario la marca como leída]
```
