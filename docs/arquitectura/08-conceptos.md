# 8. Conceptos transversales

Reglas que se repiten en todo el sistema. Si cambias una, revisa dónde más aplica.

## Autenticación

- Los usuarios viven en la tabla `usuarios`, no en `users`. Por eso `config/auth.php` apunta a `App\Models\Usuario`.
- La contraseña está en la columna `contraseña`. El modelo declara `$authPasswordName = 'contraseña'` y `getAuthPassword()` para que Laravel la encuentre.
- El inicio de sesión se hace con el campo `correo`.
- La tabla no tiene `remember_token`, así que **«recordarme» está desactivado**.
- `POST /login` tiene límite de **5 intentos por minuto**.
- Un usuario sin rol no puede entrar.

## Autorización (roles)

- Hay dos roles: `administrador` y `tecnico`.
- Se comparan por su *slug*: `Str::slug('Técnico')` da `tecnico`, así que no importan tildes ni mayúsculas.
- Las constantes están en `App\Models\Rol` (`ADMINISTRADOR`, `TECNICO`).
- `VerificarRol` recibe los roles permitidos y responde **403** si el usuario no tiene ninguno.
- El menú lateral oculta los enlaces `admin => true` a quien no es administrador.

## Inventario

- El stock (`productos.cantidad`) **solo cambia** desde `InventarioService`.
- Al crear un producto con cantidad mayor que 0 se registra un movimiento `Inventario inicial`.
- Al editar un producto **no se puede cambiar** la cantidad (la regla de validación solo existe en la creación).
- Cada entrada y cada salida deja un registro en `movimiento_inventario`.

## Dinero

- Precios en `DECIMAL(10,2)`.
- `precio_venta` no puede ser menor que `precio_compra` (regla `gte`).
- Totales y ganancias se calculan **siempre en el servidor** con `FacturacionService::calcular`.
- Si el mismo producto aparece en varias filas del formulario, las cantidades se suman.

## Borrado lógico

Todas las tablas del negocio tienen `deleted_at`. «Eliminar» marca el registro, no lo borra. Las relaciones que apuntan a registros eliminados usan `withTrashed()` para que el historial siga mostrando nombres.

## Historial de actividad

`RegistroActividad::registrar("texto")` guarda la acción a nombre del usuario en sesión. Se usa al iniciar y cerrar sesión, y al crear, editar o eliminar productos, usuarios, ventas, cotizaciones, mantenimientos y demás.

## Validación y mensajes

- Las validaciones están en `app/Http/Requests/`.
- Los mensajes salen en español gracias a `lang/es/validation.php` y `APP_LOCALE=es`.
- Los `exists:` incluyen `deleted_at,NULL` para no aceptar registros eliminados.

## Zona horaria y fechas

Las fechas de negocio (`fecha`, `fecha_compra`, `fecha_ingreso`) son tipo `DATE`. La venta no acepta fechas futuras (`before_or_equal:today`).

## Imágenes

Se guardan en `storage/app/public/productos` (disco `public`). Se necesita `php artisan storage:link` para verlas. Formatos: JPG, PNG o WEBP, hasta 2 MB.
