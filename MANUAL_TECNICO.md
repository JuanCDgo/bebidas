# 🛠️ Manual Técnico · Maldito Flanders (Delivery de Bebidas & Kiosco)

Documentación de arquitectura, modelo de datos, sincronización en la nube y lógica operativa de la micro-aplicación `bebidas`.

---

## 🏗️ 1. Arquitectura y Stack

- **Tipo de Aplicación:** Single-Page Application (SPA) responsive orientada a mobile-first (Android / iOS / Desktop).
- **Tecnologías:** HTML5, Vanilla JavaScript (ES6+), Tailwind CSS.
- **Motor de Códigos QR:** Doble motor redundante (`qrcode.min.js` local + CDN fallback + `QuickChart.io` API fallback) garantizando que nunca exista un contenedor QR vacío.
- **Sincronización Cloud + LocalStorage:**
  - Persistencia local inmediata en `localStorage`.
  - Sincronización en la nube vía KeyVal API (`keyvalue.immanuel.co`, AppKey `ozyom7gs`) para propagar cambios de stock (`[sin stock]`), precios modificados y parámetros operativos (*compra mínima, costo de envío, estado abierto/cerrado, alias y WhatsApp*) entre el teléfono del dueño y los teléfonos de los clientes.

---

## 🗄️ 2. Esquema de Almacenamiento (`localStorage` & Cloud KV)

### Llaves Locales (`localStorage`)
- `flanders_settings_v1`: Objeto de configuración general (`businessName`, `whatsappPhone`, `paymentAlias`, `minOrderAmount`, `deliveryFee`, `stampMinAmount`, `moto1Name`, `moto2Name`, `isOpen`, `businessLogo`, `targetStamps`, `rewardText`).
- `flanders_products_v1`: Array con los **336 productos y promos** estructurados en 16 categorías (`promos`, `hielo`, `cervezas`, `botellas`, `vinos`, `whisky`, `espumantes`, `licores`, `gaseosas`, `hierbas`, `snacks`, `chocolates`, `chicles`, `postres`, `vapers`, `cigarrillos`). Cada producto contiene `salesCount` calculado.
- `flanders_motoboys_v1`: Registro activo de viajes asignados a **Moto 1** y **Moto 2** para cálculo de efectivo a rendir y comisiones de envío.
- `flanders_loyalty_v1`: Listado de socios del Club de Fidelidad (`id`, `name`, `phone`, `address`, `stamps`).
- `flanders_sales_v1`: Historial de ventas registradas (`id`, `orderId`, `productId`, `productName`, `qty`, `unitPrice`, `date`, `time`, `channel`, `clientName`) con relación 1:N hacia `products`.

### Llaves en la Nube (Cloud KV)
- `flanders_cfg_v1`: Cadena compacta `minOrder~deliveryFee~stampMin~isOpen~alias~phone`.
- `flanders_sin_stock_1` y `flanders_sin_stock_2`: IDs de productos pausados (`available: false`).
- `flanders_prices_1` .. `flanders_prices_4`: Pares `id:precio` de productos cuyo precio fue actualizado por el dueño.

---

## ⚙️ 3. Reglas de Negocio Implementadas

1. **Ranking Automático por Frecuencia de Ventas:**
   - La función `recordSalesFromCurrentCart()` almacena las transacciones en `salesHistory` y actualiza `salesCount` en cada ítem.
   - Algoritmo de ordenamiento en `renderCatalog()`:
     1. Filtra disponibilidad: `available: true` primero, `available: false` (sin stock) al final.
     2. Para disponibles: Ordena descendentemente por `salesCount` (`b.salesCount - a.salesCount`).
     3. Productos con 0 ventas se ubican a continuación de los vendidos, manteniendo su posición original.
2. **Autocompletado Predictivo de Clientes:**
   - Listener reactivo sobre `#orderClientName` ejecutando búsqueda con `includes()` sobre `loyaltyMembers`.
   - Al seleccionar un socio: inyecta automáticamente su nombre, dirección habitual (`#orderStreet`) y despliega su saldo de sellos.
3. **Cancelación Segura de Pedidos:**
   - `cancelCurrentOrder()` realiza limpieza total de estado en memoria del carrito y resetea los campos del formulario tras confirmación.
4. **Validación Estricta de Compra Mínima ($7.000 ARS) en Delivery:**
   - En modo `delivery`, si `subtotal < settings.minOrderAmount`, el botón `#btnConfirmOrderWA` se deshabilita (`disabled = true`) y renderiza `#minOrderWarningBox` indicando la diferencia exacta en ARS `$`.
   - En modo `takeaway` (Retiro en local), se omite la restricción y el costo de envío pasa a `$0`.
5. **Termómetro de Fidelidad (Piso $15.000 ARS):**
   - Evalúa en tiempo real `subtotal >= settings.stampMinAmount`. Si no alcanza el piso, calcula `stampMinAmount - subtotal` para estimular el up-selling dentro del modal del carrito.
6. **Liquidación de Repartidores (`Moto 1` y `Moto 2`):**
   - Separa los pedidos por `moto` (`1` o `2`) y discrimina `payType === 'cash'` (suma al efectivo a rendir por el cadete) vs `payType === 'transfer'` (no suma efectivo en mano, pero sí contabiliza el viaje y el envío).
7. **Vista Pública de Pase Digital (`?pass=ID`):**
   - Si la URL contiene el parámetro `?pass=`, oculta `#mainAppContainer` y muestra `#publicPassView` en modo solo lectura sin exponer controles administrativos.

---

## 🚀 4. Despliegue y Respaldos

- **Repositorio GitHub:** `https://github.com/JuanCDgo/bebidas`
- **GitHub Pages URL:** `https://juancdgo.github.io/bebidas/`
- **Google Drive Backup:** Carpeta `Mini Apps / bebidas`

---

## 🔐 Separación Comprador / Dueño (arquitectura)

**Flujo de arranque (`DOMContentLoaded`):** `loadAllData()` → si hay `?admin` (y no `?pass`) → `IS_OWNER_VIEW = ownerSessionValid() || await requestOwnerAccess()` → si `!IS_OWNER_VIEW` → `enterPublicView()` → render normal.

**Vista comprador (`enterPublicView`):** elimina del DOM todo `[data-owner-only]`, `#ownerToolbar` y los modales `motoboys, qrCobro, flyerQr, loyalty, editMember, product, bulkPrice, settings`; vacía `loyaltyMembers`; limpia `?admin` de la URL.
**Guardias:** un array de ~35 nombres de funciones de dueño se envuelve en `window[name]` para que no hagan nada si `!IS_OWNER_VIEW` (incluye `pushCatalogChangesToCloud` y `recordSalesFromCurrentCart`).
Para agregar una función de dueño nueva: sumarla a esa lista y marcar su botón con `data-owner-only`.

**Acceso dueño:** modal creado dinámicamente (no existe en el HTML público). PIN → `SHA-256('flanders-owner|' + pin)` en `localStorage` `flanders_owner_pin_v1` (fallback FNV-1a si no hay `crypto.subtle`). Sesión: `flanders_owner_session_v1` (timestamp de vencimiento, 30 días, se renueva). Bloqueo: `flanders_owner_lock_v1` `{n, until}` (5 intentos → 5 min).

**Estado del local:** `settings.storeStatus ∈ {open, pickup, closed}` (`getStoreStatus()` deriva de `isOpen` si no existe). Nube: campo 4 de `flanders_cfg_v1` → `'1'`/`'2'`/`'0'`. Carrito: `enforceStoreStatusInCart()` (pickup) y bloqueo del botón de envío en `updateCartTotalsUI()`/`sendOrderToWhatsApp()` (closed). El dueño queda exento.

**Límite de seguridad (importante):** esto protege la *interfaz*. Al ser un sitio estático, un cliente técnico puede leer el código. Lo que cualquiera podría escribir es la sincronización en la nube (`keyvalue.immanuel.co`, sin autenticación). Ver `HANDOFF_AGENTE.md` §3 para la solución propuesta (firma ECDSA de los datos).

**Tests:** `scratch/test_vistas.js`, `test_flujos.js`, `test_pin.js` (Chrome headless).
