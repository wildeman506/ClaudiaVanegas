# Claudia Vanegas POS

Sistema de punto de venta e inventario para la tienda de ropa, zapatos y accesorios
Claudia Vanegas. Este documento describe la estructura actual del
código, las rutas registradas y cómo configurar y operar los servicios.

## Arquitectura general

El repositorio es un monorepo pnpm/TypeScript. El navegador sirve la aplicación React;
esta llama al API Express bajo `/api`, y el API persiste los datos en PostgreSQL mediante
Drizzle ORM.

| Parte | Tecnologías | Ubicación |
| --- | --- | --- |
| Interfaz POS y catálogo público | React 19, TypeScript, Vite, Wouter, TanStack Query, Zustand, Tailwind CSS 4, Radix UI, Recharts | `artifacts/pos/` |
| API | Node.js, Express 5, TypeScript, JWT, bcryptjs, Pino | `artifacts/api-server/` |
| Datos | PostgreSQL, `pg`, Drizzle ORM / Drizzle Kit | `lib/db/` |
| Contrato y validación | OpenAPI, Orval, Zod / drizzle-zod | `lib/api-spec/`, `lib/api-zod/` |
| Cliente para React | Hooks generados para TanStack Query y `customFetch` | `lib/api-client-react/` |
| Vista previa de componentes | React y Vite; servicio auxiliar de desarrollo | `artifacts/mockup-sandbox/` |

El frontend configura un proxy de desarrollo de `/api` a `http://localhost:8080`.
En el servidor, Express monta sus routers debajo de `/api`. Vite usa `BASE_PATH`
como base pública y `PORT` como puerto de escucha; el servidor API requiere que
le proporcione `PORT`.

## Estructura del repositorio

- `artifacts/pos/src/App.tsx` y `AppRouter.tsx`: arranque del frontend y rutas de pantalla.
- `artifacts/pos/src/pages/`: pantallas de la aplicación.
- `artifacts/pos/src/components/`: diseño de página, navegación y componentes de interfaz.
- `artifacts/pos/src/hooks/use-auth.ts`: estado de sesión del frontend.
- `artifacts/pos/src/hooks/use-brand-settings.ts`: ajustes de marca usados por la interfaz.
- `artifacts/api-server/src/app.ts`: middleware HTTP, parsers y prefijo `/api`.
- `artifacts/api-server/src/routes/`: endpoints Express; `routes/index.ts` los registra.
- `artifacts/api-server/src/lib/`: JWT, Bold, correo, zona horaria y logging.
- `lib/db/src/schema/`: esquema Drizzle y exportación de tablas.
- `lib/db/src/seed.ts`: creación idempotente de los usuarios iniciales.
- `lib/api-spec/openapi.yaml`: contrato OpenAPI usado por la generación de código.
- `lib/api-client-react/src/generated/` y `lib/api-zod/src/generated/`: código generado; no editar manualmente.
- `attached_assets/`: recursos estáticos disponibles para Vite mediante el alias `@assets`.
- `artifacts/mockup-sandbox/`: previsualización de componentes; no forma parte de las rutas de negocio del POS.

## Rutas de la interfaz

Las rutas de abajo son las que registra actualmente `artifacts/pos/src/AppRouter.tsx`.
El prefijo de la URL pública puede depender de `BASE_PATH`.

| Ruta | Pantalla | Acceso |
| --- | --- | --- |
| `/` | Catálogo público | Público |
| `/login` | Inicio de sesión | Público |
| `/dashboard` | Resumen y métricas | Admin y cajero |
| `/pos` | Punto de venta | Admin y cajero |
| `/ventas` | Historial de ventas | Admin y cajero; el cajero consulta sus ventas |
| `/pedidos` | Pedidos del catálogo público | Solo admin |
| `/inventario` | Productos, variantes e inventario | Solo admin |
| `/proveedores` | Proveedores | Solo admin |
| `/cuentas-pagar` | Cuentas por pagar | Solo admin |
| `/cuentas-cobrar` | Cuentas por cobrar | Solo admin |
| `/configuracion` | Datos de tienda y ajustes | Solo admin |
| `/usuarios` | Usuarios y roles | Solo admin |

Las rutas protegidas redirigen a `/login` si no hay sesión; si un cajero entra a una
ruta administrativa, se le redirige a `/dashboard`. Las rutas no encontradas muestran
la pantalla 404.

**Diferencias entre archivos y rutas activas:** existe `src/pages/clientes.tsx`, pero
la pantalla de clientes no está registrada en `AppRouter.tsx`. El API de clientes sí
está activo. El API también ofrece reportes, pero no hay actualmente una pantalla
`/informes` registrada en el router.

## API HTTP

Todos los endpoints llevan el prefijo `/api`. En esta tabla, “sesión” significa
JWT Bearer válido y “admin” significa sesión con rol `admin`. La autorización debe
validarse siempre en el middleware del API; ocultar una pantalla no sustituye esa
validación.

| Área | Rutas registradas | Acceso |
| --- | --- | --- |
| Salud | `GET /healthz` | Público |
| Sesión | `POST /auth/login`, `GET /auth/me` | Inicio público; perfil con sesión |
| Catálogo público | `GET /catalog`, `POST /catalog/order` | Público |
| Webhook de Bold | `POST /webhooks/bold` | Público; autenticación por firma cuando se configura el secreto |
| Usuarios | `GET/POST /users`, `GET/PUT/DELETE /users/:id` | Admin |
| Productos | `GET /products`, `GET /products/:id`, `GET /products/:id/supplier` | Sesión |
| Productos (gestión) | `POST /products`, `PUT /products/:id`, `PATCH /products/:id/visibility`, `DELETE /products/:id`, `GET /products/:id/movements` | Admin |
| Variantes | `POST /products/:id/variants`, `PUT/DELETE /products/:id/variants/:variantId` | Admin |
| Clientes | `GET/POST /customers`, `GET/PUT/DELETE /customers/:id` | Sesión |
| Proveedores | `GET /suppliers`, `GET /suppliers/:id` | Sesión |
| Proveedores (gestión) | `POST /suppliers`, `PUT/DELETE /suppliers/:id` | Admin |
| Órdenes de compra | `GET/POST /purchase-orders`, `GET/PUT /purchase-orders/:id`, `POST /purchase-orders/:id/receive` | Admin |
| Entradas de inventario | `GET/POST /inventory-entries` | Admin |
| Ventas | `GET/POST /sales`, `GET /sales/:id` | Sesión |
| Ventas (gestión) | `PATCH /sales/:id/payment-type`, `POST /sales/:id/void` | Admin |
| Enlaces de pago | `POST /payment-links/bold` | Sesión |
| Cuentas por pagar | `GET/POST /accounts-payable`, `GET /accounts-payable/:id`, `PATCH /accounts-payable/:id/status`, `POST /accounts-payable/:id/payments` | Admin |
| Cuentas por cobrar | `GET /accounts-receivable`, `GET /accounts-receivable/:id`, `POST /accounts-receivable/:id/payments` | Admin |
| Pedidos de catálogo | `GET /catalog-orders`, `GET /catalog-orders/:id`, `PUT /catalog-orders/:id`, `PUT /catalog-orders/:id/cancel`, `POST /catalog-orders/:id/invoice` | Admin |
| Dashboard | `GET /dashboard/summary`, `/dashboard/billing-vs-collection`, `/dashboard/top-products`, `/dashboard/sales-by-category`, `/dashboard/payment-type-breakdown`, `/dashboard/inventory-cost-by-category`, `/dashboard/expenses-vs-income`, `/dashboard/net-profit-trend`, `/dashboard/slow-moving-products` | Sesión |
| Informes | `GET /reports/sales-by-month`, `/reports/sales-by-category`, `/reports/payment-type-breakdown`, `/reports/top-products` | Admin |
| Configuración | `GET /settings` | Sesión |
| Configuración pública | `GET /settings/public` | Público; datos públicos de marca/tienda |
| Configuración (cambios) | `PUT /settings` | Admin |

Los endpoints y el middleware real en `artifacts/api-server/src/routes/` son la
referencia del comportamiento HTTP desplegado. `openapi.yaml` alimenta el cliente
tipado y las validaciones generadas; al añadir o cambiar endpoints, mantener ambos
al día y ejecutar el codegen (ver comandos).

## Roles, sesión y seguridad

- Roles de aplicación: `admin` y `cajero`.
- El API valida tokens JWT enviados como `Authorization: Bearer <token>`.
- Los JWT se firman con `JWT_SECRET` y vencen a los 7 días.
- El cliente web guarda el token en `localStorage` con la clave `pos_token`;
  `lib/api-client-react/src/custom-fetch.ts` lo añade a las llamadas al API.
- Las contraseñas iniciales se convierten a hash con bcryptjs. El seed omite emails
  que ya existan y puede ejecutarse de nuevo.
- El seed de desarrollo incluye estos accesos iniciales: admin
  `admin@claudiavanegas.com` / `admin123`; cajero `cajero@claudiavanegas.com` /
  `cajero123`. Son credenciales predecibles incluidas en el código: no usarlas en
  producción; cambiar las contraseñas o sustituir el seed antes de exponer el sistema.
- Usar HTTPS y un `JWT_SECRET` aleatorio, largo y exclusivo del entorno.

## Modelo de datos y reglas de negocio

El esquema vive en `lib/db/src/schema/`; se conecta a PostgreSQL con `pg` y
`drizzle-orm/node-postgres`.

| Grupo | Tablas principales | Uso |
| --- | --- | --- |
| Identidad | `users` | Usuarios, roles y hashes de contraseña |
| Catálogo e inventario | `products`, `product_variants`, `inventory_entries` | Productos, variantes por color/talla, existencias y movimientos |
| Personas | `customers`, `suppliers` | Clientes y proveedores |
| Compras | `purchase_orders`, `purchase_order_items` | Órdenes a proveedores y recepción total o parcial |
| Ventas | `sales`, `sale_items` | Ventas, líneas, método de pago y estado del cobro |
| Cartera | `accounts_payable`, `ap_payments`, `accounts_receivable`, `ar_payments` | Saldos por pagar/cobrar y sus abonos |
| Catálogo online | `catalog_orders`, `catalog_order_items` | Pedidos públicos y sus líneas |
| Ajustes | `settings` | Configuración de tienda e información SMTP |

Flujos relevantes:

1. Una venta valida productos/variantes y existencias, y descuenta del stock
   correspondiente. Una venta a crédito genera una cuenta por cobrar.
2. Recibir una orden de compra actualiza existencias; la recepción puede ser parcial.
   Las compras a crédito se reflejan en cuentas por pagar.
3. El catálogo público permite enviar un pedido pendiente. Un admin puede revisarlo,
   cancelarlo o facturarlo; al facturarlo se genera una venta asociada.
4. El stock por variante se conserva en `product_variants`; las líneas de venta
   pueden referenciar una variante además del producto.
5. Los importes monetarios usan columnas `numeric` en PostgreSQL. Tener presente el
   tratamiento de valores decimales al convertir datos en las respuestas del API.

## Integración de pagos Bold

- `POST /api/payment-links/bold` solicita un enlace de pago; requiere `BOLD_API_KEY`.
- El API también expone `POST /api/webhooks/bold`, que Bold invoca sin JWT.
  Hay que registrar la URL pública exacta en el panel de Bold para que envíe eventos.
- La firma se valida como HMAC-SHA256 del cuerpo codificado en Base64; se compara
  como hexadecimal con `x-bold-signature` (sin el prefijo `sha256=`).
- Configurar `BOLD_WEBHOOK_SECRET` antes de habilitar pagos reales.
  **Si falta, el código omite la verificación de firma y acepta los eventos**, por
  lo que no debe dejarse sin configurar en producción.
- El evento se asocia principalmente mediante `data.metadata.reference` y, si no
  coincide, se intenta `data.payment_id` contra el identificador del enlace guardado.
- Eventos: `SALE_APPROVED` cambia el estado a pagado; `SALE_REJECTED` y
  `VOID_APPROVED` lo cambian a fallido. `VOID_REJECTED`, `SALE_IN_VALIDATION` y tipos
  desconocidos se confirman sin cambiar el estado.
- Tanto la URL de pruebas como la URL publicada deben registrarse en Bold al cambiar
  de entorno. No construir ni compartir la URL publicada a partir del dominio de
  desarrollo.

## Variables de entorno y secretos

| Variable | Requerida | Uso |
| --- | --- | --- |
| `DATABASE_URL` | Sí | Conexión PostgreSQL.. |
| `JWT_SECRET` | Sí | Firma/verificación JWT. El servidor falla al arrancar si falta. |
| `PORT` | Sí para el API | Puerto asignado por el entorno; el servidor API falla si falta o no es válido. |
| `BASE_PATH` | Recomendado para el frontend | Base URL de Vite; en este proyecto suele ser `/`. |
| `BOLD_API_KEY` | Solo para crear enlaces Bold | Autorización con la API de pagos Bold. |
| `BOLD_WEBHOOK_SECRET` | Obligatoria para webhooks reales | Validación de la firma entrante de Bold; sin ella se omite la validación. |

Los ajustes SMTP se guardan en la base de datos desde la configuración de la tienda,
no son una variable de entorno enumerada aquí. La contraseña SMTP se enmascara al
consultar la configuración y solo debe sustituirse al enviar un valor nuevo.

## Inicialización y ejecución

1. Instalar dependencias desde la raíz: `pnpm install`.
2. Confirmar que la base de datos PostgreSQL está provisionada (`DATABASE_URL`) y
   añadir `JWT_SECRET`s.
3. Aplicar el esquema al entorno de desarrollo: `pnpm --filter @workspace/db run push`.
4. Insertar los usuarios iniciales: `pnpm --filter @workspace/db run seed`.
5. Iniciar el frontend y API.

Workflows configurados:

| Workflow | Comando | Función |
| --- | --- | --- |
| `artifacts/pos: web` | `pnpm --filter @workspace/pos run dev` | Aplicación web y proxy `/api` en desarrollo |
| `artifacts/api-server: API Server` | `pnpm --filter @workspace/api-server run dev` | API Express (compila con esbuild y luego inicia Node) |
| `artifacts/mockup-sandbox: Component Preview Server` | `pnpm --filter @workspace/mockup-sandbox run dev` | Vista previa aislada de componentes |

Comandos de desarrollo:

```bash
pnpm run typecheck
pnpm run build
pnpm --filter @workspace/pos run typecheck
pnpm --filter @workspace/api-server run typecheck
pnpm --filter @workspace/api-spec run codegen
pnpm --filter @workspace/db run push
pnpm --filter @workspace/db run seed
```

`push` aplica cambios de esquema a la base de datos conectada; confirmar el entorno
antes de ejecutarlo. No usar `push-force` sobre datos importantes sin revisar el
impacto y tener respaldo.

## Cambios comunes

- **Cambiar una ruta de pantalla:** editar `artifacts/pos/src/AppRouter.tsx` y la
  página correspondiente en `artifacts/pos/src/pages/`.
- **Cambiar un endpoint:** actualizar su router en `artifacts/api-server/src/routes/`
  y confirmar que se monta en `routes/index.ts`. Mantener actualizado
  `lib/api-spec/openapi.yaml`; después ejecutar
  `pnpm --filter @workspace/api-spec run codegen`.
- **Cambiar el esquema:** editar o agregar un archivo en `lib/db/src/schema/`,
  exportarlo desde `schema/index.ts`, aplicar `pnpm --filter @workspace/db run push`
  al entorno correcto y actualizar los endpoints/contratos que lo utilicen.
- **Añadir tablas:** usar Drizzle ORM y las convenciones de `lib/db/src/schema/`;
  evitar SQL manual en la lógica de negocio salvo que exista una razón específica.

## Observaciones operativas

- El API registra solicitudes y errores con Pino; revisar los logs del workflow
  `artifacts/api-server: API Server` para diagnosticar errores HTTP o de integraciones.
- El servicio API tiene un endpoint de salud `GET /api/healthz`.
- En rutas Express 5, conservar handlers asíncronos con retorno `Promise<void>` y
  normalizar los parámetros `req.params` antes de convertir IDs.
- Al hacer cambios de esquema, API o frontend, validar los tipos y probar el flujo
  afectado en los workflows antes de publicar.
