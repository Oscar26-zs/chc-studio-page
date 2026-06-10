---
name: gsap-animations
description: GSAP + ScrollTrigger animations in Astro for CHC Studio — patterns, setup, and component integration
---

# SKILL: GSAP Animations (CHC Studio)

## Setup

GSAP se carga como módulo ES desde CDN o npm. En este proyecto se usa vía npm:

```bash
pnpm add gsap
```

En cada componente Astro que necesite animación, importar en un `<script>`:

```astro
<script>
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);
</script>
```

> Nunca importar GSAP en el frontmatter (`---`). GSAP es código de browser — va exclusivamente en `<script>`.

---

## Patrones Base

### Fade-up al entrar al viewport (patrón estándar de secciones)
```astro
<section class="section-reveal">
  <h2>Título</h2>
  <p>Contenido</p>
</section>

<script>
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);

  gsap.from('.section-reveal', {
    scrollTrigger: {
      trigger: '.section-reveal',
      start: 'top 85%',
      toggleActions: 'play none none none',
    },
    opacity: 0,
    y: 40,
    duration: 0.8,
    ease: 'power2.out',
  });
</script>
```

### Stagger de elementos hijo (cards, listas, íconos)
```astro
<div class="cards-grid">
  <div class="card">...</div>
  <div class="card">...</div>
  <div class="card">...</div>
</div>

<script>
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);

  gsap.from('.card', {
    scrollTrigger: {
      trigger: '.cards-grid',
      start: 'top 80%',
    },
    opacity: 0,
    y: 30,
    duration: 0.6,
    stagger: 0.12,
    ease: 'power2.out',
  });
</script>
```

### Hero de entrada (sin ScrollTrigger, se ejecuta al cargar)
```astro
<script>
  import { gsap } from 'gsap';

  const tl = gsap.timeline({ defaults: { ease: 'power3.out' } });

  tl.from('.hero-eyebrow', { opacity: 0, y: 20, duration: 0.6 })
    .from('.hero-title',   { opacity: 0, y: 30, duration: 0.7 }, '-=0.3')
    .from('.hero-body',    { opacity: 0, y: 20, duration: 0.6 }, '-=0.3')
    .from('.hero-ctas',    { opacity: 0, y: 16, duration: 0.5 }, '-=0.3');
</script>
```

### Parallax sutil en elementos de fondo
```astro
<script>
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);

  gsap.to('.bg-glow', {
    scrollTrigger: {
      trigger: 'body',
      start: 'top top',
      end: 'bottom bottom',
      scrub: 1.5,
    },
    y: -120,
    ease: 'none',
  });
</script>
```

---

## Valores de Easing del Design System CHC

| Situación | Ease GSAP |
|-----------|-----------|
| Entradas de texto / elementos | `power2.out` |
| Hero title | `power3.out` |
| Elementos interactivos (hover) | `power1.inOut` |
| Scrub / parallax | `none` |
| Modals / overlays | `expo.out` |

---

## Duraciones Recomendadas

| Tipo | Duración |
|------|----------|
| Micro (hover, tap) | 0.2 – 0.3s |
| Entrada estándar | 0.6 – 0.8s |
| Hero principal | 0.8 – 1.0s |
| Stagger delay entre items | 0.10 – 0.14s |

---

## Reglas de Accesibilidad (obligatorias)

Siempre envolver animaciones con `prefers-reduced-motion`:

```astro
<script>
  import { gsap } from 'gsap';

  const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  if (!prefersReduced) {
    gsap.from('.section-reveal', { opacity: 0, y: 40, duration: 0.8 });
  }
</script>
```

---

## Registrar plugins una sola vez por página

Si varios componentes en la misma página usan ScrollTrigger, el `registerPlugin` extra no daña pero tampoco es necesario. La convención en este proyecto es registrar en cada componente que lo use — Astro bundlea los scripts y GSAP deduplica el registro automáticamente.

---

## Forbidden ❌
- Importar GSAP en el frontmatter Astro (`---`)
- Usar `gsap.set` visible en pantalla antes de que el usuario lo vea (provoca flash)
- ScrollTrigger con `start: 'top top'` en elementos que ya están en viewport al cargar (usar timeline de entrada sin trigger)
- Animar `width` o `height` directamente (preferir `scaleX/scaleY` para performance)
- Olvidar `prefers-reduced-motion`
