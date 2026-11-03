# 7. Despliegue

## Entorno de desarrollo con Docker

```mermaid
flowchart LR
    subgraph Tu computador
        BR[Navegador] -->|:8000| APP
        BR -->|:8881| PMA
        VITE[npm run dev<br/>Vite] -.->|archivos compilados| APP
        subgraph Docker Compose · red red_db
            APP[c_laravel<br/>PHP 8.3 · artisan serve]
            DB[(c_mysql<br/>MySQL)]
            PMA[c_phpmyadmin]
            APP -->|db:3306| DB
            PMA --> DB
        end
        DB -.->|:3308| HOST[Clientes SQL externos]
    end
```

| Contenedor     | Imagen            | Puerto del computador | Notas                                              |
|----------------|-------------------|-----------------------|----------------------------------------------------|
| `c_laravel`    | `Dockerfile` (PHP 8.3-cli) | 8000        | Ejecuta `php artisan serve`. Monta el código como volumen. |
| `c_mysql`      | `mysql:latest`    | 3308 → 3306           | Base `laravel`, contraseña `root` = `1234`. Datos en el volumen `mysql_data`. |
| `c_phpmyadmin` | `phpmyadmin:latest` | 8881                | Conecta a `db:3306`.                               |

!!! warning "Node no está en la imagen"
    Vite (`npm run dev`) se ejecuta fuera de Docker, en tu computador. Para producción hay que compilar con `npm run build`.

## Variables de entorno importantes

| Variable          | Valor recomendado        | Por qué                                              |
|-------------------|--------------------------|------------------------------------------------------|
| `APP_LOCALE`      | `es`                     | Mensajes de validación en español                    |
| `SESSION_DRIVER`  | `database`               | La tabla `sessions` guarda las sesiones              |
| `DB_CONNECTION`   | `mysql`                  | `.env.example` trae `sqlite`                         |
| `DB_HOST`         | `db` (Docker) · `127.0.0.1` (local) | Nombre del servicio en Compose            |
| `APP_DEBUG`       | `false` en producción    | No mostrar errores internos a los usuarios           |

## Hacia producción

Esto **no está automatizado** en el repositorio. Lo mínimo recomendado:

1. Un servidor web real (Nginx o Apache) en lugar de `artisan serve`.
2. `APP_ENV=production`, `APP_DEBUG=false` y una `APP_KEY` propia.
3. `npm run build`, `php artisan config:cache`, `route:cache` y `view:cache`.
4. Contraseñas nuevas para MySQL y para el administrador inicial.
5. Proteger la API (hoy no tiene autenticación). Ver [Riesgos](11-riesgos.md).
