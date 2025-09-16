# Documentación Técnica — Clínica Dental **Sonrisa+** (One‑Page)

> **Autor:** Gerson Montenegro
> **Proyecto:** Sitio público informativo de una clínica dental en Escuintla
> **Stack:** HTML5 + CSS (inline) + Bootstrap 5.3.2 + Bootstrap Icons 1.11.1 + JavaScript (vanilla)
> **Arquitectura:** sitio estático *one‑page* (un único `index.html`) apto para GitHub Pages / Netlify / Vercel (modo estático)

---

## 0. Objetivos de la solución

* **Presentación profesional** para rubro salud con identidad visual (teal/verde azulado).
* **Cero dependencias de backend** para render: todo es estático.
* **Interacción ligera en cliente:**

  * Scroll suave entre secciones.
  * Animaciones *fade‑in* on‑scroll.
  * **Contadores** de métricas (KPI) al entrar en viewport.
  * Botón **scroll‑top**.
  * **Formulario** con validación en cliente + *hook* listo para integrar a un endpoint `POST`.
* **Accesible y responsivo** (Bootstrap Grid + meta viewport).
* **Desplegable** en minutos en GitHub Pages/Netlify.

---

## 1. Estructura del proyecto

```
/ (raíz)
├─ index.html                 # Único archivo con HTML+CSS+JS
└─ /assets/                   # Carpeta sugerida para recursos estáticos
   └─ /img/
      ├─ hero.svg             # Fondo hero (referenciado en CSS)
      └─ historia.svg         # Imagen sección “Nosotros”
```

> *El proyecto funciona incluso sin `/assets` si reemplazas las imágenes por URLs públicas. Mantén rutas relativas y respeta mayúsculas/minúsculas.*

---

## 2. Dependencias y versiones

**CDN (producción):**

* Bootstrap CSS **5.3.2**: `https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css`
* Bootstrap Icons **1.11.1**: `https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.1/font/bootstrap-icons.css`
* Bootstrap Bundle **5.3.2** (incluye Popper): `https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js`

**Razonamiento:**

* Un solo *bundle* reduce número de requests (incluye Popper para tooltips/menus).
* Iconos consistentes con el ecosistema Bootstrap.
* Sin jQuery (no necesario para los componentes usados).

**Requisitos de ejecución:**

* Navegador moderno (Chromium/Firefox/Safari/Edge actuales).
* **Conexión a Internet** para cargar CDN. Para *offline*, hospeda los archivos localmente y actualiza `<link>`/`<script>`.

---

## 3. Identidad visual y estilos

### 3.1 Variables CSS (en `:root`)

```css
--color-primario: #0aa3a3;   /* Navbar, énfasis */
--color-oscuro:   #0b6262;   /* Stats, footer */
--color-acento:   #00d3b7;   /* Chips / detalles */
--color-texto:    #263238;   /* Texto base */
--color-gris:     #f7fbfb;   /* Fondo suave */
```

* Facilitan **tematización**: cambiar branding es tan simple como ajustar estas variables.

### 3.2 Clases clave

* `.navbar-custom`: color de fondo + sombra.
* `.hero-section`: **gradiente + imagen** (`url(./assets/img/hero.svg)`) + tipografía clara.
* `.chip`: *píldoras* informativas.
* `.service-card`: tarjetas elevadas con *hover*.
* `.stats-section`: bloque oscuro para contraste de KPIs.
* `.footer-custom`: pie con tono más oscuro que `.stats-section`.
* `.scroll-top`: botón flotante circular (oculto hasta 300px de scroll).
* `.fade-in` / `.fade-in.visible`: animación de entrada al hacer scroll.

### 3.3 Buenas prácticas CSS

* CSS propio **mínimo**, priorizando utilidades Bootstrap (espaciados, grid, sombras, tipografía).
* Evitar *!important* salvo en la barra (`.navbar-custom`) para garantizar contraste.

---

## 4. HTML semántico y accesibilidad (A11y)

* Estructura por **secciones**: `nav`, `section#inicio`, `section#nosotros`, `section.stats-section`, `section#servicios`, `section#especialidades`, `section#contacto`, `footer`.
* **Navbar fija** con botón *hamburguesa*.
* Enlaces internos `href="#…"` con **scroll suave**.
* Imágenes con `alt` descriptivo.
* Control de foco natural (no se manipula con JS).
* Contraste verificado: primario sobre blanco (#0aa3a3) y texto sobre fondos oscuros.
* Iconografía vía *Bootstrap Icons* (decorativos, no críticos).
* **Responsive**: Bootstrap Grid (containers, rows, columns) y `meta viewport`.

---

## 5. JavaScript: arquitectura de comportamiento

### 5.1 Organización

* Un único bloque `<script>` al final del `body`, envuelto en `DOMContentLoaded`.
* **Funciones puras y pequeñas**:

  * *Scroll suave*
  * *Revelado.* `reveal()` alterna `.visible` según posición en viewport.
  * *Contadores.* `runCounters()` anima `.counter` hasta `data-target`.
  * *Scroll‑top.* Muestra/oculta botón y sube con *smooth*.
  * *Formulario.* Valida y deja *hook* a `fetch()` (comentado) para backend.

### 5.2 Eventos principales

* `click` en enlaces internos → **scroll suave** calculando `offset` de la navbar:
  `const top = target.offsetTop - navbar.offsetHeight;`
  `window.scrollTo({ top, behavior: 'smooth' })`
* `scroll` (window) → `reveal()` + visibilidad de scroll‑top + `checkStats()`.
* `submit` en `#contactForm` → validación + envío simulado; reemplazable por `fetch()`.

### 5.3 Algoritmos

**(a) Detección de visibilidad simple**

```js
const isIn = el => el.getBoundingClientRect().top < (innerHeight - 80);
```

* Umbral de 80px para iniciar animación ligeramente antes de que el elemento sea totalmente visible.

**(b) Contadores (KPI)**

```js
const runCounters = () => document.querySelectorAll('.counter').forEach(c => {
  const target = parseInt(c.dataset.target||'0',10);
  let v = 0; const step = Math.max(1, Math.floor(target/100));
  const tick = () => { v += step; if(v<target){ c.textContent = v; requestAnimationFrame(tick);} else c.textContent = target; };
  tick();
});
```

* Aumenta en pasos (≈1%) por `requestAnimationFrame` (\~60fps).
* Dispara **una sola vez** al entrar a `.stats-section` (flag `started`).

**(c) Validación formulario**

* Requeridos: `nombre`, `email`, `mensaje`.
* Regex email: `/^[^\s@]+@[^\s@]+\.[^\s@]+$/`.
* Si falla, `alert()` y `return`.
* Si pasa, *simula* éxito con `alert()` y `reset()`; para producción, **descomentar `fetch()`**.

### 5.4 Hook de backend (sugerido)

```js
// const res = await fetch("https://api.tu-dominio.com/citas", {
//  method: "POST", headers: {"Content-Type":"application/json"},
//  body: JSON.stringify(payload)
//});
```

* **Endpoint**: `/citas` (POST)
* **Payload**: `{ nombre, email, telefono?, mensaje }`
* **Respuesta esperada**: `201 Created` ó `200 OK`; manejar `res.ok` y mensajes de usuario.

**Seguridad/Privacidad:**

* No enviar datos sensibles sin HTTPS.
* Limitar CORS en el backend al dominio del sitio.
* Validar/sanear del lado servidor y proteger contra spam (captcha/honeypot/ratelimit).

---

## 6. Contenido por sección (copy y componentes)

* **HERO**: *claim* + CTA dobles (Servicios / Reservar).
* **NOSOTROS**: texto de confianza + *bullets* (equipo, procesos, familia, precios).
* **STATS**: 4 KPI con iconos: Pacientes/Años/Blanqueamientos/Satisfacción.
* **SERVICIOS**: 3 tarjetas (Profilaxis, Estética, Urgencias) + *chips* de convenios/pagos.
* **ESPECIALIDADES**: grid de 4 con icono y breve descripción.
* **CONTACTO**: tarjeta de información y tarjeta con formulario validado.
* **FOOTER**: navegación rápida, horario, contacto, redes.

---

## 7. SEO básico

* `<title>` específico: *Clínica Dental Sonrisa+ - Escuintla*.
* Contenido semántico y jerarquía H1→H2→H5.
* Texto alternativo en imágenes.
* URLs *one‑page* (anclas) amigables.
* Meta `description` (se puede añadir si se desea enriquecer snippets).

---

## 8. Rendimiento

* CDNs minificados → menos peso y mejor caché pública.
* **Imágenes**: usar SVG/WEBP cuando sea posible. Activado `loading="lazy"` en `#nosotros`.
* JS al final del body.
* Animaciones con `transform/opacity` → **evitan layouts costosos**.

---

## 9. Despliegue

### 9.1 GitHub Pages

1. Crear repo → subir `index.html` y `/assets/img`.
2. **Settings → Pages** → *Branch* `main`, *Root* `/`.
3. URL: `https://USUARIO.github.io/NOMBRE_REPO/`.

### 9.2 Netlify

1. *New site from Git* → conecta repo.
2. **Build command**: *(vacío)*. **Publish directory**: `/` (raíz).
3. Habilita dominio y HTTPS automático.

### 9.3 Vercel (estático)

* *Import Project* → *Framework Preset*: **Other**.
* *Build Command*: vacío. *Output Directory*: `/`.

---

## 10. Pruebas y QA

**Checklist funcional**

* [ ] Navegación interna con desplazamiento suave.
* [ ] Animaciones *fade‑in* inician al hacer scroll.
* [ ] Contadores aumentan **una sola vez** al entrar en stats.
* [ ] Botón scroll‑top aparece tras 300px y vuelve al inicio.
* [ ] Formulario: bloqueo al faltar campos / email inválido; *alert* de éxito y `reset`.
* [ ] Layout responsive (XS→XL) sin solapamientos.

**Compatibilidad**

* [ ] Chrome/Chromium 122+, Firefox 120+, Safari 16+, Edge 122+.
* [ ] Modo oscuro del sistema: (no aplica toggle; paleta está definida por variables).

**Accesibilidad**

* [ ] `alt` en imágenes, foco visible, contrastes mínimos AA.
* [ ] Navegación por teclado.

---

## 11. Mantenimiento y extensiones

* **Separar** CSS/JS a `/assets/css/custom.css` y `/assets/js/app.js` si crece el código.
* **Analytics**: añadir script (ej. Plausible/GA4) al `<head>` con *defer*.
* **SEO local**: página de *Ubicación* con `schema.org/LocalBusiness`.
* **Blog/Noticias**: añadir sección dinámica (desde JSON o CMS *headless*).
* **i18n**: duplicar copy en `/assets/i18n/` y conmutar por `navigator.language`.

---

## 12. Solución de problemas (FAQ técnico)

* **No se ven imágenes:** revisa rutas (`./assets/img/...`) y mayúsculas (case‑sensitive).
* **CDN bloqueado:** estás sin Internet o el cortafuegos bloquea jsDelivr; hospeda localmente.
* **Scroll no es suave:** otro script puede interceptar los enlaces; inspecciona la consola.
* **KPIs no cuentan:** verifica clase `.counter` y atributo `data-target`.
* **Formulario no envía:** el `fetch()` está comentado; configura tu endpoint y descomenta.
* **Flicker en animaciones:** evita imágenes pesadas (>300–500 KB) y usa `object-fit: cover`.

---

## 13. Anexos

### 13.1 Convenciones de código

* HTML con indentación 2 espacios, clases Bootstrap primero, clases personalizadas después.
* JS: `const`/`let`, funciones flecha para callbacks, *early returns*.
* CSS: variables en `:root`, utilidades Bootstrap preferidas a clases nuevas.

### 13.2 Licencias

* Bootstrap y Bootstrap Icons: MIT.
* Imágenes/SVG: asegúrate de poseer derechos o usar bancos libres (o propios).

---

**Fin de la documentación técnica.**
