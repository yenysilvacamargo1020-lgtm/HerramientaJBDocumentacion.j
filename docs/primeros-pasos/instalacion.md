# Instalación

Hay dos formas de poner HerramientaJB a funcionar. La más rápida es con **Docker** (trae PHP y MySQL listos). La segunda es **local**, si ya tienes PHP y MySQL instalados.

!!! warning "Antes de empezar"
    Haz una copia de seguridad de la carpeta del proyecto si ya tenías trabajo avanzado.

## Requisitos

| Herramienta | Versión                        | Para qué                               |
|-------------|--------------------------------|----------------------------------------|
| PHP         | 8.2 o superior (8.3 en Docker) | Ejecutar Laravel                       |
| Composer    | 2.x                            | Instalar dependencias de PHP           |
| Node.js     | 20 o superior                  | Compilar estilos y JS con Vite         |
| MySQL       | 8.x                            | Base de datos                          |
| Docker      | Compose v2                     | Solo si usas la opción con Docker      |

## Opción A · Con Docker

```bash
# 1. Descarga el proyecto y entra a la carpeta
git clone https://github.com/yenysilvacamargo1020-lgtm/HerramientaJB.git
cd HerramientaJB

# 2. Crea el archivo de entorno
cp .env.example .env
```

Abre `.env` y deja estas líneas así (el servidor de base de datos se llama `db` dentro de Docker):

```ini
APP_LOCALE=es
SESSION_DRIVER=database

DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=1234
```

```bash
# 3. Levanta los contenedores
docker compose up -d --build

# 4. Prepara la aplicación
docker compose exec app php artisan key:generate
docker compose exec app php artisan migrate
docker compose exec app php artisan db:seed --class=RolesYAdministradorSeeder
docker compose exec app php artisan storage:link
docker compose exec app php artisan optimize:clear
```

La imagen de Docker **no incluye Node**, así que los estilos se compilan en tu computador:

```bash
npm install
npm run dev
```

Entra a **<http://127.0.0.1:8000/login>**.

| Servicio    | Dirección                | Notas                                    |
|-------------|--------------------------|------------------------------------------|
| Aplicación  | <http://localhost:8000>  | Contenedor `c_laravel`                   |
| MySQL       | `localhost:3308`         | Usuario `root`, contraseña `1234`        |
| phpMyAdmin  | <http://localhost:8881>  | Para mirar las tablas desde el navegador |

## Opción B · Local (sin Docker)

```bash
composer install
cp .env.example .env
php artisan key:generate
```

En `.env` configura tu base de datos (`DB_CONNECTION=mysql`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`) y agrega:

```ini
APP_LOCALE=es
SESSION_DRIVER=database
```

Luego:

```bash
php artisan migrate
php artisan db:seed --class=RolesYAdministradorSeeder
php artisan storage:link
php artisan optimize:clear
```

Enciende el proyecto con **dos terminales abiertas al mismo tiempo**:

```bash
# Terminal 1
php artisan serve
```

```bash
# Terminal 2
npm install
npm run dev
```

Entra a **<http://127.0.0.1:8000/login>**.

## Primer ingreso

| Campo      | Valor                     |
|------------|---------------------------|
| Correo     | `admin@herramientajb.com` |
| Contraseña | `Admin12345`              |

!!! danger "Cambia esa contraseña"
    Cuando entres, ve a **Usuarios** y cámbiala. Desde ahí también creas a los técnicos.

## Cargar datos de ejemplo (opcional)

Para ver el sistema con productos, ventas, máquinas y mantenimientos ya creados:

```bash
php artisan migrate:fresh --seed
```

!!! warning
    `migrate:fresh` **borra todas las tablas y datos** y los vuelve a crear. Úsalo solo en desarrollo.

Los usuarios que crea están en [Usuarios de prueba](usuarios-prueba.md).

## Si algo falla

| Error                                                          | Solución                                                                        |
|----------------------------------------------------------------|---------------------------------------------------------------------------------|
| `Class "App\Models\User" not found` o no puedes iniciar sesión | Revisa que `config/auth.php` use `App\Models\Usuario::class` y ejecuta `php artisan optimize:clear` |
| `Unable to locate file in Vite manifest`                       | `npm run dev` no está encendido                                                 |
| La página se ve sin estilos                                    | Ejecuta `npm install` y vuelve a lanzar `npm run dev`                           |
| `Cannot find module '@tailwindcss/vite'`                       | Ejecuta otra vez `npm install`                                                  |
| Las imágenes de productos no aparecen                          | Ejecuta `php artisan storage:link`                                              |
| Los mensajes salen en inglés                                   | Pon `APP_LOCALE=es` en `.env` y ejecuta `php artisan optimize:clear`            |
| `Table 'sessions' doesn't exist`                               | Ejecuta `php artisan migrate`                                                   |
| `SQLSTATE[HY000] [2002]` en Docker                             | Usa `DB_HOST=db` (no `127.0.0.1`) y espera unos segundos a que MySQL arranque   |
