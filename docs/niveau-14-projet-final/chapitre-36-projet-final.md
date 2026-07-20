# Chapitre 36 — Projet Final : Mini Design System

## Introduction

Ce projet final synthétise **toutes les notions** apprises au cours des 35 chapitres précédents. Nous allons construire un **Mini Design System complet** en SCSS, en utilisant l'architecture 7-1, les `@use`/`@forward`, un système de tokens, des composants, des utilities, et des thèmes light/dark.

---

## Objectifs du projet

- Architecture 7-1 complète avec `@use` et `@forward`
- Palette de couleurs avec variables et maps
- Échelle d'espacement
- Échelle typographique
- Système de boutons
- Composant Cartes
- Système de formulaires
- Grille responsive
- Classes utilitaires auto-générées
- Thèmes Light/Dark

---

## Arbre du projet

```
mini-design-system/
├── src/
│   ├── abstracts/
│   │   ├── _index.scss
│   │   ├── _variables.scss
│   │   ├── _colors.scss
│   │   ├── _typography.scss
│   │   ├── _spacing.scss
│   │   ├── _breakpoints.scss
│   │   ├── _shadows.scss
│   │   ├── _borders.scss
│   │   ├── _transitions.scss
│   │   ├── _z-index.scss
│   │   ├── _functions.scss
│   │   └── _mixins.scss
│   │
│   ├── base/
│   │   ├── _index.scss
│   │   ├── _reset.scss
│   │   ├── _typography.scss
│   │   └── _global.scss
│   │
│   ├── components/
│   │   ├── _index.scss
│   │   ├── _buttons.scss
│   │   ├── _cards.scss
│   │   ├── _forms.scss
│   │   ├── _badges.scss
│   │   └── _alerts.scss
│   │
│   ├── layout/
│   │   ├── _index.scss
│   │   ├── _grid.scss
│   │   ├── _container.scss
│   │   ├── _header.scss
│   │   └── _footer.scss
│   │
│   ├── themes/
│   │   ├── _index.scss
│   │   ├── _light.scss
│   │   └── _dark.scss
│   │
│   ├── utilities/
│   │   ├── _index.scss
│   │   ├── _display.scss
│   │   ├── _text.scss
│   │   ├── _spacing.scss
│   │   └── _generated.scss
│   │
│   └── main.scss
│
├── dist/
│   └── css/
│       └── main.css
│
├── index.html
├── package.json
└── README.md
```

---

## Fichier d'entrée principal

```scss
// src/main.scss

// ============================================================
// MINI DESIGN SYSTEM
// Architecture 7-1 avec @use et @forward
// ============================================================

// 1. ABSTRACTS (aucun CSS généré)
@use 'abstracts';

// 2. BASE (styles globaux)
@use 'base';

// 3. LAYOUT (structure)
@use 'layout';

// 4. COMPONENTS (composants UI)
@use 'components';

// 5. THEMES (thèmes visuels)
@use 'themes';

// 6. UTILITIES (classes d'aide)
@use 'utilities';
```

---

## 1. Abstracts — Le système de tokens

### _index.scss

```scss
// src/abstracts/_index.scss

@forward 'variables';
@forward 'colors';
@forward 'typography';
@forward 'spacing';
@forward 'breakpoints';
@forward 'shadows';
@forward 'borders';
@forward 'transitions';
@forward 'z-index';
@forward 'functions';
@forward 'mixins';
```

### _variables.scss

```scss
// src/abstracts/_variables.scss

// ============================================================
// VARIABLES GÉNÉRALES
// ============================================================

// Conteneur
$container-max-width: 1200px !default;
$container-padding: 16px !default;

// Z-index base
$z-dropdown: 1000 !default;
$z-sticky: 1020 !default;
$z-fixed: 1030 !default;
$z-modal-backdrop: 1040 !default;
$z-modal: 1050 !default;
$z-tooltip: 1060 !default;
```

### _colors.scss

```scss
// src/abstracts/_colors.scss

// ============================================================
// PALETTE DE COULEURS
// ============================================================

// --- Bleu (Primary) ---
$blue-50: #eff6ff !default;
$blue-100: #dbeafe !default;
$blue-200: #bfdbfe !default;
$blue-300: #93c5fd !default;
$blue-400: #60a5fa !default;
$blue-500: #3b82f6 !default;
$blue-600: #2563eb !default;
$blue-700: #1d4ed8 !default;
$blue-800: #1e40af !default;
$blue-900: #1e3a8a !default;

// --- Vert (Secondary) ---
$green-50: #f0fdf4 !default;
$green-100: #dcfce7 !default;
$green-200: #bbf7d0 !default;
$green-300: #86efac !default;
$green-400: #4ade80 !default;
$green-500: #22c55e !default;
$green-600: #16a34a !default;
$green-700: #15803d !default;
$green-800: #166534 !default;
$green-900: #14532d !default;

// --- Rouge (Danger) ---
$red-50: #fef2f2 !default;
$red-100: #fee2e2 !default;
$red-200: #fecaca !default;
$red-300: #fca5a5 !default;
$red-400: #f87171 !default;
$red-500: #ef4444 !default;
$red-600: #dc2626 !default;
$red-700: #b91c1c !default;
$red-800: #991b1b !default;
$red-900: #7f1d1d !default;

// --- Orange (Warning) ---
$orange-50: #fff7ed !default;
$orange-100: #ffedd5 !default;
$orange-200: #fed7aa !default;
$orange-300: #fdba74 !default;
$orange-400: #fb923c !default;
$orange-500: #f97316 !default;
$orange-600: #ea580c !default;
$orange-700: #c2410c !default;
$orange-800: #9a3412 !default;
$orange-900: #7c2d12 !default;

// --- Gris (Neutres) ---
$gray-50: #f9fafb !default;
$gray-100: #f3f4f6 !default;
$gray-200: #e5e7eb !default;
$gray-300: #d1d5db !default;
$gray-400: #9ca3af !default;
$gray-500: #6b7280 !default;
$gray-600: #4b5563 !default;
$gray-700: #374151 !default;
$gray-800: #1f2937 !default;
$gray-900: #111827 !default;

// --- Couleurs sémantiques ---
$color-primary: $blue-500 !default;
$color-primary-light: $blue-300 !default;
$color-primary-dark: $blue-700 !default;

$color-secondary: $green-500 !default;
$color-secondary-light: $green-300 !default;
$color-secondary-dark: $green-700 !default;

$color-danger: $red-500 !default;
$color-danger-light: $red-300 !default;
$color-danger-dark: $red-700 !default;

$color-warning: $orange-500 !default;
$color-warning-light: $orange-300 !default;
$color-warning-dark: $orange-700 !default;

$color-success: $green-500 !default;
$color-success-light: $green-300 !default;
$color-success-dark: $green-700 !default;

// --- Neutres sémantiques ---
$color-text: $gray-900 !default;
$color-text-secondary: $gray-600 !default;
$color-text-muted: $gray-400 !default;
$color-text-inverse: #ffffff !default;

$color-bg: #ffffff !default;
$color-bg-secondary: $gray-50 !default;
$color-bg-tertiary: $gray-100 !default;

$color-border: $gray-200 !default;
$color-border-hover: $gray-300 !default;
$color-border-focus: $color-primary !default;

// --- Map complète des couleurs ---
$colors: (
  'primary': $color-primary,
  'primary-light': $color-primary-light,
  'primary-dark': $color-primary-dark,
  'secondary': $color-secondary,
  'danger': $color-danger,
  'warning': $color-warning,
  'success': $color-success,
  'text': $color-text,
  'text-secondary': $color-text-secondary,
  'text-muted': $color-text-muted,
  'bg': $color-bg,
  'bg-secondary': $color-bg-secondary,
  'border': $color-border,
) !default;
```

### _typography.scss

```scss
// src/abstracts/_typography.scss

// ============================================================
// TYPOGRAPHIE
// ============================================================

// --- Polices ---
$font-sans: (
  'Inter',
  -apple-system,
  BlinkMacSystemFont,
  'Segoe UI',
  Roboto,
  sans-serif,
) !default;

$font-mono: (
  'JetBrains Mono',
  'Fira Code',
  'Courier New',
  monospace,
) !default;

// --- Tailles ---
$font-size-xs: 0.75rem !default;    // 12px
$font-size-sm: 0.875rem !default;   // 14px
$font-size-base: 1rem !default;     // 16px
$font-size-lg: 1.125rem !default;   // 18px
$font-size-xl: 1.25rem !default;    // 20px
$font-size-2xl: 1.5rem !default;    // 24px
$font-size-3xl: 1.875rem !default;  // 30px
$font-size-4xl: 2.25rem !default;   // 36px
$font-size-5xl: 3rem !default;      // 48px

$font-size-map: (
  xs: $font-size-xs,
  sm: $font-size-sm,
  base: $font-size-base,
  lg: $font-size-lg,
  xl: $font-size-xl,
  '2xl': $font-size-2xl,
  '3xl': $font-size-3xl,
  '4xl': $font-size-4xl,
  '5xl': $font-size-5xl,
) !default;

// --- Poids ---
$font-weight-light: 300 !default;
$font-weight-normal: 400 !default;
$font-weight-medium: 500 !default;
$font-weight-semibold: 600 !default;
$font-weight-bold: 700 !default;

// --- Interlignes ---
$line-height-tight: 1.25 !default;
$line-height-snug: 1.375 !default;
$line-height-normal: 1.5 !default;
$line-height-relaxed: 1.625 !default;
$line-height-loose: 2 !default;
```

### _spacing.scss

```scss
// src/abstracts/_spacing.scss

// ============================================================
// ESPACEMENT
// ============================================================

$spacing-unit: 4px !default;

$spacing-0: 0 !default;
$spacing-px: 1px !default;
$spacing-0-5: $spacing-unit * 0.5 !default;  // 2px
$spacing-1: $spacing-unit !default;           // 4px
$spacing-1-5: $spacing-unit * 1.5 !default;   // 6px
$spacing-2: $spacing-unit * 2 !default;       // 8px
$spacing-3: $spacing-unit * 3 !default;       // 12px
$spacing-4: $spacing-unit * 4 !default;       // 16px
$spacing-5: $spacing-unit * 5 !default;       // 20px
$spacing-6: $spacing-unit * 6 !default;       // 24px
$spacing-8: $spacing-unit * 8 !default;       // 32px
$spacing-10: $spacing-unit * 10 !default;     // 40px
$spacing-12: $spacing-unit * 12 !default;     // 48px
$spacing-16: $spacing-unit * 16 !default;     // 64px
$spacing-20: $spacing-unit * 20 !default;     // 80px

$spacing-map: (
  0: $spacing-0,
  px: $spacing-px,
  0.5: $spacing-0-5,
  1: $spacing-1,
  1.5: $spacing-1-5,
  2: $spacing-2,
  3: $spacing-3,
  4: $spacing-4,
  5: $spacing-5,
  6: $spacing-6,
  8: $spacing-8,
  10: $spacing-10,
  12: $spacing-12,
  16: $spacing-16,
  20: $spacing-20,
) !default;
```

### _breakpoints.scss

```scss
// src/abstracts/_breakpoints.scss

// ============================================================
// BREAKPOINTS
// ============================================================

$breakpoint-sm: 576px !default;
$breakpoint-md: 768px !default;
$breakpoint-lg: 992px !default;
$breakpoint-xl: 1200px !default;
$breakpoint-2xl: 1400px !default;

$breakpoints: (
  sm: $breakpoint-sm,
  md: $breakpoint-md,
  lg: $breakpoint-lg,
  xl: $breakpoint-xl,
  '2xl': $breakpoint-2xl,
) !default;
```

### _shadows.scss

```scss
// src/abstracts/_shadows.scss

// ============================================================
// OMBRES
// ============================================================

$shadow-none: none !default;
$shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05) !default;
$shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.1) !default;
$shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -4px rgba(0, 0, 0, 0.1) !default;
$shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1) !default;
$shadow-2xl: 0 25px 50px -12px rgba(0, 0, 0, 0.25) !default;
$shadow-inner: inset 0 2px 4px 0 rgba(0, 0, 0, 0.05) !default;

$shadow-map: (
  none: $shadow-none,
  sm: $shadow-sm,
  md: $shadow-md,
  lg: $shadow-lg,
  xl: $shadow-xl,
  '2xl': $shadow-2xl,
  inner: $shadow-inner,
) !default;
```

### _borders.scss

```scss
// src/abstracts/_borders.scss

// ============================================================
// BORDURES
// ============================================================

$border-width: 1px !default;
$border-color: $color-border !default;

$border-radius-none: 0 !default;
$border-radius-sm: 0.25rem !default;   // 4px
$border-radius-md: 0.5rem !default;    // 8px
$border-radius-lg: 0.75rem !default;   // 12px
$border-radius-xl: 1rem !default;      // 16px
$border-radius-full: 9999px !default;

$border-radius-map: (
  none: $border-radius-none,
  sm: $border-radius-sm,
  md: $border-radius-md,
  lg: $border-radius-lg,
  xl: $border-radius-xl,
  full: $border-radius-full,
) !default;
```

### _transitions.scss

```scss
// src/abstracts/_transitions.scss

// ============================================================
// TRANSITIONS
// ============================================================

$duration-fast: 150ms !default;
$duration-normal: 300ms !default;
$duration-slow: 500ms !default;

$ease-default: cubic-bezier(0.4, 0, 0.2, 1) !default;
$ease-in: cubic-bezier(0.4, 0, 1, 1) !default;
$ease-out: cubic-bezier(0, 0, 0.2, 1) !default;
$ease-in-out: cubic-bezier(0.4, 0, 0.2, 1) !default;
$ease-bounce: cubic-bezier(0.68, -0.55, 0.265, 1.55) !default;

$transition-base: $duration-normal $ease-default !default;
$transition-fast: $duration-fast $ease-default !default;
$transition-slow: $duration-slow $ease-default !default;
```

### _z-index.scss

```scss
// src/abstracts/_z-index.scss

// ============================================================
// Z-INDEX
// ============================================================

$z-dropdown: 1000 !default;
$z-sticky: 1020 !default;
$z-fixed: 1030 !default;
$z-modal-backdrop: 1040 !default;
$z-modal: 1050 !default;
$z-tooltip: 1060 !default;

$z-map: (
  dropdown: $z-dropdown,
  sticky: $z-sticky,
  fixed: $z-fixed,
  modal-backdrop: $z-modal-backdrop,
  modal: $z-modal,
  tooltip: $z-tooltip,
) !default;
```

### _functions.scss

```scss
// src/abstracts/_functions.scss

// ============================================================
// FONCTIONS
// ============================================================

@use 'colors' as *;

// Récupérer une couleur de la map
@function color($name) {
  @if map-has-key($colors, $name) {
    @return map-get($colors, $name);
  }
  @warn "Couleur `#{$name}` introuvable.";
  @return null;
}

// Récupérer un espace
@function space($size) {
  @if map-has-key($spacing-map, $size) {
    @return map-get($spacing-map, $size);
  }
  @warn "Taille `#{$size}` introuvable.";
  @return null;
}

// Récupérer une taille de police
@function font-size($size) {
  @if map-has-key($font-size-map, $size) {
    @return map-get($font-size-map, $size);
  }
  @warn "Taille `#{$size}` introuvable.";
  @return null;
}

// Récupérer une ombre
@function shadow($name) {
  @if map-has-key($shadow-map, $name) {
    @return map-get($shadow-map, $name);
  }
  @warn "Ombre `#{$name}` introuvable.";
  @return null;
}

// Récupérer un border-radius
@function radius($name) {
  @if map-has-key($border-radius-map, $name) {
    @return map-get($border-radius-map, $name);
  }
  @warn "Border radius `#{$name}` introuvable.";
  @return null;
}

// Récupérer un z-index
@function z($layer) {
  @if map-has-key($z-map, $layer) {
    @return map-get($z-map, $layer);
  }
  @warn "Couche z-index `#{$layer}` introuvable.";
  @return null;
}
```

### _mixins.scss

```scss
// src/abstracts/_mixins.scss

// ============================================================
// MIXINS
// ============================================================

@use 'breakpoints' as *;

// --- Responsive ---
@mixin respond-to($breakpoint) {
  $value: map-get($breakpoints, $breakpoint);

  @if $value {
    @media (min-width: $value) {
      @content;
    }
  } @else {
    @warn "Breakpoint `#{$breakpoint}` introuvable.";
  }
}

@mixin respond-below($breakpoint) {
  $value: map-get($breakpoints, $breakpoint);

  @if $value {
    @media (max-width: ($value - 0.02px)) {
      @content;
    }
  }
}

// --- Layout ---
@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

@mixin flex-between {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

@mixin flex-column {
  display: flex;
  flex-direction: column;
}

// --- Accessibilité ---
@mixin visually-hidden {
  position: absolute !important;
  width: 1px !important;
  height: 1px !important;
  padding: 0 !important;
  margin: -1px !important;
  overflow: hidden !important;
  clip: rect(0, 0, 0, 0) !important;
  white-space: nowrap !important;
  border: 0 !important;
}

// --- Texte ---
@mixin truncate($max-width: 100%) {
  max-width: $max-width;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

@mixin line-clamp($lines: 2) {
  display: -webkit-box;
  -webkit-line-clamp: $lines;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

// --- Transitions ---
@mixin transition($properties: all, $duration: $duration-normal, $easing: $ease-default) {
  transition-property: $properties;
  transition-duration: $duration;
  transition-timing-function: $easing;
}

@mixin transition-colors {
  @include transition(color, background-color, border-color, box-shadow);
}

// --- Taille ---
@mixin size($width, $height: $width) {
  width: $width;
  height: $height;
}

@mixin square($size) {
  @include size($size);
}

// --- Position ---
@mixin absolute-fill {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}

// --- Scrollbar ---
@mixin scrollbar($width: 6px, $thumb: $gray-300) {
  &::-webkit-scrollbar {
    width: $width;
  }

  &::-webkit-scrollbar-track {
    background: transparent;
  }

  &::-webkit-scrollbar-thumb {
    background: $thumb;
    border-radius: $border-radius-full;
  }
}
```

---

## 2. Base — Styles globaux

### _index.scss

```scss
// src/base/_index.scss

@use '../abstracts' as *;
@forward 'reset';
@forward 'typography';
@forward 'global';
```

### _reset.scss

```scss
// src/base/_reset.scss

@use '../abstracts' as *;

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: 100%;
  scroll-behavior: smooth;
  -webkit-text-size-adjust: 100%;
}

body {
  min-height: 100vh;
  text-rendering: optimizeSpeed;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

img,
picture,
video,
canvas,
svg {
  display: block;
  max-width: 100%;
}

input,
button,
textarea,
select {
  font: inherit;
}

p, h1, h2, h3, h4, h5, h6 {
  overflow-wrap: break-word;
}

a {
  color: inherit;
  text-decoration: none;
}

ul, ol {
  list-style: none;
  padding: 0;
  margin: 0;
}

button {
  cursor: pointer;
  border: none;
  background: none;
}

table {
  border-collapse: collapse;
}
```

### _typography.scss

```scss
// src/base/_typography.scss

@use '../abstracts' as *;

body {
  font-family: $font-sans;
  font-size: $font-size-base;
  line-height: $line-height-normal;
  color: $color-text;
  background-color: var(--color-bg, $color-bg);
}

h1, h2, h3, h4, h5, h6 {
  font-weight: $font-weight-bold;
  line-height: $line-height-tight;
  color: var(--color-text, $color-text);
}

h1 { font-size: $font-size-4xl; }
h2 { font-size: $font-size-3xl; }
h3 { font-size: $font-size-2xl; }
h4 { font-size: $font-size-xl; }
h5 { font-size: $font-size-lg; }
h6 { font-size: $font-size-base; }

p {
  line-height: $line-height-relaxed;
  margin-bottom: $spacing-md;
}

a {
  color: var(--color-primary, $color-primary);
  @include transition(color);

  &:hover {
    color: var(--color-primary-dark, $color-primary-dark);
  }

  &:focus-visible {
    outline: 2px solid var(--color-primary, $color-primary);
    outline-offset: 2px;
  }
}

strong { font-weight: $font-weight-bold; }
small { font-size: $font-size-sm; }

code {
  font-family: $font-mono;
  font-size: 0.9em;
  background-color: var(--color-bg-secondary, $color-bg-secondary);
  padding: 2px 6px;
  border-radius: $border-radius-sm;
}

hr {
  border: none;
  border-top: $border-width solid var(--color-border, $color-border);
  margin: $spacing-lg 0;
}

::selection {
  background-color: var(--color-primary, $color-primary);
  color: $color-text-inverse;
}
```

### _global.scss

```scss
// src/base/_global.scss

@use '../abstracts' as *;

.sr-only {
  @include visually-hidden;
}

.container {
  width: 100%;
  max-width: $container-max-width;
  margin-left: auto;
  margin-right: auto;
  padding-left: $container-padding;
  padding-right: $container-padding;
}

// Respect de prefers-reduced-motion
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

## 3. Layout

### _index.scss

```scss
// src/layout/_index.scss

@use '../abstracts' as *;
@forward 'grid';
@forward 'container';
@forward 'header';
@forward 'footer';
```

### _grid.scss

```scss
// src/layout/_grid.scss

@use '../abstracts' as *;

.row {
  display: flex;
  flex-wrap: wrap;
  margin-left: -$spacing-2;
  margin-right: -$spacing-2;
}

.col {
  flex: 1;
  padding-left: $spacing-2;
  padding-right: $spacing-2;
}

// Génération des colonnes
@for $i from 1 through 12 {
  .col-#{$i} {
    flex: 0 0 calc(#{$i} / 12 * 100%);
    max-width: calc(#{$i} / 12 * 100%);
    padding-left: $spacing-2;
    padding-right: $spacing-2;
  }
}

// Responsive : md
@include respond-to('md') {
  @for $i from 1 through 12 {
    .col-md-#{$i} {
      flex: 0 0 calc(#{$i} / 12 * 100%);
      max-width: calc(#{$i} / 12 * 100%);
    }
  }
}

// Responsive : lg
@include respond-to('lg') {
  @for $i from 1 through 12 {
    .col-lg-#{$i} {
      flex: 0 0 calc(#{$i} / 12 * 100%);
      max-width: calc(#{$i} / 12 * 100%);
    }
  }
}

// Grid CSS moderne
.grid {
  display: grid;
  gap: $spacing-md;

  &--2 { grid-template-columns: repeat(2, 1fr); }
  &--3 { grid-template-columns: repeat(3, 1fr); }
  &--4 { grid-template-columns: repeat(4, 1fr); }

  &--auto {
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  }
}

@include respond-to('md') {
  .grid {
    &--md-2 { grid-template-columns: repeat(2, 1fr); }
    &--md-3 { grid-template-columns: repeat(3, 1fr); }
    &--md-4 { grid-template-columns: repeat(4, 1fr); }
  }
}
```

### _container.scss

```scss
// src/layout/_container.scss

@use '../abstracts' as *;

.container {
  width: 100%;
  max-width: $container-max-width;
  margin-left: auto;
  margin-right: auto;
  padding-left: $container-padding;
  padding-right: $container-padding;

  &--narrow {
    max-width: 800px;
  }

  &--wide {
    max-width: 1400px;
  }

  &--full {
    max-width: none;
    padding-left: 0;
    padding-right: 0;
  }
}
```

### _header.scss

```scss
// src/layout/_header.scss

@use '../abstracts' as *;

.header {
  position: sticky;
  top: 0;
  z-index: $z-sticky;
  background-color: var(--color-bg, $color-bg);
  box-shadow: $shadow-sm;
  border-bottom: $border-width solid var(--color-border, $color-border);
  @include transition(background-color, box-shadow);

  &__inner {
    @include flex-between;
    max-width: $container-max-width;
    margin: 0 auto;
    padding: $spacing-3 $container-padding;
  }

  &__brand {
    font-size: $font-size-xl;
    font-weight: $font-weight-bold;
    color: var(--color-primary, $color-primary);
    letter-spacing: -0.025em;
  }

  &__nav {
    @include flex-center;
    gap: $spacing-1;
  }

  &__link {
    padding: $spacing-1 $spacing-3;
    font-size: $font-size-sm;
    font-weight: $font-weight-medium;
    color: var(--color-text-secondary, $color-text-secondary);
    border-radius: $border-radius-md;
    @include transition(color, background-color);

    &:hover {
      color: var(--color-text, $color-text);
      background-color: var(--color-bg-secondary, $color-bg-secondary);
    }

    &--active {
      color: var(--color-primary, $color-primary);
      background-color: var(--color-bg-secondary, $color-bg-secondary);
    }
  }

  &__toggle {
    display: block;
    padding: $spacing-2;

    @include respond-to('md') {
      display: none;
    }
  }

  @include respond-below('md') {
    &__nav {
      display: none;

      &--open {
        display: flex;
        flex-direction: column;
        position: absolute;
        top: 100%;
        left: 0;
        right: 0;
        background-color: var(--color-bg, $color-bg);
        box-shadow: $shadow-md;
        padding: $spacing-4;
        gap: $spacing-1;
      }
    }
  }
}
```

### _footer.scss

```scss
// src/layout/_footer.scss

@use '../abstracts' as *;

.footer {
  background-color: var(--color-text, $color-text);
  color: var(--color-text-inverse, $color-text-inverse);
  padding: $spacing-16 0 $spacing-8;

  &__inner {
    max-width: $container-max-width;
    margin: 0 auto;
    padding: 0 $container-padding;
    display: grid;
    gap: $spacing-8;
    grid-template-columns: 1fr;

    @include respond-to('md') {
      grid-template-columns: 2fr 1fr 1fr;
    }
  }

  &__brand {
    font-size: $font-size-xl;
    font-weight: $font-weight-bold;
    margin-bottom: $spacing-4;
  }

  &__text {
    opacity: 0.7;
    line-height: $line-height-relaxed;
  }

  &__title {
    font-size: $font-size-sm;
    font-weight: $font-weight-bold;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    margin-bottom: $spacing-4;
    opacity: 0.5;
  }

  &__link {
    display: block;
    color: inherit;
    opacity: 0.7;
    padding: $spacing-1 0;
    @include transition(opacity);

    &:hover {
      opacity: 1;
    }
  }

  &__bottom {
    max-width: $container-max-width;
    margin: $spacing-8 auto 0;
    padding: $spacing-4 $container-padding 0;
    border-top: $border-width solid rgba(255, 255, 255, 0.1);
    text-align: center;
    font-size: $font-size-sm;
    opacity: 0.5;
  }
}
```

---

## 4. Components

### _index.scss

```scss
// src/components/_index.scss

@use '../abstracts' as *;
@forward 'buttons';
@forward 'cards';
@forward 'forms';
@forward 'badges';
@forward 'alerts';
```

### _buttons.scss

```scss
// src/components/_buttons.scss

@use '../abstracts' as *;

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: $spacing-2;
  padding: $spacing-2 $spacing-5;
  font-family: $font-sans;
  font-size: $font-size-sm;
  font-weight: $font-weight-medium;
  line-height: 1;
  border-radius: $border-radius-md;
  border: 2px solid transparent;
  cursor: pointer;
  @include transition(all);

  &:focus-visible {
    outline: 2px solid var(--color-primary, $color-primary);
    outline-offset: 2px;
  }

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    pointer-events: none;
  }

  // --- Variantes ---
  &--primary {
    background-color: var(--color-primary, $color-primary);
    color: $color-text-inverse;

    &:hover {
      background-color: var(--color-primary-dark, $color-primary-dark);
    }
  }

  &--secondary {
    background-color: var(--color-secondary, $color-secondary);
    color: $color-text-inverse;

    &:hover {
      background-color: var(--color-secondary-dark, $color-secondary-dark);
    }
  }

  &--danger {
    background-color: var(--color-danger, $color-danger);
    color: $color-text-inverse;

    &:hover {
      background-color: var(--color-danger-dark, $color-danger-dark);
    }
  }

  &--outline {
    background-color: transparent;
    border-color: var(--color-primary, $color-primary);
    color: var(--color-primary, $color-primary);

    &:hover {
      background-color: var(--color-primary, $color-primary);
      color: $color-text-inverse;
    }
  }

  &--ghost {
    background-color: transparent;
    color: var(--color-primary, $color-primary);

    &:hover {
      background-color: var(--color-bg-secondary, $color-bg-secondary);
    }
  }

  // --- Tailles ---
  &--sm {
    padding: $spacing-1 $spacing-3;
    font-size: $font-size-xs;
  }

  &--lg {
    padding: $spacing-3 $spacing-6;
    font-size: $font-size-base;
  }

  &--block {
    display: flex;
    width: 100%;
  }

  // --- États ---
  &--loading {
    position: relative;
    color: transparent !important;
    pointer-events: none;

    &::after {
      content: '';
      position: absolute;
      @include size(16px);
      border: 2px solid currentColor;
      border-top-color: transparent;
      border-radius: 50%;
      animation: spin 0.6s linear infinite;
    }
  }
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

### _cards.scss

```scss
// src/components/_cards.scss

@use '../abstracts' as *;

.card {
  background-color: var(--color-bg, $color-bg);
  border: $border-width solid var(--color-border, $color-border);
  border-radius: $border-radius-lg;
  overflow: hidden;
  @include transition(box-shadow, transform);

  &:hover {
    box-shadow: $shadow-lg;
  }

  &--clickable {
    cursor: pointer;

    &:hover {
      transform: translateY(-2px);
    }

    &:focus-visible {
      outline: 2px solid var(--color-primary, $color-primary);
      outline-offset: 2px;
    }
  }

  &--flat {
    box-shadow: none;
    border: $border-width solid var(--color-border, $color-border);

    &:hover {
      box-shadow: none;
      border-color: var(--color-primary, $color-primary);
    }
  }

  &__image {
    width: 100%;
    aspect-ratio: 16 / 9;
    object-fit: cover;
  }

  &__body {
    padding: $spacing-5;
  }

  &__header {
    padding: $spacing-5;
    border-bottom: $border-width solid var(--color-border, $color-border);
  }

  &__footer {
    padding: $spacing-4 $spacing-5;
    border-top: $border-width solid var(--color-border, $color-border);
    background-color: var(--color-bg-secondary, $color-bg-secondary);
    display: flex;
    align-items: center;
    justify-content: flex-end;
    gap: $spacing-2;
  }

  &__title {
    font-size: $font-size-xl;
    font-weight: $font-weight-bold;
    margin-bottom: $spacing-2;
    color: var(--color-text, $color-text);
  }

  &__subtitle {
    font-size: $font-size-sm;
    color: var(--color-text-secondary, $color-text-secondary);
    margin-bottom: $spacing-3;
  }

  &__text {
    color: var(--color-text-secondary, $color-text-secondary);
    line-height: $line-height-relaxed;
    @include line-clamp(3);
  }

  &__meta {
    display: flex;
    align-items: center;
    gap: $spacing-2;
    font-size: $font-size-xs;
    color: var(--color-text-muted, $color-text-muted);
    margin-top: $spacing-3;
  }

  // --- Layout horizontal ---
  &--horizontal {
    @include respond-to('md') {
      display: flex;

      .card__image {
        width: 240px;
        flex-shrink: 0;
        aspect-ratio: auto;
        height: 100%;
      }
    }
  }
}
```

### _forms.scss

```scss
// src/components/_forms.scss

@use '../abstracts' as *;

.form {
  &__group {
    margin-bottom: $spacing-5;
  }

  &__label {
    display: block;
    font-size: $font-size-sm;
    font-weight: $font-weight-medium;
    color: var(--color-text, $color-text);
    margin-bottom: $spacing-1-5;
  }

  &__required {
    color: var(--color-danger, $color-danger);
    margin-left: 2px;
  }

  &__input,
  &__textarea,
  &__select {
    display: block;
    width: 100%;
    padding: $spacing-2-5 $spacing-3;
    font-family: $font-sans;
    font-size: $font-size-sm;
    color: var(--color-text, $color-text);
    background-color: var(--color-bg, $color-bg);
    border: 2px solid var(--color-border, $color-border);
    border-radius: $border-radius-md;
    @include transition(border-color, box-shadow);

    &:hover {
      border-color: var(--color-border-hover, $color-border-hover);
    }

    &:focus {
      outline: none;
      border-color: var(--color-primary, $color-primary);
      box-shadow: 0 0 0 3px rgba($color-primary, 0.15);
    }

    &::placeholder {
      color: var(--color-text-muted, $color-text-muted);
    }

    &--error {
      border-color: var(--color-danger, $color-danger);

      &:focus {
        box-shadow: 0 0 0 3px rgba($color-danger, 0.15);
      }
    }

    &--success {
      border-color: var(--color-success, $color-success);

      &:focus {
        box-shadow: 0 0 0 3px rgba($color-success, 0.15);
      }
    }
  }

  &__textarea {
    min-height: 120px;
    resize: vertical;
  }

  &__select {
    appearance: none;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%236b7280' d='M6 8L1 3h10z'/%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-position: right $spacing-3 center;
    padding-right: $spacing-10;
  }

  &__checkbox,
  &__radio {
    display: flex;
    align-items: center;
    gap: $spacing-2;
    cursor: pointer;
    font-size: $font-size-sm;

    input {
      width: 18px;
      height: 18px;
      accent-color: var(--color-primary, $color-primary);
      cursor: pointer;
    }
  }

  &__help {
    font-size: $font-size-xs;
    color: var(--color-text-muted, $color-text-muted);
    margin-top: $spacing-1;
  }

  &__error {
    font-size: $font-size-xs;
    color: var(--color-danger, $color-danger);
    margin-top: $spacing-1;
    display: flex;
    align-items: center;
    gap: 4px;
  }

  &__row {
    display: grid;
    gap: $spacing-4;

    @include respond-to('md') {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  &__actions {
    display: flex;
    gap: $spacing-3;
    margin-top: $spacing-6;

    @include respond-to('md') {
      justify-content: flex-end;
    }
  }
}
```

### _badges.scss

```scss
// src/components/_badges.scss

@use '../abstracts' as *;

.badge {
  display: inline-flex;
  align-items: center;
  padding: $spacing-0-5 $spacing-2;
  font-size: 11px;
  font-weight: $font-weight-bold;
  line-height: 1.5;
  border-radius: $border-radius-full;
  text-transform: uppercase;
  letter-spacing: 0.05em;

  &--primary {
    background-color: rgba($color-primary, 0.15);
    color: var(--color-primary, $color-primary);
  }

  &--secondary {
    background-color: rgba($color-secondary, 0.15);
    color: var(--color-secondary-dark, $color-secondary-dark);
  }

  &--danger {
    background-color: rgba($color-danger, 0.15);
    color: var(--color-danger, $color-danger);
  }

  &--warning {
    background-color: rgba($color-warning, 0.15);
    color: var(--color-warning-dark, $color-warning-dark);
  }

  &--success {
    background-color: rgba($color-success, 0.15);
    color: var(--color-success-dark, $color-success-dark);
  }

  &--dark {
    background-color: rgba($color-text, 0.15);
    color: var(--color-text, $color-text);
  }

  &--pill {
    padding: $spacing-0-5 $spacing-3;
  }
}
```

### _alerts.scss

```scss
// src/components/_alerts.scss

@use '../abstracts' as *;

.alert {
  padding: $spacing-4 $spacing-5;
  border-radius: $border-radius-md;
  border-left: 4px solid;
  margin-bottom: $spacing-4;

  &--success {
    background-color: rgba($color-success, 0.1);
    border-color: $color-success;
    color: $color-success-dark;
  }

  &--danger {
    background-color: rgba($color-danger, 0.1);
    border-color: $color-danger;
    color: $color-danger-dark;
  }

  &--warning {
    background-color: rgba($color-warning, 0.1);
    border-color: $color-warning;
    color: $color-warning-dark;
  }

  &--info {
    background-color: rgba($color-primary, 0.1);
    border-color: $color-primary;
    color: $color-primary-dark;
  }

  &__title {
    font-weight: $font-weight-bold;
    margin-bottom: $spacing-1;
  }

  &__text {
    line-height: $line-height-relaxed;
  }

  &__close {
    float: right;
    font-size: $font-size-lg;
    line-height: 1;
    opacity: 0.5;
    @include transition(opacity);

    &:hover {
      opacity: 1;
    }
  }
}
```

---

## 5. Themes

### _index.scss

```scss
// src/themes/_index.scss

@use '../abstracts' as *;
@forward 'light';
@forward 'dark';
```

### _light.scss

```scss
// src/themes/_light.scss

:root,
[data-theme="light"] {
  --color-primary: #{$color-primary};
  --color-primary-light: #{$color-primary-light};
  --color-primary-dark: #{$color-primary-dark};
  --color-secondary: #{$color-secondary};
  --color-secondary-dark: #{$color-secondary-dark};
  --color-danger: #{$color-danger};
  --color-danger-dark: #{$color-danger-dark};
  --color-warning: #{$color-warning};
  --color-warning-dark: #{$color-warning-dark};
  --color-success: #{$color-success};
  --color-success-dark: #{$color-success-dark};

  --color-text: #{$color-text};
  --color-text-secondary: #{$color-text-secondary};
  --color-text-muted: #{$color-text-muted};

  --color-bg: #{$color-bg};
  --color-bg-secondary: #{$color-bg-secondary};
  --color-bg-tertiary: #{$color-bg-tertiary};

  --color-border: #{$color-border};
  --color-border-hover: #{$color-border-hover};
}
```

### _dark.scss

```scss
// src/themes/_dark.scss

[data-theme="dark"] {
  --color-primary: #{lighten($color-primary, 10%)};
  --color-primary-light: #{lighten($color-primary, 25%)};
  --color-primary-dark: #{$color-primary};
  --color-secondary: #{lighten($color-secondary, 10%)};
  --color-secondary-dark: #{$color-secondary};
  --color-danger: #{lighten($color-danger, 10%)};
  --color-danger-dark: #{$color-danger};
  --color-warning: #{lighten($color-warning, 10%)};
  --color-warning-dark: #{$color-warning};
  --color-success: #{lighten($color-success, 10%)};
  --color-success-dark: #{$color-success};

  --color-text: #e2e8f0;
  --color-text-secondary: #a0aec0;
  --color-text-muted: #718096;

  --color-bg: #1a202c;
  --color-bg-secondary: #2d3748;
  --color-bg-tertiary: #4a5568;

  --color-border: #4a5568;
  --color-border-hover: #718096;
}
```

---

## 6. Utilities

### _index.scss

```scss
// src/utilities/_index.scss

@use '../abstracts' as *;
@forward 'display';
@forward 'text';
@forward 'spacing';
@forward 'generated';
```

### _display.scss

```scss
// src/utilities/_display.scss

.u-hidden { display: none !important; }
.u-block { display: block !important; }
.u-flex { display: flex !important; }
.u-grid { display: grid !important; }
.u-inline { display: inline !important; }
.u-inline-block { display: inline-block !important; }
.u-inline-flex { display: inline-flex !important; }

.u-flex-center { @include flex-center; }
.u-flex-between { @include flex-between; }
.u-flex-col { @include flex-column; }
.u-flex-wrap { flex-wrap: wrap !important; }

.u-items-center { align-items: center !important; }
.u-items-start { align-items: flex-start !important; }
.u-items-end { align-items: flex-end !important; }
.u-justify-center { justify-content: center !important; }
.u-justify-between { justify-content: space-between !important; }
.u-justify-end { justify-content: flex-end !important; }

.u-w-full { width: 100% !important; }
.u-w-auto { width: auto !important; }
.u-max-w-sm { max-width: 540px !important; }
.u-max-w-md { max-width: 720px !important; }
.u-max-w-lg { max-width: 960px !important; }
.u-max-w-xl { max-width: 1140px !important; }

.u-mx-auto { margin-left: auto !important; margin-right: auto !important; }

.u-overflow-hidden { overflow: hidden !important; }
.u-relative { position: relative !important; }
.u-absolute { position: absolute !important; }
.u-sticky { position: sticky !important; }
```

### _text.scss

```scss
// src/utilities/_text.scss

.u-text-center { text-align: center !important; }
.u-text-left { text-align: left !important; }
.u-text-right { text-align: right !important; }

.u-text-uppercase { text-transform: uppercase !important; }
.u-text-lowercase { text-transform: lowercase !important; }
.u-text-capitalize { text-transform: capitalize !important; }
.u-text-truncate { @include truncate; }

.u-text-primary { color: var(--color-primary, $color-primary) !important; }
.u-text-danger { color: var(--color-danger, $color-danger) !important; }
.u-text-success { color: var(--color-success, $color-success) !important; }
.u-text-muted { color: var(--color-text-muted, $color-text-muted) !important; }
.u-text-white { color: $color-text-inverse !important; }

.u-font-light { font-weight: $font-weight-light !important; }
.u-font-normal { font-weight: $font-weight-normal !important; }
.u-font-medium { font-weight: $font-weight-medium !important; }
.u-font-semibold { font-weight: $font-weight-semibold !important; }
.u-font-bold { font-weight: $font-weight-bold !important; }

@each $name, $value in $font-size-map {
  .u-text-#{$name} {
    font-size: $value !important;
  }
}
```

### _spacing.scss

```scss
// src/utilities/_spacing.scss

@each $size-name, $size-value in $spacing-map {
  // Marges
  .u-m-#{$size-name} { margin: $size-value !important; }
  .u-mt-#{$size-name} { margin-top: $size-value !important; }
  .u-mr-#{$size-name} { margin-right: $size-value !important; }
  .u-mb-#{$size-name} { margin-bottom: $size-value !important; }
  .u-ml-#{$size-name} { margin-left: $size-value !important; }
  .u-mx-#{$size-name} {
    margin-left: $size-value !important;
    margin-right: $size-value !important;
  }
  .u-my-#{$size-name} {
    margin-top: $size-value !important;
    margin-bottom: $size-value !important;
  }

  // Paddings
  .u-p-#{$size-name} { padding: $size-value !important; }
  .u-pt-#{$size-name} { padding-top: $size-value !important; }
  .u-pr-#{$size-name} { padding-right: $size-value !important; }
  .u-pb-#{$size-name} { padding-bottom: $size-value !important; }
  .u-pl-#{$size-name} { padding-left: $size-value !important; }
  .u-px-#{$size-name} {
    padding-left: $size-value !important;
    padding-right: $size-value !important;
  }
  .u-py-#{$size-name} {
    padding-top: $size-value !important;
    padding-bottom: $size-value !important;
  }
}
```

### _generated.scss

```scss
// src/utilities/_generated.scss

// Auto-générer les classes de shadow
@each $name, $value in $shadow-map {
  .u-shadow-#{$name} {
    box-shadow: $value !important;
  }
}

// Auto-générer les classes de border-radius
@each $name, $value in $border-radius-map {
  .u-rounded-#{$name} {
    border-radius: $value !important;
  }
}

// Gaps
.u-gap-1 { gap: $spacing-1 !important; }
.u-gap-2 { gap: $spacing-2 !important; }
.u-gap-3 { gap: $spacing-3 !important; }
.u-gap-4 { gap: $spacing-4 !important; }
.u-gap-5 { gap: $spacing-5 !important; }
.u-gap-6 { gap: $spacing-6 !important; }
.u-gap-8 { gap: $spacing-8 !important; }

// Opacity
.u-opacity-0 { opacity: 0 !important; }
.u-opacity-25 { opacity: 0.25 !important; }
.u-opacity-50 { opacity: 0.5 !important; }
.u-opacity-75 { opacity: 0.75 !important; }
.u-opacity-100 { opacity: 1 !important; }

// Cursor
.u-cursor-pointer { cursor: pointer !important; }
.u-cursor-not-allowed { cursor: not-allowed !important; }
```

---

## Exemple d'utilisation (index.html)

```html
<!DOCTYPE html>
<html lang="fr" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mini Design System</title>
  <link rel="stylesheet" href="dist/css/main.css">
</head>
<body>
  <!-- Header -->
  <header class="header">
    <div class="header__inner">
      <a href="#" class="header__brand">MiniDS</a>
      <nav class="header__nav">
        <a href="#" class="header__link header__link--active">Accueil</a>
        <a href="#" class="header__link">Composants</a>
        <a href="#" class="header__link">Documentation</a>
      </nav>
    </div>
  </header>

  <!-- Hero -->
  <section class="u-py-12 u-text-center">
    <div class="container">
      <h1 class="u-mb-4">Mini Design System</h1>
      <p class="u-text-lg u-text-muted u-mb-6" style="max-width: 600px; margin-left: auto; margin-right: auto;">
        Un système de design complet construit avec SCSS.
      </p>
      <div class="u-flex-center u-gap-3">
        <button class="btn btn--primary btn--lg">Commencer</button>
        <button class="btn btn--outline btn--lg">Documentation</button>
      </div>
    </div>
  </section>

  <!-- Cards -->
  <section class="u-py-8" style="background: var(--color-bg-secondary);">
    <div class="container">
      <h2 class="u-text-center u-mb-8">Composants</h2>
      <div class="grid grid--auto">
        <div class="card card--clickable">
          <div class="card__body">
            <span class="badge badge--primary u-mb-3">Nouveau</span>
            <h3 class="card__title">Boutons</h3>
            <p class="card__text">Plusieurs variantes et tailles disponibles.</p>
          </div>
        </div>
        <div class="card card--clickable">
          <div class="card__body">
            <span class="badge badge--success u-mb-3">Stable</span>
            <h3 class="card__title">Cartes</h3>
            <p class="card__text">Composant flexible pour afficher du contenu.</p>
          </div>
        </div>
        <div class="card card--clickable">
          <div class="card__body">
            <span class="badge badge--warning u-mb-3">Beta</span>
            <h3 class="card__title">Formulaires</h3>
            <p class="card__text">Inputs, selects, checkboxes et plus.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Theme Toggle -->
  <script>
    document.querySelector('.header__brand').addEventListener('click', () => {
      const html = document.documentElement;
      const current = html.getAttribute('data-theme');
      html.setAttribute('data-theme', current === 'dark' ? 'light' : 'dark');
    });
  </script>
</body>
</html>
```

---

## Installation et build

```bash
# Initialiser le projet
mkdir mini-design-system && cd mini-design-system
npm init -y
npm install --save-dev sass

# Compiler
npx sass src/main.scss dist/css/main.css --style=compressed --source-map

# Watch
npx sass --watch src/main.scss:dist/css/main.css --source-map
```

```json
// package.json (scripts)
{
  "scripts": {
    "build": "sass src/main.scss dist/css/main.css --style=compressed --source-map",
    "dev": "sass --watch src/main.scss:dist/css/main.css --source-map",
    "lint": "stylelint 'src/**/*.scss'"
  }
}
```

---

## Résumé

Ce Mini Design System démontre comment les concepts SCSS s'assemblent pour créer un système cohérent et maintenable. L'architecture 7-1, les `@use`/`@forward`, les tokens, les mixins et les utilities travaillent ensemble pour produire un CSS performant et un code source organisé. Ce projet peut servir de base à n'importe quel projet web réel.
