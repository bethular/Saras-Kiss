# Sara'sKiss — Primera versión

## Antes de subir a GitHub

Abrí el archivo `config.js` (está en la raíz, afuera de las carpetas `catalogo` y `admin`)
y completá esta línea con el número de WhatsApp real de tu señora:

```
const TELEFONO_SARA = "COMPLETAR_NUMERO_AQUI";
```

Ejemplo: si el número es 376-4123456 (Misiones), tenés que poner:
`"5493764123456"` — código de país (54) + 9 + código de área sin el 0 (376) + número sin el 15.

## Cómo subir a GitHub

1. Entrá al repo nuevo que creaste (Saras-Kiss).
2. Arrastrá TODO el contenido de este zip (las carpetas `catalogo` y `admin`, y el archivo
   `config.js`) directo a la raíz del repo, igual que hacías con Punto Electro.
3. Activá GitHub Pages si todavía no lo hiciste (Settings → Pages → Branch: main).

## Cómo queda organizado

- `tu-usuario.github.io/Saras-Kiss/catalogo/` → el catálogo público, para mandarle el link
  a los clientes (por WhatsApp, redes, donde quieras). No necesita instalarse ni loguearse.
- `tu-usuario.github.io/Saras-Kiss/admin/` → la app de tu señora. Ahí se loguea con el
  correo y contraseña que le creaste en Firebase, y puede instalarla en su celu como PWA
  (en Chrome: menú → "Instalar app" o "Agregar a pantalla de inicio").

## Lo que ya funciona en esta versión

- Catálogo público: navegar productos, ver detalle con galería de fotos, armar pedido,
  manejo de "sin stock" al momento del pedido, y envío final por WhatsApp con el mensaje
  ya armado.
- Panel de tu señora: login, carga/edición de productos (con fotos, categoría, subcategoría,
  stock opcional), carga/edición de categorías y subcategorías libremente, y una bandeja de
  pedidos (los que van llegando por WhatsApp también quedan guardados acá, con un botón para
  abrir WhatsApp y responderle al cliente directo, y marcar el pedido como atendido).

## Novedad de esta entrega: módulo de Cuentas

Se agregó una pestaña nueva "Cuentas" en el panel de tu señora, con tres sub-secciones:

- **Resumen**: ganancia del período (semana/mes/año/todo), saldo en caja (incluye
  inyecciones de dinero aparte), desglose de ventas online/personales/gastos/inyecciones,
  ranking de productos más vendidos con promedio de venta, y un botón para compartir el
  resumen por WhatsApp.
- **Movimientos**: lista de todo lo que entra y sale de la caja, con filtros por tipo,
  categoría de gasto y producto. Las ventas de pedidos del catálogo se cargan solas
  cuando marcás "Atendido" en la pestaña de Pedidos. Además se puede cargar a mano:
  una venta personal (por fuera del catálogo), un gasto (con categoría, método de pago
  y foto de comprobante opcional), o una inyección de dinero (con el origen).
- **Categ. de gastos**: igual que las categorías de productos, las carga y edita tu
  señora libremente; arranca con algunas de ejemplo (Materiales, Envío, Herramientas,
  Empaque, Otros).

### ⚠️ Paso obligatorio en Firebase antes de usarlo

Este módulo usa dos colecciones nuevas en Firestore: `movimientos` y `categoriasGastos`.
Como son datos privados del negocio (plata, gastos), hay que agregarles reglas de acceso
en la consola de Firebase, si no la app no va a poder leer ni guardar nada ahí:

1. Entrá a la [consola de Firebase](https://console.firebase.google.com/), abrí el
   proyecto "saraskiss" (con la cuenta sasmivera@gmail.com).
2. En el menú izquierdo: **Firestore Database → Reglas**.
3. Vas a ver un bloque de texto con las reglas actuales (algo como `match /productos/{id} {...}`).
   Dentro de las llaves de `service cloud.firestore { match /databases/{database}/documents { ... } }`,
   agregá estas dos secciones nuevas (al mismo nivel que las que ya existen para
   `productos` y `categorias`):

   ```
   match /movimientos/{id} {
     allow read, write: if request.auth != null;
   }
   match /categoriasGastos/{id} {
     allow read, write: if request.auth != null;
   }
   match /camposEntrega/{id} {
     allow read: if true;
     allow write: if request.auth != null;
   }
   ```

4. Tocá **Publicar**.

Si no hacés este paso, al entrar a la pestaña "Cuentas" va a quedar cargando sin mostrar
nada (error de permisos).

## Novedad de esta entrega: estados del pedido + datos de entrega

**Pedidos** ahora tiene 4 pasos en vez de un solo "atendido":
- **Sin responder / Respondido**: se marca solo cuando tocás "Responder por WhatsApp"; si le
  contestaste por otro medio, hay un botón manual "Marcar respondido".
- **Sin cobrar / Cobrado**: botón manual "Marcar cobrado" — esto es lo que genera la venta
  en Cuentas (reemplaza al viejo "Marcar atendido").
- **Sin entregar / Entregado**: botón manual "Marcar entregado", aparece después de cobrado.

Arriba de la lista de pedidos aparece un resumen tipo "2 sin responder · 1 sin cobrar · 3 sin
entregar" para verlo de un vistazo. Los pedidos viejos (de antes de esta actualización) se
muestran igual, sin romperse: si ya estaban "atendidos" van a figurar como Respondido y
Cobrado, y Sin entregar (porque ese paso no existía antes).

**Datos de entrega**: en el catálogo, el checkout ahora pide además dirección, localidad,
referencia, forma de entrega, horario preferido y nota adicional (aparecen en el mensaje
de WhatsApp y quedan guardados en el pedido). El celular del cliente los recuerda para la
próxima compra (no hace falta cuenta ni login de su parte, es local a su navegador).

Estos campos **no están fijos**: en el panel de tu señora, dentro de la pestaña
"Categorías", más abajo hay un bloque "Campos de entrega del pedido" donde puede agregar,
editar o eliminar campos libremente (igual que hace con las categorías de productos).

### Nota sobre notificaciones push

Como hablamos, quedó descartada por ahora la notificación push al celular (hubiera
necesitado pasar a un plan de Firebase que pide cargar una tarjeta). La alerta de pedidos
sin atender sigue siendo el contador visual dentro de la app.

## Lo que falta para la próxima entrega

Como hablamos, esto lo dejamos para el siguiente lote de cambios:
- Clientes vinculados a sus compras (carpeta por cliente, como en Punto Electro)
- Sincronización/respaldo con Google Drive
- Que la app de administración funcione offline de verdad (por ahora necesita conexión
  para traer y guardar los datos, porque todo vive en Firebase)

## Nota sobre las fotos de los productos

Para no complicar la configuración de Firebase con un servicio de almacenamiento aparte
(que en Google ahora pide una cuenta de facturación aunque no gastes nada), las fotos se
guardan directamente adentro de cada producto, comprimidas automáticamente cuando las carga
tu señora. Anda bien para fotos de productos (no para videos pesados) — si más adelante
querés subir videos reales, avisame y lo resolvemos con otro método.
