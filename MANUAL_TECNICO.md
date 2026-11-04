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
- `flanders_products_v1`: Array con los **336 productos y promos** estructurados en 16 categorías (`promos`, `hielo`, `cervezas`, `botellas`, `vinos`, `whisky`, `espumantes`, `licores`, `gaseosas`, `hierbas`, `snacks`, `chocolates`, `chicles`, `postres`, `vapers`, `cigarrillos`).
- `flanders_motoboys_v1`: Registro activo de viajes asignados a **Moto 1** y **Moto 2** para cálculo de efectivo a rendir y comisiones de envío.
- `flanders_loyalty_v1`: Listado de socios del Club de Fidelidad y sus sellos acumulados.

### Llaves en la Nube (Cloud KV)
- `flanders_cfg_v1`: Cadena compacta `minOrder~deliveryFee~stampMin~isOpen~alias~phone`.
- `flanders_sin_stock_1` y `flanders_sin_stock_2`: IDs de productos pausados (`available: false`).
- `flanders_prices_1` .. `flanders_prices_4`: Pares `id:precio` de productos cuyo precio fue actualizado por el dueño.

---

## ⚙️ 3. Reglas de Negocio Implementadas

1. **Validación Estricta de Compra Mínima ($7.000 ARS) en Delivery:**
   - En modo `delivery`, si `subtotal < settings.minOrderAmount`, el botón `#btnConfirmOrderWA` se deshabilita (`disabled = true`) y renderiza `#minOrderWarningBox` indicando la diferencia exacta en ARS `$`.
   - En modo `takeaway` (Retiro en local), se omite la restricción y el costo de envío pasa a `$0`.
2. **Termómetro de Fidelidad (Piso $15.000 ARS):**
   - Evalúa en tiempo real `subtotal >= settings.stampMinAmount`. Si no alcanza el piso, calcula `stampMinAmount - subtotal` para estimular el up-selling dentro del modal del carrito.
3. **Liquidación de Repartidores (`Moto 1` y `Moto 2`):**
   - Separa los pedidos por `moto` (`1` o `2`) y discrimina `payType === 'cash'` (suma al efectivo a rendir por el cadete) vs `payType === 'transfer'` (no suma efectivo en mano, pero sí contabiliza el viaje y el envío).
4. **Vista Pública de Pase Digital (`?pass=ID`):**
   - Si la URL contiene el parámetro `?pass=`, oculta `#mainAppContainer` y muestra `#publicPassView` en modo solo lectura sin exponer controles administrativos.

---

## 🚀 4. Despliegue y Respaldos

- **Repositorio GitHub:** `https://github.com/JuanCDgo/bebidas`
- **GitHub Pages URL:** `https://juancdgo.github.io/bebidas/`
- **Google Drive Backup:** Carpeta `Mini Apps / bebidas`
