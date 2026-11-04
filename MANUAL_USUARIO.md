# 🍺 Manual de Usuario · Maldito Flanders (Delivery de Bebidas, Promos & Kiosco)

Bienvenido al manual operativo de la aplicación web de **Maldito Flanders**. Esta herramienta fue diseñada para agilizar la venta nocturna de bebidas, promos, kiosco, vapers y cigarrillos, controlar la caja de los **2 repartidores en moto**, automatizar el mensaje de lista de precios de WhatsApp y fidelizar clientes.

---

## 🚀 1. Puesta en Marcha y Configuración Inicial

1. Abrí la app y tocá el botón superior **`⚙️`** (o activá **`👨‍🍳 Modo Dueño: ON`** arriba a la derecha).
2. Completá tus datos reales en **Configuración**:
   - **Logo del Local:** Tocá *"📷 Subir Imagen"* para cargar el logo de *Maldito Flanders* desde tu celular o PC (se optimiza automáticamente).
   - **WhatsApp de Pedidos:** Poné el número con código de país (ej. `54911...` o tu característica local sin `+` ni espacios) donde caen los pedidos.
   - **Alias de Mercado Pago / CBU:** Tu alias para recibir transferencias y generar el código QR de mostrador.
   - **Mínimo de Compra para Envío:** Preconfigurado en **$7.000** (podés cambiarlo cuando quieras).
   - **Costo de Envío en Moto:** Preconfigurado en **$1.500** (100% editable).
   - **Monto Mínimo para Sello de Fidelidad:** Preconfigurado en **$15.000**.
   - **Nombres de los 2 Cadetes:** Podés personalizar **`🛵 Moto 1`** y **`🛵 Moto 2`** con el nombre de cada repartidor (ej. *Moto 1 - Franco*, *Moto 2 - Nahuel*).
3. Tocá **"💾 Guardar Configuración"**. Todos estos cambios se sincronizan en la nube para que los clientes los vean actualizados en sus teléfonos.

---

## 🔥 2. Cómo Administrar los 336 Productos, Precios y `[sin stock]`

En la barra superior, tocá **`👨‍🍳 Modo Dueño: OFF`** para pasarlo a **`ON`**:

### A. Marcar un Producto `🟢 En Stock` o `🔴 Sin Stock` en 1 toque
- Al lado de cada bebida, chocolate o vaper vas a ver el botón **`🟢 En Stock`** (o **`🔴 Sin Stock`** en los agotados como *Chivas*, *Mosquita Muerta*, *Pepsi 1.5L*, etc.).
- Tocalo una vez para pausarlo o activarlo:
  - Los productos **`🔴 Sin Stock`** no se borran: bajan automáticamente al final de su categoría, se ven en gris claro con cartel *"Sin Stock"* para que ningún cliente los pida por error, y cuando te vuelve a entrar mercadería los reactivás con 1 toque.

### B. Cambiar un Precio Individual o Aumentar por %
- **Precio individual:** Tocá el botón amarillo **`✏️ $...`** debajo del producto, escribí el precio nuevo y listo.
- **Aumento por categoría o general (`⚡ Aumentar %`):** Desde la barra de Modo Dueño tocá **`⚡ Aumentar %`**, elegí si querés aumentar *Todo el Catálogo* o solo una categoría (ej. *Cervezas*, *Cigarrillos*, *Promos*) y tocá **+5%**, **+10%** o **+15%**. El sistema redondea automáticamente a múltiplos de $100 para no dejar precios con monedas raras.

### C. Agregar Nuevas Promos o Productos
- Tocá **`+ Nuevo Producto`** en la barra de Modo Dueño, elegí la categoría (y el sub-filtro si es una Promo de Fernet, Vodka, Gin, etc.), poné el precio y guardá.

---

## 📋 3. Generador Automático de Lista Completa para WhatsApp

¿Querés seguir mandando tu lista larga de precios por WhatsApp o estados sin tener que editar renglón por renglón?
1. Activá **`👨‍🍳 Modo Dueño: ON`**.
2. Tocá el botón **`📋 Copiar Lista WA`**.
3. La app genera en 1 segundo el mensaje completo de texto con el encabezado (`MÍNIMO DE COMPRA $7.000, envío $1.500`), las 16 categorías ordenadas, todos los precios actualizados y la etiqueta `[sin stock]` en los agotados. Solo tenés que ir a WhatsApp y tocar **Pegar**.

---

## 🛵 4. Control de los 2 Repartidores (`Moto 1` y `Moto 2`) y Rendición de Caja

Tocá el botón **`🛵 Motos`** en la cabecera para abrir el panel de despacho:

1. **Asignar un Viaje:**
   - Elegí **`🛵 Moto 1`** o **`🛵 Moto 2`**.
   - Ingresá la dirección o nombre del cliente, el monto total a cobrar y si paga en **💵 Efectivo (rinde caja)** o si **💳 Ya pagó por Transferencia**.
   - Tocá **`+ Asignar Viaje`** (también podés importar directamente un carrito armado tocando *"🛵 Asignar directo a Moto 1 / Moto 2"* desde el carrito).
2. **Rendición de Efectivo al Final de la Noche:**
   - El panel te muestra por separado para **Moto 1** y **Moto 2**:
     - Cantidad de viajes realizados en el turno.
     - **Efectivo exacto en mano que debe entregarte cada cadete**.
     - **Total acumulado de envíos ($1.500 c/u)** de cada moto para liquidarle sus viajes sin usar calculadora ni papel.
   - Cuando el repartidor te entrega la plata, tocás **`✅ Rendir Caja Moto`** y su cuenta vuelve a cero.

---

## 🛒 5. Cómo Pide el Cliente (Freno de $7.000, Hielo y Vuelto Exacto)

1. El cliente busca o filtra por categoría (dentro de **Promos** tiene filtros rápidos por *Fernet, Smirnoff/Skyy/Vodka, Gin, Ron/Whisky, Aperitivos, Vino*).
2. Abre su pedido y tiene el recordatorio rápido **`🧊 ¿Sumás Hielo a la previa en 1 toque?`** (+1kg, +3kg, +12kg).
3. Elige temperatura (**`❄️ Bien Fría`** o **`📦 Al Natural`**) y forma de entrega:
   - Si elige **`🛵 Envío en Moto`** y no llega a los **$7.000**, la app frena el envío y le avisa cuánto le falta para alcanzar el mínimo.
   - Si elige **`🏃 Retiro en Local`**, puede comprar sin mínimo ni costo de envío.
4. Si paga en **Efectivo**, indica con cuánto paga (ej. `$20.000` o `$50.000`) y el mensaje de WhatsApp te llega con el cálculo listo: **`👉 LLEVAR VUELTO: $...`**.

---

## ⭐ 6. Club de Fidelidad (Sellos desde $15.000) y Cobro QR

- **Sellos de Fidelidad (`⭐`):** Cada compra que supera los **$15.000** suma 1 sello. Si al cliente le faltan unos pesos para llegar a $15.000, el carrito le avisa para incentivar que agregue un hielo o un snack. Podés enviarle a cada cliente su **Pase Digital por WhatsApp** con 1 toque.
- **Cobro QR Mostrador (`📷 QR`):** Muestra en pantalla completa tu código QR de Mercado Pago y tu Alias para cobrar en el local al instante.
- **Cartel QR Imprimible (`🪧`):** Genera el afiche con el logo de *Maldito Flanders* y el código QR del catálogo para pegar en el mostrador o subir a Instagram.
