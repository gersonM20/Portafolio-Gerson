# Documentación Técnica — Sitio Único “Iglesia de Cristo Ebenezer”

## 1. Resumen de Arquitectura
- **Tipo de aplicación:** Sitio estático de una sola página (one‑page).
- **Tecnologías:** HTML5, CSS3, JavaScript (vanilla) + Bootstrap 5 (CDN) + Bootstrap Icons (CDN).
- **Entrega/hosting:** Cualquier servidor estático (GitHub Pages, Netlify, Vercel, Nginx/Apache). En desarrollo se recomienda **Live Server** en VS Code.
- **Estructura:** Un único `index_comentado.html` que incluye CSS y JS integrados.
- **Extensibilidad:** Hook para backend en `fetch("/api/contacto")` (sección Contacto).

## 2. Dependencias
- **CDN Bootstrap 5**: CSS + bundle JS para navbar colapsable y utilidades.
- **CDN Bootstrap Icons**: íconos vectoriales.
- **Sin libraries adicionales**: el código JS es nativo (no jQuery ni frameworks).

## 3. Estructura Lógica de Secciones
1. **Navbar (fija)** — navegación entre secciones con anchors internos.
2. **Hero (Inicio)** — presentación, botones de acción (Servicios / Contacto).
3. **Nosotros** — texto + imagen responsiva (ratio 16:9).
4. **Stats** — cuatro KPIs con **contadores animados**.
5. **Servicios** — tarjetas con horarios y bloque de redes/eventos.
6. **Ministerios** — rejilla de 8 áreas (ícono + título + descripción).
7. **Contacto** — tarjeta de info + **formulario validado**; hook a backend.
8. **Footer** — enlaces rápidos, horarios, datos de contacto.
9. **Botón scroll-top** — aparece al bajar, vuelve arriba con suavizado.

## 4. Hoja de Estilos (CSS)
- **Variables CSS (`:root`)** para colores: `--color-primario`, `--color-secundario`, `--color-acento`, `--color-texto`.
- **Clases clave:** `.navbar-custom`, `.hero-section`, `.service-card`, `.stats-section`, `.footer-custom`, `.scroll-top`, `.fade-in`.
- **Animaciones:** `.fade-in` + `.fade-in.visible` (transición de opacidad y translateY).

## 5. Lógica JavaScript
Todo el JS está al final del HTML (mejora el performance al cargar contenido primero).

### 5.1 Scroll suave
- Selecciona `a[href^="#"]`, evita el comportamiento por defecto y hace `window.scrollTo({ behavior: "smooth" })`.
- Compensa la altura del navbar fijo para que la sección no quede oculta.

### 5.2 Animaciones on‑scroll
- `reveal()` agrega `.visible` cuando el elemento `.fade-in` entra al viewport.
- Se ejecuta en `DOMContentLoaded` y en el evento `scroll`.

### 5.3 Contadores (Stats)
- `animateCounters()` aumenta cada `.counter` desde 0 hasta `data-target`.
- Se dispara **una sola vez** al detectar que `.stats-section` está en vista.

### 5.4 Botón scroll‑top
- Se muestra si `window.pageYOffset > 300`.
- Al hacer clic, hace scroll suave a `top: 0`.

### 5.5 Formulario de contacto
- Valida: nombre, email (regex), mensaje (requeridos).
- **Hook a backend:** comentar/activar `fetch("/api/contacto")` con `POST` JSON.
- Manejo de errores con `try/catch` y alertas.

### 5.6 Navbar activo dinámico
- Detecta la sección visible y alterna `.active` en el link correspondiente.

### 5.7 Mensajes de bienvenida / domingo
- Escribe en consola saludo según hora del día y recordatorio especial si es domingo.

## 6. Conexión a Backend (Opcional)
**Ejemplo de contrato sugerido (Node/Express o .NET):**
- `POST /api/contacto`
  - **Body (JSON):** `{ nombre: string, email: string, telefono?: string, mensaje: string }`
  - **Validaciones:**
    - `email` válido (regex).
    - Longitud mínima de `mensaje` (p.ej. 10 caracteres).
  - **Respuesta 200:** `{ ok: true, message: "Recibido" }`
  - **Respuesta 400/422:** `{ ok: false, error: "Campo X inválido" }`
  - **Respuesta 500:** `{ ok: false, error: "Error interno" }`

**Seguridad recomendada:**
- **Rate‑limit** del endpoint (evitar spam).
- **reCAPTCHA** v3 o Turnstile.
- **Sanear/escapar** entradas para evitar inyección de código en emails/almacenamiento.

## 7. Accesibilidad y SEO
- **Accesibilidad:**
  - Uso de etiquetas semánticas (`nav`, `section`, `footer`).
  - `alt` descriptivo en imágenes.
  - Contrastes suficientes (texto sobre fondos).
  - Enlaces y botones con foco visible.
- **SEO on‑page:**
  - `<title>` claro.
  - Copys con palabras clave (iglesia, servicios, horarios, Escuintla).
  - Etiquetas `meta` adicionales (description, og:*, etc.) — opcional.

## 8. Rendimiento
- Carga deferida natural al ubicar JS al final.
- Imágenes con `loading="lazy"` donde aplica.
- Posibles mejoras:
  - Minificar HTML/CSS/JS.
  - Reemplazar placeholder de hero por imagen optimizada (webp).
  - Preload de fuentes si usas tipografías externas.

## 9. Mantenimiento y Versionado
- Mantener cambios en bloques comentados para trazabilidad.
- Versionar con Git: tags por entregas (v1.0, v1.1, etc.).
- Checklist de PR: probar scroll, responsividad, formulario, y navbar colapsable.

## 10. Pruebas recomendadas
- **Funcionales:** navegación entre secciones, animaciones, contadores, envío del formulario (simulado y real con backend).
- **Responsive:** móvil, tablet, desktop (breakpoints Bootstrap).
- **Accesibilidad:** teclado (TAB), lectores de pantalla, contraste.
- **Cross‑browser:** Chrome, Edge, Firefox, Safari.

## 11. Despliegue
- **Local:** Live Server (VS Code).
- **Prod:** subir `index_comentado.html` y carpeta `MaterialDesign/` con imágenes al host.
- **Dominios/SSL:** configurar HTTPS (Let’s Encrypt / plataforma de hosting).

## 12. Roadmap (ideas futuras)
- PWA (instalable): manifest + service worker (cache estático).
- Modo oscuro (toggle).
- Panel administrador (si hay backend): CRUD de eventos/ministerios/horarios.