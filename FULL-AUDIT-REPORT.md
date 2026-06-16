# Auditoría SEO — Finanflix landing

**Fecha:** 2026-06-16
**Alcance:** 4 páginas estáticas (`index.html`, `nosotros.html`, `programas-avanzados.html`, `faq.html`)
**Método:** LLM-first sobre el HTML local + validación determinística de JSON-LD (`validate_schema.py`).

## Limitaciones de entorno

El sitio **no está desplegado todavía** (son archivos locales). No se pudieron correr chequeos que requieren URL en vivo:
Core Web Vitals (PageSpeed), `robots.txt`/headers HTTP reales, redirects, broken links externos, indexabilidad real.
Esas categorías quedan con confianza **Hipótesis** hasta el deploy.

## Score estimado

| Categoría | Score | Nota |
|---|---|---|
| Technical SEO | 70/100 | Buen markup; pendiente canonical vs estructura de URL y `robots/sitemap`. |
| On-Page SEO | 88/100 | Titles, H1 único, metas correctas. |
| Content / E-E-A-T | 82/100 | Equipo con credenciales reales, aviso de riesgo, autoría clara. Falta blog/contenido. |
| Schema | 80/100 | Válido en 3/4. FAQPage restringido. Sumado `sameAs`. |
| Performance (CWV) | 55/100 (hipótesis) | Tailwind Play CDN no apto para producción. |
| Imágenes | n/a | No hay `<img>` reales aún (placeholders). |
| AI Search (GEO) | 75/100 | Bien estructurado; sumado `llms.txt`. |
| **Global (estimado)** | **~76/100 — Bueno** | |

---

## 🔴 Críticos

### 1. Canonical no coincide con la estructura real de URLs
- **Evidencia:** los `<link rel="canonical">` apuntan a URLs sin extensión (`/nosotros`, `/programas-avanzados`, `/faq`), pero los archivos y todos los links internos usan `.html` (`nosotros.html`).
- **Impacto:** si el servidor no reescribe `.html` → URL limpia, Google ve dos URLs (la canónica declarada y la real enlazada), diluyendo señales o indexando la equivocada.
- **Fix:** decidir la estructura final y unificar las 3 capas (canonical + links internos + sitemap). Ver Action Plan #1.

### 2. Tailwind vía Play CDN (`cdn.tailwindcss.com`)
- **Evidencia:** las 4 páginas cargan `<script src="https://cdn.tailwindcss.com?plugins=forms">`.
- **Impacto:** el Play CDN compila CSS en el navegador (JS pesado, bloquea render) — penaliza LCP/CWV en producción. Está explícitamente no recomendado por Tailwind para prod.
- **Fix:** generar el CSS estático (Tailwind CLI) y servir un `.css` minificado. Ver Action Plan #2.

---

## ⚠️ Warnings

### 3. Assets sociales/favicon referenciados pero inexistentes
- **Evidencia:** las 4 páginas referencian `/og-image.jpg`, `/favicon.ico`, `/apple-touch-icon.png`; no existen en el proyecto.
- **Impacto:** previews rotos al compartir en WhatsApp/X/LinkedIn; sin favicon en pestaña. El `logo` del schema también apunta a `/og-image.jpg`.
- **Fix:** crear og-image (1200×630), favicon y apple-touch-icon, y un logo cuadrado real para el schema.

### 4. FAQPage schema restringido (faq.html)
- **Evidencia:** `validate_schema.py` → "Block 1: @type 'FAQPage' is restricted to government and healthcare sites only (Aug 2023)".
- **Impacto:** Finanflix es comercial; Google no mostrará rich results de FAQ. No penaliza, pero no aporta el snippet esperado.
- **Fix:** opcional. Mantenerlo ayuda a motores de IA (AEO) a parsear las Q&A. Si se busca prolijidad, se puede quitar. No es urgente. (Decisión del cliente.)

### 5. Falta sitemap.xml, robots.txt y llms.txt
- **Estado:** **resueltos en esta auditoría** — se crearon los tres en la raíz (ver Artefactos). Validar URLs tras decidir estructura (#1).

### 6. Meta description de programas-avanzados muy larga
- **Estado:** **corregida** — se acortó a ~190 caracteres enfocando en los tres caminos.

---

## ✅ Lo que está bien

- **Un solo `<h1>` por página**, con jerarquía de headings coherente.
- **Titles** descriptivos y < 60 caracteres en las 4 páginas.
- **`lang="es-AR"`** consistente; locale único (no requiere hreflang).
- **JSON-LD válido** en index (EducationalOrganization + Offer), nosotros (AboutPage), programas (ItemList/Course).
- **Open Graph + Twitter Card** presentes en las 4 páginas.
- **Accesibilidad:** SVGs con `aria-hidden`, botones con `aria-label`, `aria-current` en navegación.
- **Sin `<img>` sin alt** (no hay imágenes raster aún; cuando se agreguen las fotos del equipo, requieren `alt`).
- **Cross-linking** sólido vía navbar y footer entre las 4 páginas.
- **Schema enriquecido** en esta pasada: `sameAs` (4 redes), `logo`, `foundingDate`.

---

## Mejoras aplicadas en esta auditoría

1. `sameAs` (X, YouTube, Instagram, LinkedIn) + `logo` + `foundingDate` en el schema del home.
2. Meta description de programas-avanzados acortada.
3. `robots.txt`, `sitemap.xml` y `llms.txt` creados en la raíz.

## Artefactos generados

- `FULL-AUDIT-REPORT.md` (este archivo)
- `ACTION-PLAN.md`
- `robots.txt`, `sitemap.xml`, `llms.txt`
