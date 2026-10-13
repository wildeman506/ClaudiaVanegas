# Manual de uso

## Sistema POS Claudia Vanegas

Guía práctica para administradores y cajeros. El sistema también incluye un catálogo
público para que los clientes consulten productos y envíen pedidos.

## 1. Acceso al sistema

1. Abre la dirección web que te haya compartido la tienda. Para iniciar sesión,
   selecciona **Iniciar sesión** o entra a `/login`.
2. Escribe el correo y la contraseña asignados por el administrador y pulsa el
   botón para ingresar.
3. Para salir, usa **Cerrar Sesión** al final del menú lateral.

No compartas tu contraseña ni dejes la sesión abierta en un equipo compartido.
Si no puedes entrar, solicita al administrador que revise tu cuenta o restablezca
la contraseña.

### Menús según el perfil

| Perfil | Acceso en el menú |
| --- | --- |
| Cajero | Panel, Punto de Venta, Historial Ventas |
| Administrador | Panel, Punto de Venta, Historial Ventas, Pedidos Catálogo, Inventario, Proveedores, Cuentas por Cobrar, Cuentas por Pagar, Configuración y Usuarios |
| Público | Catálogo de productos y envío de pedidos, sin iniciar sesión |

El administrador puede gestionar usuarios y configuración. El cajero registra ventas
y consulta el historial disponible para su perfil. Si una opción no aparece, confirma
con el administrador qué permisos tiene tu cuenta.

## 2. Registrar una venta

1. En el menú, abre **Punto de Venta**.
2. Busca el producto por nombre o código y selecciónalo para añadirlo a la orden.
   Si tiene variantes, elige el color y la talla disponibles.
3. Revisa la orden. Usa los controles para cambiar cantidades, quitar artículos o
   revisar el precio de cada línea antes de cobrar.
4. Si quieres asociar la venta a un cliente, búscalo por nombre o cédula. Si no
   existe, elige **Crear cliente**, completa cédula, nombres y apellidos, y guarda.
   El correo y el teléfono son opcionales.
5. Selecciona el método de pago:
   - **Efectivo / Transferencia**
   - **Crédito**
   - **Datáfono**
   - **Link de pago**
6. Verifica el total mostrado y pulsa **Cobrar**. Para datáfono y link, revisa el
   total final, ya que el sistema puede incluir un recargo.
7. Espera el mensaje de confirmación antes de iniciar otra venta.

### Ventas a crédito

- Para cobrar a crédito debes seleccionar un cliente.
- Puedes registrar un anticipo opcional; el sistema muestra el saldo pendiente.
- La venta crea una cuenta por cobrar, que el administrador puede consultar y
  actualizar desde **Cuentas por Cobrar**.
- La pantalla indica un vencimiento de 15 días para el saldo.

### Link de pago Bold

Cuando seleccionas **Link de pago**, el sistema registra la venta e intenta crear
el enlace. Si se genera, copia el enlace y compártelo con el cliente. El estado del
pago puede consultarse en **Historial Ventas**.

Si aparece un error al generar el link, **la venta puede haber quedado registrada
igualmente**. No repitas el cobro sin verificar primero el historial; informa al
administrador para que revise la venta y la conexión con Bold. Si el enlace figura
como vencido, el administrador puede abrir el detalle de la venta y usar **Renovar**
cuando esa opción esté disponible.

## 3. Consultar el historial de ventas

1. Abre **Historial Ventas**.
2. Busca por nombre o cédula del cliente, o filtra por método de pago.
3. Selecciona el detalle de una venta para consultar sus datos y artículos.
4. El administrador puede cambiar el método de pago cuando la venta lo permita y
   anular ventas desde el historial. Antes de anular, confirma el registro correcto
   y escribe el motivo solicitado.

Las ventas anuladas se identifican en el historial. Los pagos por link muestran el
estado reportado para Bold.

## 4. Administrar inventario (administrador)

### Buscar y revisar productos

Abre **Inventario**. Puedes buscar por nombre o código y filtrar por categoría.
La tabla muestra código, producto, categoría, costo, precio de venta, existencias,
variantes y visibilidad en el catálogo.

- Usa el control de visibilidad para mostrar u ocultar el producto del catálogo
  público.
- El historial de movimientos permite revisar entradas y movimientos del producto.
- Para editar, selecciona el control de edición. Para eliminar, revisa con cuidado
  el producto seleccionado antes de confirmar.

### Crear o editar un producto

1. Selecciona **Nuevo Producto** o abre la edición de un producto existente.
2. Completa nombre, descripción, categoría, existencias, precio de costo y precio
   de venta. Puedes añadir URLs de imágenes separadas por comas.
3. Guarda el producto. Al editar uno que ya existe, su código no se puede modificar.
4. Para prendas o calzado con inventario separado por combinación, abre la pestaña
   **Variantes** y agrega las combinaciones de color y talla que correspondan.
   Introduce las existencias de cada variante; el stock total del producto se calcula
   a partir de estas.

### Recibir mercancía

Usa **Recibir Mercancía** cuando llegue inventario:

1. Añade una fila por producto recibido.
2. Si el producto no existe, pulsa **Nuevo producto** en la parte superior del modal,
   completa nombre, categoría y precios de costo y venta, y selecciona **Crear y agregar**.
   El producto se crea sin stock inicial y se coloca automáticamente en una fila vacía.
3. Selecciona el producto y, si la variante aún no existe, pulsa el botón **+** junto
   al selector de variantes. Elige color y talla cuando corresponda y pulsa
   **Crear y seleccionar**. La variante nueva queda con stock cero; la cantidad que
   recibas se registra en la fila de recepción.
4. Indica cantidad y costo unitario. Puedes ajustar el precio de venta, seleccionar
   un proveedor y añadir una fecha límite si la compra queda pendiente.
5. Marca el estado de pago como **Pagado** o **Pendiente**.
6. Revisa los datos y guarda la recepción.

La recepción aumenta el stock. Si la registras como pagada, queda contabilizada como
pagada; si la dejas pendiente, el sistema crea la cuenta correspondiente en
**Cuentas por Pagar**. Para los productos con variantes, elige la variante correcta
en cada fila para que se actualice su stock específico.

## 5. Administrar proveedores (administrador)

En **Proveedores** puedes crear, buscar, editar y revisar el historial de un proveedor.
Al crear o editarlo, completa el nombre de la empresa; también puedes guardar persona
de contacto, correo y teléfono. Mantener proveedores actualizados facilita asociarlos
al recibir mercancía.

## 6. Revisar y procesar pedidos del catálogo (administrador)

Los pedidos enviados desde el catálogo público aparecen en **Pedidos Catálogo**.
Puedes filtrar la lista por **Pendientes**, **Facturados**, **Cancelados** o **Todos**.
Selecciona un pedido para revisar sus artículos y los datos de contacto.

Para facturar un pedido:

1. Abre un pedido pendiente y selecciona **Facturar**.
2. Revisa cantidades y precios. Si hace falta, actualiza los artículos antes de
   facturar y guarda los cambios para continuar después.
3. Elige el método de pago. Para facturar a crédito, selecciona un cliente existente
   o crea uno y selecciónalo.
4. Verifica el total y confirma **Facturar**. Si eliges link de pago, comparte el
   enlace generado con el cliente.

Al facturarse, el pedido queda asociado a una venta y deja de estar pendiente. Si el
pedido no debe continuar, usa **Cancelar** y confirma la acción. La facturación
depende de que haya existencias suficientes para los artículos y variantes.

## 7. Cuentas por cobrar (administrador)

Las ventas a crédito generan cuentas por cobrar. En **Cuentas por Cobrar** puedes:

- Filtrar cuentas pendientes o pagadas y buscar por cliente.
- Abrir el historial para consultar los abonos recibidos.
- Seleccionar **Recibir Pago**, registrar el monto recibido y añadir una nota, por
  ejemplo, el medio de pago.
- Confirmar el registro y revisar el saldo/estado actualizado.

Registra solo los pagos efectivamente recibidos. Si el cliente hace varios abonos,
registra cada uno por separado para conservar el historial.

## 8. Cuentas por pagar (administrador)

En **Cuentas por Pagar** puedes filtrar obligaciones pendientes, parciales y pagadas.
Las recepciones de mercancía marcadas como pendientes aparecen aquí automáticamente.
También puedes crear una cuenta para otros gastos indicando descripción y monto.

Para registrar un pago, abre la cuenta, selecciona la acción de pago, indica el monto
y agrega una nota si hace falta. Confirma después de verificar el valor. Los pagos
parciales mantienen un saldo pendiente; una obligación puede volver a estado pendiente
si se usa la acción para revertir su estado.

## 9. Panel de control

El **Panel** resume la actividad del negocio. Presenta indicadores y gráficos de
facturación y recaudo, utilidad, ventas por categoría y método de pago, gastos frente
a ingresos, costo del inventario, productos más vendidos y productos sin movimiento.

Usa los filtros de fechas de los gráficos para cambiar el período. El listado de
productos más vendidos también permite filtrar por categoría. El panel ayuda a revisar
tendencias; para confirmar una transacción específica, abre **Historial Ventas** o la
cuenta correspondiente.

## 10. Usuarios del sistema (administrador)

En **Usuarios**, selecciona **Nuevo Usuario** y registra nombre, correo, perfil
(Administrador o Cajero) y contraseña. Usa la edición para actualizar los datos o
cambiar una contraseña; si dejas el campo de contraseña vacío al editar, se conserva
la contraseña actual.

Revisa el perfil antes de guardar: un administrador puede acceder a configuración,
inventario y operaciones administrativas. No es posible eliminar desde esta pantalla
la cuenta con la que tienes la sesión abierta.

## 11. Configuración de tienda (administrador)

En **Configuración** puedes:

- Cambiar el color de marca y el logo. El logo admite PNG, JPG o SVG y no debe superar
  2 MB.
- Actualizar nombre, correo, dirección, teléfono y enlaces de Instagram o TikTok.
- Activar o desactivar el envío automático de facturas y de links de pago.
- Configurar servidor, puerto, usuario, contraseña y remitente SMTP.

Guarda los cambios al terminar. Para que los correos se envíen, el servidor SMTP debe
estar correctamente configurado. La contraseña SMTP aparece protegida al consultar
la configuración; solo reemplázala cuando quieras establecer una nueva.

## 12. Catálogo público para clientes

El catálogo está disponible desde la página principal del sistema y no requiere
iniciar sesión. El cliente puede explorar categorías, elegir productos y variantes,
añadirlos al carrito y enviar el pedido con su nombre y celular. El correo y las notas
son opcionales.

Enviar el pedido **no constituye un pago ni confirma por sí solo la disponibilidad**.
La solicitud queda pendiente; la tienda revisa existencias y se comunica con el cliente
para confirmar. El seguimiento del pedido lo realiza el administrador desde
**Pedidos Catálogo**.

## 13. Funciones que no tienen una pantalla propia actualmente

- No hay una opción independiente de **Clientes** en el menú. Busca o crea clientes
  desde Punto de Venta o al facturar un pedido del catálogo.
- No hay una pantalla independiente de **Informes**. Para consultar gráficos usa el
  Panel de control.
- La ruta de administración formal de órdenes de compra a proveedores no está
  conectada al menú actual. Para registrar stock recibido, utiliza **Inventario →
  Recibir Mercancía**; las obligaciones pendientes se consultan en **Cuentas por Pagar**.

## 14. Si algo no funciona

- **No aparece un producto en el POS o catálogo:** pide al administrador revisar que
  esté visible en el catálogo y que tenga stock disponible para el producto o variante.
- **El sistema indica stock insuficiente:** revisa cantidad, variante y existencias
  en Inventario antes de intentar cobrar de nuevo.
- **No puedes registrar una venta a crédito:** selecciona o crea primero un cliente.
- **El link Bold falla:** verifica el historial antes de volver a cobrar; la venta
  puede haberse registrado aunque no se haya generado el enlace. Informa al
  administrador.
- **No puedes abrir una opción del menú:** confirma tu perfil con el administrador.
- **No llegan facturas o links por correo:** solicita que el administrador revise
  la configuración SMTP y las opciones de notificación.

