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

## Lo que falta para la próxima entrega

Como hablamos, esto lo dejamos para el siguiente lote de cambios:
- Cargar la **venta** en la app de tu señora a partir de un pedido atendido (con costo,
  ganancia, etc.)
- Clientes vinculados a sus compras (carpeta por cliente, como en Punto Electro)
- Caja general del negocio (gastos aparte de las ventas)
- Resumen por semana/mes/año con ganancia neta
- Sincronización/respaldo con Google Drive
- Que la app de administración funcione offline de verdad (por ahora necesita conexión
  para traer y guardar los datos, porque todo vive en Firebase)

## Nota sobre las fotos de los productos

Para no complicar la configuración de Firebase con un servicio de almacenamiento aparte
(que en Google ahora pide una cuenta de facturación aunque no gastes nada), las fotos se
guardan directamente adentro de cada producto, comprimidas automáticamente cuando las carga
tu señora. Anda bien para fotos de productos (no para videos pesados) — si más adelante
querés subir videos reales, avisame y lo resolvemos con otro método.
