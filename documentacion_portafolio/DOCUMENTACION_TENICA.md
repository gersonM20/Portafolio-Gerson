# Documentación Técnica — Portafolio **Gerson Montenegro De Leon**

> **Stack:** HTML5 + CSS mínimo (inline) + Bootstrap 5.3.3 + Bootstrap Icons 1.11.3 + JavaScript (vanilla).
> **Arquitectura:** sitio estático *one‑page* (`index.html`).
> **Objetivo:** presentar habilidades/proyectos con buenas prácticas de accesibilidad, SEO y rendimiento.

---

## 1. Estructura del proyecto

```
/ (raíz)
├─ index.html                 # Único archivo de la landing (hiper-comentado)
└─ /assets/
   └─ /img/
      └─ gerson.png           # Foto para navbar y héroe
```

> Puedes añadir `/proyectos/<nombre>/index.html` para las demos de cada tarjeta.

---

## 2. Dependencias

* **Bootstrap 5.3.3 (CSS + Bundle JS)** desde CDN jsDelivr.

  * *Por qué bundle*: incluye Popper (requerido por algunos componentes) y reduce requests.
* **Bootstrap Icons 1.11.3**: íconos vectoriales como fuente.
* **Sin jQuery**: la interacción es ligera; se usa JavaScript nativo.

> *Modo offline*: hospeda los archivos de Bootstrap e Icons localmente y cambia los `<link>/<script>`.

---

## 3. Estilos y branding

### 3.1 Variables CSS

```css
:root { --accent: #6f42c1; --nav-h: 72px; }
```

* `--accent` define el color de marca (morado). Cambiarlo re‑tematiza el sitio.
* `--nav-h` expresa la altura aproximada de la navbar y se usa en dos **FIX**:

  * `body { padding-top: var(--nav-h); }` → evita que la barra fija tape el comienzo.
  * `section { scroll-margin-top: calc(var(--nav-h) + 12px); }` → asegura que los títulos anclados queden visibles.

### 3.2 Utilidades propias

* `.brand-gradient` → relleno degradado en el título principal.
* `.hero` → fondo con **radial-gradients** (ligeros para GPU y sin bloqueo).
* `.shadow-soft` y `.card:hover` → micro‑interacciones y profundidad.
* `.tag` → chip con borde punteado para el estado "Estudiante de Ingeniería en Sistemas".
* `.avatar-img` y `.profile-pic-lg` → tamaños/recorte de las fotos.

---

## 4. HTML semántico y accesibilidad

* Secciones estructurales: `nav`, `header#inicio`, `section#sobre-mi`, `section#habilidades`, `section#proyectos`, `section#educacion`, `section#contacto`, `footer`.
* Navegación interna por anclas con `href="#…"` + scroll suave nativo (CSS `scroll-behavior`).
* Texto alternativo en imágenes: describe la foto de la persona.
* Controles accesibles: botón de cambio de tema con `aria-pressed`.
* Foco visible forzado con `:focus-visible` y contraste suficiente.

---

## 5. JavaScript (comportamiento)

### 5.1 Año dinámico

```js
document.getElementById('year').textContent = new Date().getFullYear();
```

### 5.2 Tema claro/oscuro

* Estado guardado en `localStorage('theme')` con valores `light|dark`.
* Cambia el atributo `data-bs-theme` en `<html>` (Bootstrap aplica tokens de color).
* `setIcon()` sincroniza el ícono e `aria-pressed`.

### 5.3 Validación de formulario

* Usa **validación nativa** (`checkValidity`) + clases Bootstrap (`was-validated`).
* `novalidate` evita mensajes nativos del navegador y permite estilos consistentes.
* `action="mailto:"` es demostrativo. En producción, usar `fetch()` a un endpoint o servicios como **EmailJS**/**Brevo**.

---

## 6. Portafolio y rutas

* Cada tarjeta apunta a `./proyectos/<carpeta>/index.html`.
* Para GitHub Pages (rama `main`, carpeta `/root`): esas rutas relativas funcionan siempre que subas el árbol tal cual.

---

## 7. SEO

* `<title>` y `<meta name="description">` descriptivos.
* **JSON‑LD (Schema.org/Person)**: mejora *rich snippets* en buscadores.
* Jerarquía correcta de encabezados (H1 → H2 → H3/H5).

---

## 8. Rendimiento

* CDN minificados, pocas dependencias, JS al final del body.
* Imágenes con `object-fit: cover` (mejor framing); optimízalas a 100–200 KB.
* Efectos visuales con `transform/opacity` (evitan *reflows* costosos).

---

## 9. Despliegue (GitHub Pages)

1. Sube `index.html` y `/assets/img/gerson.png` (y `/proyectos/...` si usas demos).
2. Repo → **Settings → Pages** → Branch `main` y Folder `/ (root)`.
3. Accede a `https://TU_USUARIO.github.io/TU_REPO/`.

---

## 10. QA / Lista de verificación

* [ ] La parte superior (chip de estudiante) **no** queda oculta por la navbar.
* [ ] Enlaces de menú desplazan a cada sección con el título visible.
* [ ] Tarjetas hacen hover y no saltan el layout.
* [ ] Formulario: campos requeridos muestran feedback, envío bloquea inválidos.
* [ ] En móvil: navbar colapsa correctamente; tipografías legibles.

---

## 11. Extensiones sugeridas

* **Descarga de CV**: enlaza un PDF real (GitHub Releases/Drive) y mide clics.
* **Analytics**: Plausible/GA4 con script `defer` en `<head>`.
* **Tema automático**: leer `prefers-color-scheme` al primer render.
* **Filtros de proyectos**: agregar `data-category` y jQuery/vanilla para filtrar.
* **Accesibilidad**: probar con lectores de pantalla y mejorar `aria-label`s.

---

## 12. Troubleshooting

* **La parte superior no se ve** → confirma `--nav-h` y que `body{padding-top}` esté activo.
* **Anclas cortadas** → revisa `section{scroll-margin-top}`.
* **Iconos no cargan** → verifica CDN de Bootstrap Icons (red/privacidad).
* **Imagen rota** → ruta y mayúsculas en `assets/img/gerson.png`.
* **Formulario no envía** → es normal con `mailto:`. Conecta un servicio/endpoint.

---

**Fin de la documentación técnica.**
