# Documentación Técnica — Landing Bootstrap + jQuery

> **Stack:** HTML5, Bootstrap 5.3.3, jQuery 3.7.x, Bootstrap Icons.
> **Arquitectura:** sitio estático *one‑page* (single HTML), sin backend.
> **Objetivo:** plantilla limpia, accesible y mantenible.

---

## 1. Estructura del proyecto

```
/ (raíz)
├─ index.html          # Sitio completo (HTML + JS inline, mínimo CSS)
├─ /assets/            # (opcional) imágenes/CSS propios si los agregas
└─ README_USUARIO.md   # Guía de uso para personas no técnicas
```

> **Nota:** por simplicidad todo vive en `index.html`. Si deseas separar, crea `assets/css/custom.css` y `assets/js/app.js` y mueve los bloques correspondientes.

---

## 2. Dependencias

* **Bootstrap 5.3.3 (CSS/JS)**: componentes, grid y utilidades responsivas.
* **Bootstrap Icons**: set de íconos basado en fuentes.
* **jQuery 3.7**: interacciones (scroll suave, filtro, validación).

Se consumen desde **CDN** (ver `<head>` y `<script>` al final). Requieren conexión a Internet.

---

## 3. Secciones principales (index.html)

1. **Navbar (fixed‑top)**: navegación interna, botón de tema (claro/oscuro).
2. **Hero**: encabezado con CTA, tarjeta de *stack*.
3. **Beneficios**: tres tarjetas con puntos clave.
4. **Portafolio**: grid de proyectos + filtro por categoría (jQuery).
5. **Testimonios**: tarjetas simples con avatar.
6. **FAQ**: acordeón Bootstrap.
7. **Contacto**: formulario validado en cliente + datos.
8. **Footer**: enlaces y año dinámico.
9. **Utilidades**: botón *scroll‑top*, scroll suave, tema persistente.

---

## 4. JavaScript y jQuery

### 4.1 Inicialización

* Todo el JS jQuery vive dentro de `$(function(){ ... });` para asegurar DOM listo.

### 4.2 Scroll suave

* Selecciona `a[href^="#"]` y anima `$('html, body').animate({scrollTop: ...}, 400)` restando la altura de la navbar fija.

### 4.3 Filtro de portafolio

* Botones con `data-filter` controlan visibilidad de columnas cuya categoría vive en `data-category` (`*`, `ui`, `landing`, `dashboard`).

### 4.4 Scroll‑top

* Botón `#btnTop` aparece con `toggle(scrollTop > 300)` y vuelve al inicio con animación.

### 4.5 Tema claro/oscuro

* Manipula `data-bs-theme` en `<html>` y persiste en `localStorage`. Cambia ícono (sol/luna) vía jQuery.

### 4.6 Formulario de contacto

* Valida campos requeridos y formato de email (`/^[^\s@]+@[^\s@]+\.[^\s@]+$/`).
* Marca inputs con `.is-invalid`/`.is-valid`.
* **En producción:** reemplazar `alert()` por `$.ajax({ url: "https://api.tu-dominio.com/contacto", method: "POST", data: JSON.stringify(payload), contentType: "application/json" })` y manejar la respuesta.

---

## 5. Accesibilidad (A11y)

* Semántica HTML5 (header, section, nav, footer).
* `aria-label` en navbar; contraste suficiente; foco visible por defecto.
* Imágenes con `alt`.
* Botones con `aria-pressed` cuando aplica (tema).

---

## 6. SEO básico

* `<title>` + `<meta name="description">` descriptivos.
* URLs limpias (one‑page).
* Contenido legible y jerarquía H1→H2→H3.

---

## 7. Rendimiento

* CDNs minificados.
* Imágenes `loading="lazy"`.
* JS al final del body.
* CSS propio mínimo para evitar *reflows*.

---

## 8. Estilos

* Se prioriza **utilidades Bootstrap** (márgenes, padding, sombras, tipografía).
* CSS propio reducido (variables de marca, fondos decorativos, hover cards).

---

## 9. Personalización

* Cambia los colores en `:root { --brand: … }`.
* Edita textos/íconos en cada tarjeta.
* Para más proyectos, duplica una columna del portafolio y ajusta `data-category`.

---

## 10. Despliegue

### 10.1 GitHub Pages

1. Crea repo y sube `index.html` (y `/assets` si usas recursos locales).
2. Settings → **Pages** → Branch `main` / folder `/root`.
3. Publica: `https://TU_USUARIO.github.io/TU_REPO/`.

### 10.2 Servir localmente

* Extensión **Live Server** en VS Code → `Open with Live Server`.

---

## 11. Compatibilidad

* Navegadores modernos (Chromium/Firefox/Safari/Edge) actualizados.
* No IE11.
* Móvil y desktop (responsive) gracias a grid/containers.

---

## 12. Seguridad (front)

* Enlaces externos con `rel="noopener"`.
* Si agregas AJAX, valida/sanea en servidor.
* No se incluyen dependencias de terceros no confiables.

---

## 13. Limitaciones/Próximos pasos

* Sin backend: el formulario no envía a un correo real.
* Agregar *analytics*, i18n, unit tests de utilidades, y compresión de imágenes.
