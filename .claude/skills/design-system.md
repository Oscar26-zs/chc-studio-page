---
name: design-system
description: Design system de CHC Studio — tokens de color, tipografía, componentes y reglas que toda sección y componente debe respetar
---

# SKILL: Design System CHC Studio

Toda sección y componente del sitio debe seguir este sistema. Antes de construir cualquier elemento, leer este archivo.

---

## Paleta de Colores

Usar siempre las clases Tailwind v4 definidas en `src/styles/global.css` con `@theme`. Nunca hardcodear valores hex en componentes.

| Token | Valor | Uso |
|-------|-------|-----|
| `bg-background` | `#1a1e29` | Fondo base de toda la página |
| `bg-card` | `#132d46` | Cards, paneles, contenedores elevados |
| `text-foreground` | `#ffffff` | Texto principal |
| `text-muted-foreground` | `rgba(255,255,255,0.55)` | Subtítulos, captions, descripciones secundarias |
| `bg-accent` / `text-accent` | `#01c38e` | Color de acento: CTAs, highlights, íconos activos |
| `border-border` | `rgba(255,255,255,0.10)` | Bordes sutiles de separación |
| `bg-input` | `rgba(255,255,255,0.08)` | Inputs, áreas interactivas |
| `ring-ring` | `rgba(1,195,142,0.40)` | Focus ring del accent |

**Variables CSS adicionales disponibles:**
- `--glass-bg`: `rgba(255,255,255,0.05)` — glassmorphism base
- `--glass-border`: `rgba(255,255,255,0.12)` — borde de glass
- `--glass-blur`: `16px` — blur estándar de glass

---

## Tipografía

Fuente única: **Inter** (Google Fonts, cargada en global.css).

### Escala de texto (clases Tailwind v4)

| Clase | Tamaño | Line-height | Letter-spacing | Uso |
|-------|--------|-------------|----------------|-----|
| `text-display-xxl` | 80px | 0.95 | 1.6px | Hero principal, headline máximo |
| `text-display-xl` | 60px | 1.2 | 1.2px | Títulos de sección grandes |
| `text-display-lg` | 48px | 1.25 | 0.96px | Subtítulos de sección |
| `text-body-lg` | 16px | 1.7 | 0.32px | Párrafos con más espacio |
| `text-body-md` | 16px | 1.5 | 0.32px | Párrafos estándar |
| `text-button-cap` | 13px | 0.94 | 1.17px | Texto de botones (uppercase) |
| `text-micro-cap` | 12px | 2.0 | 0.96px | Labels, badges, eyebrows |
| `text-caption` | 13px | 1.5 | 0 | Captions, footers, metadata |

**Regla:** Los títulos de display siempre van en `uppercase`. El texto de botones siempre `uppercase`.

---

## Componentes de Estilo Globales

Definidos en `@layer components` dentro de `global.css`. Usar como clases directamente.

### Botones

```html
<!-- CTA principal -->
<a class="filled-button">Contactar ahora</a>

<!-- CTA secundario / outline -->
<a class="ghost-button">Ver portafolio</a>
```

- `filled-button`: fondo `#01c38e`, hover `#02d99e`, pill radius, 18px 24px padding
- `ghost-button`: borde blanco, hover borde+texto accent, mismas proporciones

**Nunca crear botones con estilos inline o clases ad-hoc.** Extender estas clases si se necesita variación.

### Glassmorphism

```html
<!-- Glass sutil (blur leve, para overlays y panels) -->
<div class="liquid-glass rounded-2xl p-6">...</div>

<!-- Glass fuerte (blur pesado, para modals, navbars flotantes) -->
<div class="liquid-glass-strong rounded-2xl p-6">...</div>
```

Ambas clases incluyen el pseudo-elemento `::before` que genera el borde degradado luminoso. No agregar `border` adicional a elementos con estas clases.

---

## Estructura de Secciones

Cada sección de página sigue esta estructura base:

```astro
<section class="py-24 md:py-32">
  <div class="max-w-6xl mx-auto px-6 md:px-12">

    <!-- Eyebrow opcional -->
    <p class="text-micro-cap uppercase text-accent tracking-widest mb-4">
      Etiqueta de sección
    </p>

    <!-- Título -->
    <h2 class="font-heading text-display-xl uppercase text-foreground mb-6">
      Título de sección
    </h2>

    <!-- Descripción -->
    <p class="text-body-lg text-muted-foreground max-w-2xl mb-12">
      Descripción de la sección.
    </p>

    <!-- Contenido (grid, cards, etc.) -->

  </div>
</section>
```

**Espaciado vertical entre secciones:** `py-24` en mobile, `md:py-32` en desktop. No variar sin razón justificada.

**Contenedor:** Siempre `max-w-6xl mx-auto px-6 md:px-12`. No usar anchos distintos por sección.

---

## Efectos Visuales de Fondo

El fondo `#1a1e29` es el canvas. Para agregar profundidad se usan glows difusos:

```astro
<!-- Glow de acento posicionado absolutamente dentro de la sección -->
<div class="absolute inset-0 overflow-hidden pointer-events-none" aria-hidden="true">
  <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2
              w-[600px] h-[600px] rounded-full
              bg-accent/10 blur-[120px]">
  </div>
</div>
```

- Color: siempre `bg-accent/10` (verde accent con 10% opacidad) o `bg-white/5`
- Blur: mínimo `blur-[80px]`, máximo `blur-[180px]`
- Nunca agregar más de 2 glows por sección

---

## Bordes y Radios

- Elementos card/panel: `rounded-2xl` (16px)
- Botones: `rounded-pill` / `rounded-full` (9999px) — ya incluido en `.ghost-button` y `.filled-button`
- Inputs: `rounded-xl`
- Separadores: `border-border` (`rgba(255,255,255,0.10)`)

---

## Animaciones (ver skill `gsap-animations`)

El diseño usa animaciones de entrada al scroll. Todo elemento "above the fold" anima en timeline al cargar; los demás usan ScrollTrigger. La referencia de valores (easing, duraciones) está en la skill `gsap-animations`.

CSS keyframes disponibles en `global.css`:
- `fade-up`: entrada desde abajo con fade (para uso sin GSAP en elementos simples)
- `scroll-line`: animación de línea de scroll indicator

---

## Forbidden ❌
- Colores hardcodeados que no correspondan a los tokens (ej: `text-blue-500`, `bg-gray-900`)
- Fuentes distintas a Inter
- Tamaños de texto fuera de la escala definida (ej: `text-3xl` de Tailwind crudo sin override)
- Botones que no usen `.filled-button` o `.ghost-button`
- Secciones sin el contenedor `max-w-6xl mx-auto px-6 md:px-12`
- Agregar bordes a elementos con clase `liquid-glass` o `liquid-glass-strong`
- Fondos blancos o claros en cualquier componente (el sitio es 100% dark mode)
