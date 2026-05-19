# CLAUDE.md — Generador de Landing Pages para Negocios Peruanos

## Objetivo del proyecto
Generar landing pages HTML personalizadas para negocios peruanos, una por empresa,
listas para enviar como propuesta comercial por WhatsApp al dueño del negocio.
Cada archivo debe sentirse como una propuesta profesional, no una plantilla genérica.

---

## Input esperado
Archivo CSV o Excel con estos campos por empresa:

| Campo | Uso |
|---|---|
| `id` | Nombre del archivo de salida |
| `nombre_negocio` | Hero, título, SEO |
| `rubro` | Servicios, imágenes, tono del copy |
| `distrito` | Copy local, menciones geográficas |
| `direccion` | Sección contacto, iframe Maps |
| `telefono` | Sección contacto |
| `whatsapp` | Botón flotante: `https://wa.me/{whatsapp}` |
| `website_actual` | Fuente de info adicional (hacer fetch si existe) |
| `rating` | Criterio para tono de testimonios |
| `cantidad_resenas` | Mostrar badge de reseñas Google |
| `url_google_maps` | Solo para iframe embebido y botón "Ver en Maps" |
| `oportunidad_textual` | Tagline secundario en hero o sección "¿Por qué elegirnos?" |
| `Estilo de pagina web` | Paleta, tipografía, layout completo |

---

## Flujo de trabajo por empresa

```
1. Leer fila de la tabla
2. Buscar info adicional en la web:
   - "{nombre_negocio} {distrito} Lima" → horario, datos extra
   - Si tiene website_actual → hacer web_fetch para extraer info real
   - "{nombre_negocio} {distrito} fotos" → encontrar imágenes públicas
3. Generar HTML completo
4. Guardar como {id}-{slug}.html en /mnt/user-data/outputs/
5. Continuar con la siguiente empresa
```

---

## Estructura de cada landing page

### Secciones obligatorias (en orden):

1. **`<head>`** — Meta tags SEO: title, description, og:title, viewport
2. **Hero** — Nombre del negocio + tagline basada en `oportunidad_textual` + CTA WhatsApp
3. **Sobre nosotros** — Copy persuasivo basado en rubro + distrito (NO genérico)
4. **Servicios** — 4 a 6 servicios típicos del rubro con íconos SVG inline
5. **Estadísticas/Confianza** — Badge: ⭐ {rating} · {cantidad_resenas} reseñas en Google
6. **Testimonios** — 3 testimonios con nombres peruanos reales (ver criterio abajo)
7. **Galería** — 3 imágenes Unsplash fijas por rubro (ver criterio abajo)
8. **Ubicación** — Iframe Google Maps embebido + botón "Ver en Google Maps"
9. **Contacto** — Teléfono, WhatsApp, dirección, horario estimado por rubro
10. **Footer** — Nombre del negocio, rubro, distrito

### Elemento flotante obligatorio:
- Botón WhatsApp fijo en esquina inferior derecha
- Color: #25D366
- Link: `https://wa.me/{whatsapp}` (limpiar el número: solo dígitos, agregar 51 si es peruano)

---

## Criterios de diseño por `Estilo de pagina web`

| Estilo | Paleta sugerida | Fuente Google | Mood |
|---|---|---|---|
| moderno | #1a1a2e + #e94560 | Inter | Tech, limpio |
| premium | #0f0f0f + #c9a84c | Playfair Display + Lato | Lujo, exclusivo |
| minimalista | #ffffff + #333 + #f5f5f5 | DM Sans | Blanco, espacio |
| profesional | #003366 + #0066cc | Roboto | Corporativo, serio |
| elegante | #2c2c2c + #b8860b | Cormorant Garamond | Sofisticado |
| juvenil | #ff6b6b + #4ecdc4 | Nunito | Colorido, energía |
| corporativo | #1b2a4a + #3a86ff | Source Sans Pro | Empresarial |
| visual | #ff4757 + #2f3542 | Montserrat | Bold, impactante |
| médico | #ffffff + #0077b6 | Poppins | Confianza, salud |
| creativo | #6c5ce7 + #fd79a8 | Raleway | Artístico, único |

Si el estilo no está en la tabla, inferir paleta apropiada para el rubro.

---

## Criterio para testimonios

Generar 3 testimonios con nombres peruanos (ej: Carlos Quispe, María Flores, José Huamán):

- **Rating ≥ 4.5** → Muy positivos, énfasis en excelencia, recomiendan ampliamente
- **Rating 4.0–4.4** → Positivos y confiables, destacan profesionalismo y trato
- **Rating < 4.0** → Positivos pero moderados, énfasis en mejora y servicio

Tono por rubro:
- Restaurante/cevichería → informal, hablan de sabor, ambiente, precio
- Clínica/médico → formal, hablan de atención, diagnóstico, confianza
- Taller/ferretería → directo, hablan de solución rápida, precio justo
- Belleza/spa → emotivo, hablan de cómo se sintieron, resultado visual
- Legal/contable → serio, hablan de seguridad, resultado, ahorro

---

## Imágenes

Usar URLs fijas de Unsplash por rubro. Formato:
```
https://images.unsplash.com/photo-[ID]?w=1200&q=80&fit=crop
```

Ejemplos por rubro:
- Restaurante: photo-1555396273-367ea4eb4db5 (comida), photo-1414235077428-338989a2e8c0
- Clínica/Salud: photo-1519494026892-80bbd2d6fd0d, photo-1576091160399-112ba8d25d1d
- Belleza/Spa: photo-1560066984-138dadb4c035, photo-1522337360788-8b13dee7a37e
- Legal/Contable: photo-1450101499163-c8848c66ca85, photo-1507679799987-c73779587ccf
- Taller/Auto: photo-1487754180451-c456f719a1fc, photo-1558618666-fcd25c85cd64
- Educación: photo-1503676260728-1c00da094a0b, photo-1523050854058-8df90110c9f1
- Tecnología: photo-1518770660439-4636190af475, photo-1461749280684-dccba630e2f6

Si el rubro no está listado, buscar foto apropiada o usar gradiente CSS elegante.

---

## Reglas técnicas del HTML

- Todo en **un solo archivo .html** (CSS y JS embebidos, sin archivos externos)
- Solo dependencias permitidas: Google Fonts (link CDN)
- **100% responsive**, mobile-first
- Animaciones CSS sutiles: fade-in al scroll, hover en cards y botones
- Iframe de Maps: `https://maps.google.com/maps?q={direccion_encoded}&output=embed`
- Botón WhatsApp: limpiar número → solo dígitos → si empieza por 9, agregar prefijo 51
- Código organizado con comentarios de sección: `<!-- HERO -->`, `<!-- SERVICIOS -->`, etc.
- NO usar frameworks JS (React, Vue, etc.)
- NO usar Bootstrap ni Tailwind
- SÍ puede usar CSS custom properties (variables) para la paleta

---

## Nomenclatura de archivos de salida

```
{id_con_ceros}-{nombre_slug}.html
```

Ejemplos:
- `001-cevicheria-el-mar.html`
- `015-clinica-salud-total.html`
- `032-taller-mecanico-jesus.html`

Slug: nombre en minúsculas, sin tildes, espacios reemplazados por guiones, sin caracteres especiales.

---

## Qué NO hacer

- ❌ No inventar datos que no estén en la tabla ni se puedan verificar
- ❌ No usar `source.unsplash.com` (está deprecado)
- ❌ No intentar navegar `url_google_maps` directamente (usar solo para iframe)
- ❌ No copiar reseñas literalmente de ninguna fuente
- ❌ No usar frases genéricas como "somos líderes en el sector" o "calidad garantizada"
- ❌ No poner datos de contacto ficticios si el campo está vacío (omitir la sección o poner placeholder claro)

---

## Qué hacer si faltan datos

| Campo vacío | Acción |
|---|---|
| `telefono` | Omitir en sección contacto |
| `whatsapp` | Omitir botón flotante (o usar telefono si existe) |
| `website_actual` | Omitir enlace a web |
| `direccion` | Omitir iframe Maps, poner solo distrito |
| `rating` / `cantidad_resenas` | Omitir badge de reseñas |
| `oportunidad_textual` | Generar tagline basada en rubro + distrito |
| `Estilo de pagina web` | Usar estilo "profesional" por defecto |

---

## Entregables finales

1. Archivos `.html` en `/mnt/user-data/outputs/`
2. Resumen en consola al terminar:

```
✅ Empresas procesadas: X
📁 Archivos generados:
   - 001-nombre-empresa.html → Estilo: moderno | Info extra: website fetch ✓ | Imágenes: Unsplash
   - 002-otra-empresa.html → Estilo: premium | Info extra: búsqueda web ✓ | Imágenes: Unsplash
⚠️  Empresas con datos faltantes: Y (ver detalle arriba)
```

---

## Copy — Idioma y tono

- **Idioma**: Español peruano
- **Tono**: Cercano, profesional, confiable
- **Evitar**: jerga muy técnica, anglicismos innecesarios, frases corporativas vacías
- **Incluir**: Referencias locales al distrito cuando sea natural (ej: "el mejor servicio en Miraflores")
