# Action Plan SEO — Finanflix

Priorizado por impacto / esfuerzo. ✅ = ya aplicado en esta pasada.

## Antes de publicar (bloqueantes)

1. ✅ **Estructura de URL unificada en URLs limpias.**
   Canonical + links internos + sitemap usan ahora `/nosotros`, `/programas-avanzados`, `/faq`, `/`.
   **Pendiente de deploy:** el hosting debe servir `nosotros.html` en `/nosotros` (rewrite). En Vercel/Netlify es automático; en hosting tradicional (Apache/Nginx) hay que configurar la regla. Nota: abrir los archivos por `file://` ya no navega entre páginas — requiere el server.

2. **Reemplazar Tailwind Play CDN por CSS compilado.** 🔴
   Generar con Tailwind CLI un `styles.css` minificado y reemplazar el `<script src="cdn.tailwindcss.com">`. Mejora directa de LCP/CWV.

3. **Crear assets de imagen.** ⚠️
   `og-image.jpg` (1200×630), `favicon.ico`, `apple-touch-icon.png` (180×180) y un logo cuadrado real para `logo` del schema (hoy apunta a og-image como placeholder).

## Primera semana post-deploy

4. **Verificar en vivo:** correr PageSpeed (CWV), validar el sitemap en Search Console, confirmar indexabilidad y que no haya redirect/404.
5. **Subir las fotos reales del equipo** en nosotros.html con `alt` descriptivo ("Facundo Zamora, CEO de Finanflix").
6. **Decisión FAQPage schema:** mantener (ayuda a AEO/IA) o quitar (no da rich results en sitio comercial). Sin urgencia.
7. **Reemplazar el lorem ipsum** de las descripciones de programas avanzados por copy real (afecta contenido indexable de esa página).

## Mejora continua

8. **Contenido / E-E-A-T:** un blog o sección de notas (análisis de mercado, glosario crypto) daría superficie de keywords y reforzaría autoridad. Hoy son 4 páginas sin contenido editorial.
9. **BreadcrumbList schema** en las páginas hijas (nosotros, programas, faq) para breadcrumbs en resultados.
10. **Confirmar checkout del plan anual** (hoy usa el link `type=basic`; el anual probablemente necesita su propia URL).

## Ya aplicado ✅

- `sameAs` (X, YouTube, Instagram, LinkedIn) + `logo` + `foundingDate` en schema del home.
- Meta description de programas-avanzados acortada.
- `robots.txt`, `sitemap.xml`, `llms.txt` creados.
