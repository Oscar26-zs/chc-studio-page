# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm dev       # Start dev server at localhost:4321
pnpm build     # Build production site to ./dist/
pnpm preview   # Preview the built site locally
```

Node >=22.12.0 and pnpm are required.

## What This Project Is

**CHC Studio** — landing page para una agencia de tecnología en Costa Rica. Sitio dark-mode, en español, orientado a empresas y negocios. Stack: Astro 6 (SSG) + Tailwind CSS v4 + GSAP para animaciones.

## Architecture

**Routing:** File-based via `src/pages/`. Cada `.astro` en ese directorio es una ruta. Actualmente solo existe `index.astro` (`/`).

**Layout:** `src/layouts/Layout.astro` es el wrapper de todas las páginas. Importa `src/styles/global.css` (Tailwind v4 + design tokens) y define `<head>` con el SEO base del sitio.

**Componentes:** `src/components/` contiene componentes `.astro` reutilizables. El archivo de frontmatter (`---`) corre en servidor/build; el `<script>` corre en browser (ahí va GSAP).

**Estilos:** Todo el design system está centralizado en `src/styles/global.css` usando `@theme` de Tailwind v4. No modificar valores de color, tipografía o componentes directamente en los archivos de componente — extender desde `global.css`.

**Assets:** Imágenes bajo `src/assets/` pasan por el pipeline de optimización de Astro al importarlas. Archivos estáticos sin procesar van en `public/`.

## Design System (leer antes de construir cualquier componente)

El proyecto tiene un design system definido. Antes de crear o modificar cualquier sección o componente, consultar la skill en `.claude/skills/design-system.md`. Resumen:

- **Paleta:** Dark mode único. Fondo `#1a1e29`, accent `#01c38e`, cards `#132d46`.
- **Tipografía:** Inter exclusivamente. Escala custom: `text-display-xxl` (80px) → `text-micro-cap` (12px), definida en `@theme`.
- **Botones globales:** `.filled-button` (accent sólido) y `.ghost-button` (outline blanco). Nunca crear botones ad-hoc.
- **Glass:** `.liquid-glass` y `.liquid-glass-strong` — ya incluyen pseudo-borde luminoso, no agregar `border` extra.
- **Sección estándar:** `py-24 md:py-32` vertical, contenedor `max-w-6xl mx-auto px-6 md:px-12`.

## Animaciones GSAP

El proyecto usa GSAP + ScrollTrigger para animaciones de entrada al scroll. Ver `.claude/skills/gsap-animations.md` para patrones, valores de easing y duración del design system.

Regla crítica: GSAP **solo** va en `<script>` de componentes Astro, nunca en el frontmatter `---`.

## Skills Disponibles

- `.claude/skills/design-system.md` — tokens, componentes globales, estructura de secciones
- `.claude/skills/gsap-animations.md` — patrones GSAP + ScrollTrigger para este proyecto
