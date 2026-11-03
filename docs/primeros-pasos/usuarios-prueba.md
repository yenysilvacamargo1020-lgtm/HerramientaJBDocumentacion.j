# Usuarios de prueba

El sistema tiene **dos roles**: Administrador y Técnico. Hay dos formas de tener usuarios para probar.

## 1. Solo el administrador inicial

Lo crea el comando `php artisan db:seed --class=RolesYAdministradorSeeder`. Si ya existe, no lo duplica.

| Rol           | Correo                    | Contraseña   |
|---------------|---------------------------|--------------|
| Administrador | `admin@herramientajb.com` | `Admin12345` |

## 2. Datos de ejemplo completos

Con `php artisan migrate:fresh --seed` se crea además un pequeño taller de mentira: usuarios, 6 categorías, 5 proveedores, 8 productos, entradas, salidas, ventas, cotizaciones, máquinas con hoja de vida y mantenimientos.

Todos los usuarios de ejemplo usan la contraseña **`12345678`**.

| Rol           | Correo                                         | Para probar                             |
|---------------|------------------------------------------------|-----------------------------------------|
| Administrador | `admin@herramientajb.com` (clave `Admin12345`) | Todo el sistema                         |
| Técnico       | `carlos@sena.edu.co`                           | Salidas, hojas de vida y mantenimientos |
| Técnico       | `ana@sena.edu.co`                              | Igual que el anterior                   |
| Técnico       | `pedro@sena.edu.co`                            | Igual que el anterior                   |
| Técnico       | `maria@sena.edu.co`                            | Igual que el anterior                   |

!!! info "Más de un administrador"
    El *seeder* de ejemplo también crea un administrador adicional con correo `@sena.edu.co`. Puedes verlo en `database/seeders/DatabaseSeeder.php`.

## Qué probar con cada rol

=== "Administrador"

    1. Entra y mira el **Panel**: verás ventas y ganancia del mes.
    2. Crea una **categoría** y un **proveedor**.
    3. Crea un **producto** con stock inicial y un stock mínimo cercano.
    4. Registra una **entrada** (compra) y comprueba que el stock sube.
    5. Haz una **cotización**, pulsa **Convertir en venta** y confirma que el stock baja.
    6. Revisa el **Historial** para ver quién hizo cada cosa.

=== "Técnico"

    1. Entra y mira el **Panel**: no hay datos de dinero.
    2. Abre **Productos**: puedes ver, pero no crear ni editar.
    3. Registra una **salida** de un repuesto (por ejemplo, «reparación»).
    4. Deja un producto bajo su mínimo y mira cómo aparece en **Alertas**.
    5. Abre una **máquina**, edita su hoja de vida y registra un **mantenimiento**.
    6. Intenta abrir `/ventas`: verás la pantalla **Zona restringida**.
