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

## Pendientes de corrección (detectados en auditoría, aún no corregidos)
- [x] URLs YouTube mock reemplazadas por reales el 2026-10-05 en `dashboard.html:345,354,363,372`
- [ ] Stats hardcodeadas `dashboard.html:855,877`
- [ ] Placeholders de pago en `ventas.html:778-842` y `TU_HOTLINK_AQUI` en dashboard
- [ ] Foto de perfil remota `index.html:648` — diferida al final por el dueño
- [x] `ventas.html` lee config del dashboard (`aplicarConfigPagosTienda`, 2026-10-05)
- [x] QR Yape/Plin/Binance se persisten (`qrDraft` + campo `qr`, 2026-10-05)
- [x] `ventas.html` renderiza productos desde dashboard (`renderProductosTienda`, `storeProducts` localStorage, 2026-10-05)
- [x] `ventas.html` muestra 3 productos demo si `storeProducts` está vacío (`PRODUCTOS_DEMO`, 2026-10-05)
- [x] Dashboard: acción Duplicar producto (`duplicateProduct`, 2026-10-05)
- [x] Login real con Firebase Auth en `dashboard.html` + `index.html` (Etapa 5, 2026-10-16) — **pendiente validar `apiKey`**
- [ ] Añadir dominios autorizados en Firebase → Authentication → Settings → Authorized domains: `localhost` y `adolfogelabert.github.io` (necesario para enlaces de recuperación de contraseña)
