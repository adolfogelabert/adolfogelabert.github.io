# 📕 REGISTRO DE ERRORES — Proyecto Adolfo Gelabert

> Exigido por `GUARDARRAILS.md` REGLA #6. Todo error se registra con fecha, archivo, línea, causa y solución.

## Formato
| Fecha | Archivo:línea | Qué pasó | Causa | Solución |
|---|---|---|---|---|

## Historial

| Fecha | Archivo:línea | Qué pasó | Causa | Solución |
|---|---|---|---|---|
| 2026-09-17 | `terminos.html:374` | Typo `secancela` pegado en `data-es` | Error de tipeo original | Separado a `se cancela`. EN ya estaba bien |
| 2026-09-17 | `terminos.html` / `terminos-condiciones.html` | Contenido mezclado: servicios hablaba de productos y viceversa | Redacción original combinada | `terminos.html` dejado solo servicios (11 secciones), `terminos-condiciones.html` solo productos (11 secciones) |
| 2026-09-17 | `dashboard.html` / `index.html` | Sin control de acceso al panel | No existía login | Puerta visual temporal (`login-gate` + `siteLoginModal`, credenciales `admin`/`gelabert2026`) hasta Firebase |

| 2026-10-05 | repo GitHub `main` | Push rechazado por divergencia con remoto (versión vieja con README, foto y PDF) | `git init` local sin historial remoto | Rescate de 3 archivos + `push --force-with-lease`, deploy OK |
| 2026-10-05 | repo GitHub `main` | `.BAK` públicos exponían clave temporal en texto plano | Backups dentro de carpeta publicada | Movidos a `..\respaldo_cv_2026-10-05\` fuera del repo y eliminados del tracking |
| 2026-10-05 | `ventas.html:788` | Botón PayPal redirigía a `TU_LINK_PAYPAL` | Sin link real | Pre-cargado con `https://paypal.me/adolfogelabert?locale.x=es_XC&country.x=VE` |
| 2026-10-05 | `ventas.html:796,799` | QR Yape placeholder + tel genérico | Sin datos reales | `images/qr_yape-plin.jpg` + `+51 989 013 043` |
| 2026-10-05 | `ventas.html:809,812` | QR Plin placeholder + tel genérico | Sin datos reales | `images/qr-plin_adolfo_gelabert.jpg` + `+51 989 013 043` |
| 2026-10-05 | `ventas.html:822` | QR Binance placeholder | Sin QR real | `images/qr_binance.jpg` |
| 2026-10-05 | `ventas.html:789` | Solo botón PayPal | Faltaba dato de confianza | Añadidos correos `adolfo.1974@gmail.com` y `adolfo_gelabert@hotmail.com` como info bajo el botón |
| 2026-10-05 | `ventas.html:825` | Wallet Binance placeholder | Sin address real | `0xd45bad4c27a4e5e1a72ecf73f428f44264be90a3` (USDT BEP-20) |
| 2026-10-05 | `ventas.html:836-842` | Datos bancarios placeholder | Faltaba info real | BCP, Soles `192-94565740-0-47`, CCI `00219219456574004734`, titular "Adolfo Jose Gelabert Semeco" |
| 2026-10-16 | `dashboard.html` / `index.html` | Login temporal con credenciales en texto plano (`admin`/`gelabert2026`, `sessionStorage.dash_auth`) | Auth provisional (fila 2026-09-17) | Reemplazado por Firebase Auth 13.0.0 modular: `signInWithEmailAndPassword`, persistencia local/session ("Recordarme"), `sendPasswordResetEmail`, `signOut`. Eliminados `DASH_AUTH_TEMP`/`SITE_AUTH_TEMP` |
| 2026-10-16 | Firebase `apiKey` (prueba `accounts:signInWithPassword` + `firebaseinstallations`) | Google responde `400 API_KEY_INVALID` en 2 APIs distintas | La key pegada tenía 3 caracteres mal copiados (`V`→`v`, `F`→`f`, `I`→`l` antes de `1`) | Reemplazada por `AIzaSyDXvD55QfZ0ugr5RfYTQvl1rjzu3r4GgSY` (verificada: ya no devuelve `API_KEY_INVALID`) |
| 2026-10-16 | Firebase proyecto `cv-interactivo-ajgs` | `accounts:signInWithPassword` responde `CONFIGURATION_NOT_FOUND` con la key válida | Authentication no aprovisionado (no se hizo click real en "Get started" en el panel Auth) | **BLOQUEADO**: pedir al dueño confirmar en Firebase Console → Build → Authentication → que Email/Password esté en estado "Enabled" |
| 2026-10-17 | `firestore.rules` / Firestore `(default)` | Escritura y borrado **anónimos** abiertos: cualquiera podía crear/borrar `products` y leer `orders` (probado con REST: HTTP 200) | Repo sin `firestore.rules` ni `firebase.json`; consola con reglas por defecto | Creado `firestore.rules` (products: lectura pública / escritura con sesión; orders: creación anónima validada solo-`pendiente`, lectura/cambio solo sesión; resto cerrado) + `firebase.json`; publicado en consola y verificado: escritura anónima ahora HTTP 403, compra anónima OK (orden `b4RlIvffp32uJ6tik0vw`) |
| 2026-10-17 | `ventas.html:873,870,872` (antes) | **Compra rota**: `sendOrder()` lanzaba `ReferenceError: currentPaymentMethod is not defined` y `openModal()` veía `productListTienda=[]` → pedido nunca se guardaba y el modal de producto no abría | `var currentPaymentMethod`, `var currentProductForOrder`, `var productListTienda` estaban declarados *dentro* del callback `DOMContentLoaded` del primer `<script>`, pero `sendOrder`/`comprarAhora` viven en otro `<script>` (scope separado: copias distintas o indefinidas) | Subidas las 3 declaraciones al top-level del bloque (globales compartidas); probado con harness Node: orden capturada en Firestore stub + botón "✅ Pedido enviado" |
| 2026-10-17 | `dashboard.html:1977` (orders) | XSS reflejado: `o.receiptUrl` (campo que escribe el comprador **anónimo**) se inyectaba crudo en `href` → `javascript:` ejecutable en sesión admin | Sin validación de protocolo ni escape | Solo se renderiza si coincide `^https?:\/\/` y con `escapeHtml()`; `status` de la tabla también escapado; mensajes de bandeja escapados (`nombre`, `email`, `asunto`, `fecha`, `mensaje`) |
| 2026-10-17 | `ventas.html:697` | Botón Stripe con `href="TU_LINK_STRIPE"` muerto (pestaña activa por defecto) | Link no configurado | `href="#"` + `aplicarConfigPagosTienda` oculta la pestaña Stripe y activa PayPal cuando no hay link configurado (ids `pay-tab-stripe`/`pay-tab-paypal`) |
| 2026-10-17 | `dashboard.html` home | Stats hardcodeadas `1,240 / 843 / 9,120` en tarjetas de tráfico (fila pendientes 2026-09-17) | Valores fijos en HTML inline | Nuevo `state.siteMetrics` + form editable (`sm-form`) y `persistMetrics()`/`loadPersistedMetrics()` en `localStorage['dashMetrics']` — también persiste `brandMetrics` (antes se perdía al recargar) |
| 2026-10-17 | `dashboard.html:459,472,485,971,994,1672` | Placeholders `TU_HOTLINK_AQUI`/`TU_LINK_AQUI` visibles en producto | Defaults de ejemplo | Defaults `""`, input `type="url"` con placeholder real, y normalización al cargar Firestore (`/^TU_[A-Z_]+$/` → `""`) |
| 2026-10-17 | `dashboard.html` bandeja (inbox) | Las consultas solo se marcaban (`updateMessageStatus`) — **no había opción de borrar** y los cambios de estado no sobrevivían a la recarga | Faltaba `deleteMessage`; `state.messages` solo existía en memoria | Añadido `deleteMessage(id)` con `confirm()` + botón "🗑️ Eliminar" por tarjeta, y `persistMessages()`/`loadPersistedMessages()` en `localStorage['dashMessages']` (cargado en `window.onload`). Nota: la bandeja es contenido demo hardcodeado, no hay formulario que la llene |

## Pendientes de corrección (detectados en auditoría, aún no corregidos)
- [x] URLs YouTube mock reemplazadas por reales el 2026-10-05 en `dashboard.html:345,354,363,372`
- [x] Stats hardcodeadas `dashboard.html` (antes 855,877) — resueltas 2026-10-17 (`state.siteMetrics` editables + persistencia)
- [x] Placeholders de pago: `TU_LINK_STRIPE` (`ventas.html:697`) resuelto 2026-10-17; QR/bancarios ya reales desde 2026-10-05. Queda `TU_HOTLINK_AQUI` → resuelto 2026-10-17
- [ ] Foto de perfil remota `index.html:648` — diferida al final por el dueño
- [x] `ventas.html` lee config del dashboard (`aplicarConfigPagosTienda`, 2026-10-05)
- [x] QR Yape/Plin/Binance se persisten (`qrDraft` + campo `qr`, 2026-10-05)
- [x] `ventas.html` renderiza productos desde dashboard (`renderProductosTienda`, `storeProducts` localStorage, 2026-10-05)
- [x] `ventas.html` muestra 3 productos demo si `storeProducts` está vacío (`PRODUCTOS_DEMO`, 2026-10-05)
- [x] Dashboard: acción Duplicar producto (`duplicateProduct`, 2026-10-05)
- [x] Login real con Firebase Auth en `dashboard.html` + `index.html` (Etapa 5, 2026-10-16) — `apiKey` ya válida y Auth habilitado (verificado 2026-10-17: responde `INVALID_LOGIN_CREDENTIALS`, no `CONFIGURATION_NOT_FOUND`)
- [ ] Añadir dominios autorizados en Firebase → Authentication → Settings → Authorized domains: `localhost` y `adolfogelabert.github.io` (necesario para enlaces de recuperación de contraseña)
- [x] Firestore con reglas de seguridad (2026-10-17, ver arriba)
- [ ] Orden de prueba `b4RlIvffp32uJ6tik0vw` (creada durante la auditoría 2026-10-17) — borrarla desde el dashboard o consola de Firestore
- [ ] XSS restante: `p.titulo`/`p.descripcion` de productos y proyectos se insertan sin escapar en `renderProductCard`/portafolio (`dashboard.html`). Riesgo bajo porque `products` exige sesión para escribir, pero recomendado escapar
- [ ] Código duplicado: config Firebase + Cloudinary copiados en `index.html`, `ventas.html` y `dashboard.html` → extraer a `js/firebase-init.js`
- [ ] Tailwind CDN (`cdn.tailwindcss.com`) en 7 páginas — no recomendado para producción (compilar vía CLI)
- [ ] Imágenes pesadas: `foto_perfil_ajgs.jpg` 902KB duplicada en raíz e `images/`; `producto1-3.jpg` sin comprimir
