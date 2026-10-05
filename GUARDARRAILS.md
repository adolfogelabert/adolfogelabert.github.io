# 🛡️ GUARDARRAILS — Proyecto Adolfo Gelabert

> Reglas estrictas para mantener la integridad del diseño, CSS y JS existentes.
> **No modifiques nada sin leer esto primero.**

---

## ⚠️ REGLA #1 — Siempre preguntar antes de ejecutar

**No ejecutes ninguna corrección, creación o modificación sin preguntarme antes.**

Cada cambio debe ser aprobado explícitamente. Incluso si parece obvio o urgente, pregunta primero. Yo confirmo o ajusto antes de que toques algo.

El flujo es:
1. Propón el cambio (qué, por qué, cómo)
2. Espera mi confirmación
3. Solo entonces ejecútalo

## ⚠️ REGLA #2 — Prohibido alucinar o decidir por cuenta propia

- No inventes rutas, funciones, IDs, clases ni contenido que no hayas verificado en el archivo real.
- No supongas que algo existe: léelo primero con `read` / `grep` / `glob`.
- No tomes decisiones de diseño, texto legal, precios o estructura por tu cuenta. Si falta un dato, pregunta.
- Si no estás seguro, dilo y propone opciones en lugar de adivinar.

## ⚠️ REGLA #3 — Siempre crear un punto de restauración

Antes de tocar **cualquier archivo**, deja una copia del estado actual para poder volver atrás si algo falla.

| Situación | Acción |
|---|---|
| Proyecto sin Git | Copia toda la carpeta del proyecto a `cv_interactivo_ajgs_BACKUP_YYYY-MM-DD/` (en el mismo nivel, fuera del proyecto) |
| Proyecto con Git | `git add . && git commit -m "checkpoint antes de [cambio]"` |
| Cambio mínimo (1 archivo) | Copia ese archivo como `archivo.html.BAK` en la misma carpeta |
| Cambio en CSS/JS inline | Antes de editar, comenta el bloque original en lugar de borrarlo, o guarda el estado del archivo |

**Limpieza de copias:** elimina las copias de seguridad innecesarias y deja solo la última vigente. Nada de acumular `BACKUP_1`, `BACKUP_2`, `BAK`, `BAK2`.

**No empieces a trabajar si no hay un punto de retorno claro.** Cada sesión arranca verificando desde dónde se continúa.

## ⚠️ REGLA #4 — Cambios solo en la sección puntual

Cada mejora es sobre una **sección específica** y debe resolverse **sin leer el documento completo ni desarmar archivos enteros**.

| ✅ Correcto | ❌ Incorrecto |
|---|---|
| Leer solo las líneas de la sección a modificar | Leer el HTML completo de 1700+ líneas |
| Editar solo la función o bloque necesario | Reestructurar o refactorizar todo el archivo |
| Hacer cirugía precisa, no amputación | Mover código, cambiar IDs, renombrar clases |

Si el cambio es en el slider → solo tocas el slider.  
Si es en el modal de pago → solo tocas el modal de pago.  
Si es en una página legal → solo tocas esa página.

No reordenes, refactorices ni "mejores" código que no está en la mira del cambio autorizado.

## ⚠️ REGLA #5 — Revisar lo último realizado y partir de ahí

- Al iniciar cada sesión, revisa el último cambio hecho (último backup, último commit, últimos archivos tocados).
- Nunca empieces desde cero ni repitas trabajo ya hecho.
- Si hay un `ERRORES.md`, léelo primero para no repetir fallos.

## ⚠️ REGLA #6 — Registro de errores obligatorio

- Todo error, causa y solución se registra en `ERRORES.md` (fecha, archivo, línea, qué pasó, cómo se corrigió).
- Antes de un cambio riesgoso, consulta ese registro.
- Sin registro no hay aprendizaje: si algo se rompió una vez, no se rompe dos veces igual.

## ⚠️ REGLA #7 — No romper lo que ya funciona

- Si una sección funciona (slider, modal, i18n, pagos UI, menú), no la toques como efecto colateral de otro cambio.
- Todo cambio termina con verificación: recarga la página afectada y confirma que lo anterior sigue funcionando.
- Si algo que funcionaba deja de funcionar, revierte al punto de restauración de inmediato y registra el error.

---

## 1. LÍNEA BASE — Lo que NO se puede modificar

### Diseño global
| Elemento | Regla |
|---|---|
| Paleta de colores | `--accent: #ca8a04` (oro), `--bg-dark: #0a0a0a` (fondo), `--card-dark: #161616` (tarjetas). No agregar nuevos colores base. |
| Tipografía | Headings: `'Space Grotesk'`, Body: `'Outfit'`. No cambiar ni agregar otra fuente. |
| Bordes redondeados | Mantener `rounded-3xl` en bento cards, `rounded-2xl` en tarjetas, `rounded-full` en badges. |
| Sombras | Solo `shadow-2xl` en elementos destacados. Nada de sombras adicionales. |

### CSS existente
- No borrar **ninguna** variable CSS de `:root` en ningún archivo.
- No eliminar clases de Tailwind del HTML. Si necesitas cambiar un estilo, **agrega** una clase personalizada, no reemplaces una existente.
- Los estilos del slider (`#productos-digitales .slider-*`), modal (`.modal-*`), menú hamburguesa (`.hamburger`), bento grid (`.bento-item`) son **intocables** — solo se permite agregar estilos complementarios.

### JS existente
| Archivo | No tocarlo |
|---|---|
| `index.html` | Canvas animation, slider logic, changeLang, productData, IntersectionObserver, smoothScroll |
| `dashboard.html` | state object, switchTab, renderCurrentTab, todos los CRUD handlers |
| `ventas.html` | openModal, openPayModal, showPayTab, copyText, changeLang, cookie consent |

---

## 2. CÓMO hacer cambios seguros

### Agregar CSS nuevo
```css
/* ✅ CORRECTO: crear una nueva clase sin tocar las existentes */
.mi-nuevo-componente { ... }

/* ❌ INCORRECTO: modificar una clase existente que ya se usa */
.bento-item { background: red; } /* ROMPE TODO */
```

### Agregar JS nuevo
```js
// ✅ CORRECTO: función nueva con nombre único
function miNuevaFuncionalidad() { ... }

// ✅ CORRECTO: escuchar eventos sin interferir
document.addEventListener('mi-evento', handler);

// ❌ INCORRECTO: sobreescribir funciones existentes
function changeLang() { ... } // SOBREESCRIBE LA ORIGINAL
```

### Modificar HTML
- **Nunca** quites atributos `data-es`, `data-en` de elementos con clase `.i18n`.
- **Nunca** quites `onclick`, `onsubmit` o IDs del HTML — están vinculados al JS.
- Si agregas una nueva sección, usa la misma estructura bento existente.

---

## 3. Convenciones del proyecto

| Convención | Regla |
|---|---|
| Archivos nuevos | Solo HTML plano (sin frameworks). Si necesitas backend, crear archivo `.php` nuevo. |
| Imágenes nuevas | Colocar en `/images/`. Usar `.webp` si es posible. Máximo 200 KB. |
| Enlaces externos | Siempre con `target="_blank"` y `rel="noopener noreferrer"`. |
| i18n | Todo texto visible debe tener atributos `data-es` / `data-en`. |
| Footer | Idéntico en todas las páginas. Si cambias uno, cámbialos todos. |

---

## 4. Dependencias externas (NO ACTUALIZAR sin probar)

| Recurso | Versión fija |
|---|---|
| Tailwind CSS | `https://cdn.tailwindcss.com` (CDN) |
| Font Awesome | `https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css` |
| Google Fonts | `Space+Grotesk:wght@300;500;700` + `Outfit:wght@200;400;900` |
| Google Analytics | `G-ZLZQBRFRYM` — No tocar el script |

> No agregar nuevas librerías JS externas sin aprobación. Prefierelimplementar con JS vanilla (como ya está el proyecto).

---

## 5. CheckList antes de guardar cambios

- [ ]  ¿Agregaste CSS nuevo sin modificar el existente?
- [ ]  ¿Las clases `.i18n` mantienen sus atributos `data-es` y `data-en`?
- [ ]  ¿Los modales, slider y menú móvil siguen funcionando?
- [ ]  ¿El toggle ES/EN sigue traduciendo todo?
- [ ]  ¿La paleta de colores sigue siendo la misma?
- [ ]  ¿No rompiste ningún `onclick` del HTML?
- [ ]  Probaste en **mobile** (320px) y **desktop** (1920px)?

---

## 6. 🔴 Zonas de alto riesgo (prohibido editar sin permiso)

```
index.html:44-48      → Variables CSS :root
index.html:210-375    → Slider productos digitales
index.html:450-510    → Modal productos
index.html:1386-1447  → Canvas animation + particles
index.html:1522-1538  → changeLang function
dashboard.html:232-439 → Objeto state (base de datos local)
dashboard.html:789+   → renderCurrentTab (motor de vistas)
ventas.html:306-510   → Modal de pago completo
ventas.html:856-1036  → Lógica de modales y pagos
```

---

## 7. ¿Algo se rompió?

```bash
# 1. Revisa que los cambios sean solo en lo que tocaste
git diff --name-only

# 2. Busca errores de sintaxis en HTML
# Abre cada archivo en navegador y revisa la consola (F12)

# 3. Verifica que no hayas duplicado IDs
# Busca en el archivo: id=" (cada ID debe ser único)
```

---

*Si no respetas estas reglas, el diseño se rompe, el slider deja de funcionar, los modales no abren, y el toggle de idioma muere. No digas que no te avisé.*
