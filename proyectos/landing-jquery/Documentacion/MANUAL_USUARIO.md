# Guía de Usuario — Landing Bootstrap + jQuery

Bienvenido/a. Esta guía explica **cómo navegar y usar** el sitio.

## ¿Qué es este sitio?

Es una página web de una sola vista (one‑page) con secciones típicas: presentación, beneficios, portafolio de trabajos, testimonios, preguntas frecuentes y contacto.

## Requisitos

* Un navegador moderno (Chrome, Edge, Firefox, Safari).
* Conexión a Internet (el sitio usa librerías desde CDN).

## Cómo navegar

* **Menú superior (navbar):** siempre visible; usa los enlaces *Inicio*, *Beneficios*, *Portafolio*, *Testimonios*, *FAQ*, *Contacto*.
* **Desplazamiento suave:** al hacer clic en un enlace del menú, la página se mueve suavemente a la sección.
* **Botón “arriba” (↥):** aparecerá en la esquina inferior derecha cuando bajes; al hacer clic te lleva al inicio.
* **Tema claro/oscuro:** botón de luna/sol en la esquina del menú. Tu elección se recuerda para la próxima visita.

## Secciones

1. **Inicio (Hero):** título del sitio y accesos rápidos para ver el portafolio o abrir contacto.
2. **Beneficios:** explica brevemente por qué este diseño es útil.
3. **Portafolio:**

   * Verás tarjetas con proyectos.
   * Usa los **filtros** (Todo/UI/Landing/Dashboard) para mostrar ciertos tipos.
   * Puedes añadir más tarjetas (el administrador del sitio lo hace editando el HTML).
4. **Testimonios:** opiniones breves.
5. **FAQ:** sección de preguntas frecuentes en formato acordeón (abre/cierra cada pregunta).
6. **Contacto:**

   * Completa **Nombre**, **Correo** y **Mensaje** (obligatorios).
   * Si los datos no son válidos, verás mensajes de error.
   * Al enviar, aparece una confirmación en pantalla (simulada). En una versión con servidor, el mensaje será enviado a un correo o almacenado en una base de datos.

## Buenas prácticas de uso

* Evita recargar la página muy rápido para que carguen los recursos desde el CDN.
* Si tu conexión es lenta, las imágenes tardarán un poco; se cargan en modo diferido (*lazy*).

## Preguntas frecuentes (resumen)

* **¿Necesito instalar algo?** No, solo abre el link del sitio.
* **¿Por qué no cambia el tema?** Puede que tu navegador bloquee `localStorage` en modo incógnito. Prueba en una pestaña normal.
* **¿No veo imágenes?** Si el administrador las reemplazó por archivos locales y estás sin internet/servidor, podrían no mostrarse.

## Soporte

Si encuentras un error visual o tienes una sugerencia:

* Toma una captura de pantalla.
* Incluye tu navegador y sistema operativo.
* Envía el reporte al correo de contacto publicado en la sección **Contacto**.

## Créditos

* Construido con **Bootstrap 5** y **jQuery 3.7**. Íconos de **Bootstrap Icons**.

---

> **Consejo:** Añade este sitio como Pestaña Favorita/Marcador en tu navegador para acceder rápidamente.
