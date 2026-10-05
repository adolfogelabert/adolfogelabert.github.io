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

## Pendientes de corrección (detectados en auditoría, aún no corregidos)
- [x] URLs YouTube mock reemplazadas por reales el 2026-10-05 en `dashboard.html:345,354,363,372`
- [ ] Stats hardcodeadas `dashboard.html:855,877`
- [ ] Placeholders de pago en `ventas.html:778-842` y `TU_HOTLINK_AQUI` en dashboard
- [ ] Foto de perfil remota `index.html:648` — diferida al final por el dueño
- [ ] `ventas.html` no lee config de pagos del dashboard (`localStorage productPaymentConfigs`)
- [ ] QR Yape/Plin/Binance solo hacen preview, no se persisten
