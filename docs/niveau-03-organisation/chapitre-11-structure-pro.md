# Chapitre 11 : Structure professionnelle

## Introduction

Dans ce chapitre final de la partie « Organisation », nous allons assembler **tous les concepts précédents** pour créer une **structure de projet Sass professionnelle**. Vous apprendrez à organiser vos fichiers, à créer un point d'entrée efficace et à adopter les conventions qui rendent votre projet maintenable à long terme.

---

## 11.1 La structure de dossiers

### Architecture recommandée

Voici la structure complète d'un projet Sass professionnel :

```
src/scss/
├── abstracts/                  → Outils (pas de CSS généré)
│   ├── _colors.scss
│   ├── _variables.scss
│   ├── _mixins.scss
│   ├── _functions.scss
│   ├── _tokens.scss
│   └── _index.scss
├── base/                       → Styles de base
│   ├── _reset.scss
│   ├── _typography.scss
│   ├── _animations.scss
│   ├── _accessibility.scss
│   └── _index.scss
├── layout/                     → Mise en page
│   ├── _grid.scss
│   ├── _header.scss
│   ├── _footer.scss
│   ├── _sidebar.scss
│   ├── _navigation.scss
│   └── _index.scss
├── components/                 → Composants UI
│   ├── _buttons.scss
│   ├── _cards.scss
│   ├── _forms.scss
│   ├── _modals.scss
│   ├── _alerts.scss
│   ├── _badges.scss
│   ├── _breadcrumbs.scss
│   ├── _pagination.scss
│   ├── _tabs.scss
│   ├── _tooltips.scss
│   └── _index.scss
├── pages/                      → Styles par page
│   ├── _home.scss
│   ├── _about.scss
│   ├── _contact.scss
│   ├── _blog.scss
│   └── _index.scss
├── themes/                     → Thèmes
│   ├── _light.scss
│   ├── _dark.scss
│   ├── _high-contrast.scss
│   └── _index.scss
├── vendors/                    → Bibliothèques externes
│   ├── _bootstrap-overrides.scss
│   ├── _font-awesome-overrides.scss
│   └── _index.scss
├── utilities/                  → Classes utilitaires
│   ├── _spacing.scss
│   ├── _visibility.scss
│   ├── _text.scss
│   ├── _flex.scss
│   └── _index.scss
└── main.scss                   → POINT D'ENTRÉE UNIQUE
```

### Chaque dossier a un rôle précis

| Dossier | Contenu | Génère du CSS ? |
|---------|---------|-----------------|
| `abstracts/` | Variables, mixins, fonctions, tokens | Non |
| `base/` | Reset, typographie, animations | Oui |
| `layout/` | Grid, header, footer, sidebar | Oui |
| `components/` | Boutons, cartes, formulaires, etc. | Oui |
| `pages/` | Styles spécifiques à chaque page | Oui |
| `themes/` | Thèmes clair/sombre/contraste | Oui |
| `vendors/` | Overrides de bibliothèques | Oui |
| `utilities/` | Classes utilitaires | Oui |

### Pourquoi cette structure fonctionne

1. **Clarté** : Chaque dossier a un rôle bien défini.
2. **Scalabilité** : Ajouter un nouveau composant est simple.
3. **Collaboration** : Chaque développeur sait où chercher.
4. **Maintenance** : Modifier un aspect du projet est rapide.
5. **Réutilisabilité** : Les abstracts peuvent être partagés entre projets.

---

## 11.2 Créer les fichiers partiels de chaque dossier

### abstracts/_colors.scss

```scss
// ============================================
// COULEURS DU PROJET
// ============================================

// --- Primaires ---
$color-primary: #3498db;
$color-primary-light: #5dade2;
$color-primary-dark: #2980b9;
$color-primary-ultra-dark: #1a5276;

// --- Secondaires ---
$color-secondary: #2ecc71;
$color-secondary-light: #58d68d;
$color-secondary-dark: #27ae60;

// --- Accent ---
$color-accent: #f39c12;
$color-accent-light: #f5b041;
$color-accent-dark: #d68910;

// --- Neutres ---
$color-white: #ffffff;
$color-black: #000000;
$color-gray-100: #f8f9fa;
$color-gray-200: #e9ecef;
$color-gray-300: #dee2e6;
$color-gray-400: #ced4da;
$color-gray-500: #adb5bd;
$color-gray-600: #6c757d;
$color-gray-700: #495057;
$color-gray-800: #343a40;
$color-gray-900: #212529;

// --- Sémantiques ---
$color-success: #27ae60;
$color-warning: #f39c12;
$color-danger: #e74c3c;
$color-info: #3498db;

// --- Maps ---
$colors: (
  "primary": $color-primary,
  "secondary": $color-secondary,
  "accent": $color-accent,
  "success": $color-success,
  "warning": $color-warning,
  "danger": $color-danger,
  "info": $color-info
);

$grays: (
  "100": $color-gray-100,
  "200": $color-gray-200,
  "300": $color-gray-300,
  "400": $color-gray-400,
  "500": $color-gray-500,
  "600": $color-gray-600,
  "700": $color-gray-700,
  "800": $color-gray-800,
  "900": $color-gray-900
);
```

### abstracts/_variables.scss

```scss
// ============================================
// VARIABLES GLOBALES DU PROJET
// ============================================

// --- Breakpoints ---
$breakpoint-sm: 576px;
$breakpoint-md: 768px;
$breakpoint-lg: 992px;
$breakpoint-xl: 1200px;
$breakpoint-xxl: 1400px;

// --- Z-index ---
$z-dropdown: 1000;
$z-sticky: 1020;
$z-fixed: 1030;
$z-modal-backdrop: 1040;
$z-modal: 1050;
$z-popover: 1060;
$z-tooltip: 1070;
$z-toast: 1080;

// --- Transitions ---
$transition-speed: 0.3s;
$transition-easing: cubic-bezier(0.4, 0, 0.2, 1);
$transition-speed-fast: 0.15s;
$transition-speed-slow: 0.5s;

// --- Espacement ---
$spacing-unit: 8px;
$spacing-xs: $spacing-unit * 0.5;
$spacing-sm: $spacing-unit;
$spacing-md: $spacing-unit * 2;
$spacing-lg: $spacing-unit * 3;
$spacing-xl: $spacing-unit * 4;
$spacing-xxl: $spacing-unit * 6;

// --- Bordures ---
$border-radius-sm: 3px;
$border-radius: 5px;
$border-radius-lg: 10px;
$border-radius-xl: 15px;
$border-radius-pill: 50px;
$border-radius-circle: 50%;

// --- Ombres ---
$shadow-xs: 0 1px 2px rgba(0, 0, 0, 0.05);
$shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.12);
$shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
$shadow-lg: 0 4px 16px rgba(0, 0, 0, 0.2);
$shadow-xl: 0 8px 32px rgba(0, 0, 0, 0.25);

// --- Container ---
$container-max-width: 1200px;
$container-padding: $spacing-md;

// --- Header ---
$header-height: 64px;

// --- Sidebar ---
$sidebar-width: 260px;

// --- Font ---
$font-family-base: 'Inter', -apple-system, BlinkMacSystemFont,
  'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
$font-family-mono: 'Fira Code', 'Consolas', 'Monaco', monospace;
$font-size-base: 16px;
$font-size-sm: 14px;
$font-size-lg: 18px;
$font-size-xl: 20px;
$line-height-base: 1.6;
$font-weight-light: 300;
$font-weight-normal: 400;
$font-weight-medium: 500;
$font-weight-semibold: 600;
$font-weight-bold: 700;
```

### abstracts/_mixins.scss

```scss
// ============================================
// MIXINS UTILITAIRES
// ============================================

@use 'variables' as *;
@use 'colors' as *;

// --- Responsive ---
@mixin respond-to($breakpoint) {
  @if $breakpoint == "sm" {
    @media (min-width: $breakpoint-sm) { @content; }
  } @else if $breakpoint == "md" {
    @media (min-width: $breakpoint-md) { @content; }
  } @else if $breakpoint == "lg" {
    @media (min-width: $breakpoint-lg) { @content; }
  } @else if $breakpoint == "xl" {
    @media (min-width: $breakpoint-xl) { @content; }
  } @else if $breakpoint == "xxl" {
    @media (min-width: $breakpoint-xxl) { @content; }
  }
}

// --- Flexbox ---
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

@mixin flex-between {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

@mixin flex-column-center {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

// --- Grid ---
@mixin auto-grid($min-width: 250px, $gap: $spacing-md) {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax($min-width, 1fr));
  gap: $gap;
}

@mixin grid($columns, $gap: $spacing-md) {
  display: grid;
  grid-template-columns: repeat($columns, 1fr);
  gap: $gap;
}

// --- Texte ---
@mixin truncate($lines: 1) {
  @if $lines == 1 {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  } @else {
    display: -webkit-box;
    -webkit-line-clamp: $lines;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
}

// --- Position ---
@mixin absolute-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

@mixin fixed-center {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

// --- Transitions ---
@mixin transition($properties...) {
  $result: ();
  @each $prop in $properties {
    $result: append($result, $prop $transition-speed $transition-easing, comma);
  }
  transition: $result;
}

@mixin transition-fast($properties...) {
  $result: ();
  @each $prop in $properties {
    $result: append($result, $prop $transition-speed-fast $transition-easing, comma);
  }
  transition: $result;
}

// --- Accessibilité ---
@mixin visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

// --- Clearfix ---
@mixin clearfix {
  &::after {
    content: "";
    display: table;
    clear: both;
  }
}

// --- Appear animation ---
@mixin fade-in($duration: 0.3s) {
  animation: fadeIn $duration ease-in;
}

@mixin slide-up($duration: 0.3s) {
  animation: slideUp $duration ease-out;
}

// --- Shadow avec couleur ---
@mixin colored-shadow($color, $blur: 10px, $spread: 0px, $opacity: 0.25) {
  box-shadow: 0 $spread $blur rgba($color, $opacity);
}

// --- Aspect ratio ---
@mixin aspect-ratio($width: 16, $height: 9) {
  aspect-ratio: #{$width} / #{$height};
}
```

### abstracts/_functions.scss

```scss
// ============================================
// FONCTIONS UTILITAIRES
// ============================================

@use 'colors' as *;
@use 'variables' as *;

// --- Conversions ---
@function to-rem($px) {
  @return ($px / 16px) * 1rem;
}

@function to-em($px, $context: 16px) {
  @return ($px / $context) * 1em;
}

// --- Couleurs ---
@function color($key) {
  @if not map-has-key($colors, $key) {
    @warn "La couleur '#{$key}' n'existe pas dans $colors.";
    @return null;
  }
  @return map-get($colors, $key);
}

@function gray($level) {
  @if not map-has-key($grays, $level) {
    @warn "Le gris '#{$level}' n'existe pas dans $grays.";
    @return null;
  }
  @return map-get($grays, $level);
}

// --- Espacement ---
@function spacing($multiplier) {
  @return $spacing-unit * $multiplier;
}

// --- Responsive ---
@function breakpoint($name) {
  $breakpoints: (
    "sm": $breakpoint-sm,
    "md": $breakpoint-md,
    "lg": $breakpoint-lg,
    "xl": $breakpoint-xl,
    "xxl": $breakpoint-xxl
  );
  @return map-get($breakpoints, $name);
}
```

### abstracts/_index.scss

```scss
// ============================================
// INDEX DES ABSTRACTS
// Fichier barrel — ré-exporte tous les modules
// ============================================

@forward 'colors';
@forward 'variables';
@forward 'mixins' as *;
@forward 'functions' as *;
```

---

## 11.3 Créer les fichiers de base

### base/_reset.scss

```scss
// ============================================
// RESET CSS MODERNISE
// ============================================

@use '../abstracts/variables' as *;

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: $font-size-base;
  scroll-behavior: smooth;
  -webkit-text-size-adjust: 100%;
}

body {
  font-family: $font-family-base;
  font-size: 1rem;
  line-height: $line-height-base;
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
  color: inherit;
}

a {
  color: inherit;
  text-decoration: none;
}

ul,
ol {
  list-style: none;
}

h1, h2, h3, h4, h5, h6 {
  font-size: inherit;
  font-weight: inherit;
}

table {
  border-collapse: collapse;
  border-spacing: 0;
}
```

### base/_typography.scss

```scss
// ============================================
// STYLES DE TYPOGRAPHIE
// ============================================

@use '../abstracts/variables' as *;
@use '../abstracts/colors' as *;
@use '../abstracts/mixins' as *;
@use '../abstracts/functions' as *;

body {
  font-size: $font-size-base;
  line-height: $line-height-base;
}

h1 {
  font-size: to-rem(36px);
  font-weight: $font-weight-bold;
  line-height: 1.2;
  margin-bottom: $spacing-md;

  @include respond-to('md') {
    font-size: to-rem(48px);
  }
}

h2 {
  font-size: to-rem(28px);
  font-weight: $font-weight-bold;
  line-height: 1.3;
  margin-bottom: $spacing-md;

  @include respond-to('md') {
    font-size: to-rem(36px);
  }
}

h3 {
  font-size: to-rem(22px);
  font-weight: $font-weight-semibold;
  line-height: 1.4;
  margin-bottom: $spacing-sm;

  @include respond-to('md') {
    font-size: to-rem(28px);
  }
}

h4 {
  font-size: to-rem(18px);
  font-weight: $font-weight-semibold;
  line-height: 1.4;
  margin-bottom: $spacing-sm;
}

h5 {
  font-size: $font-size-base;
  font-weight: $font-weight-medium;
  line-height: 1.5;
  margin-bottom: $spacing-sm;
}

h6 {
  font-size: $font-size-sm;
  font-weight: $font-weight-medium;
  line-height: 1.5;
  margin-bottom: $spacing-xs;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

p {
  margin-bottom: $spacing-md;
}

small {
  font-size: $font-size-sm;
}

code,
pre {
  font-family: $font-family-mono;
}

blockquote {
  padding-left: $spacing-lg;
  border-left: 4px solid $color-primary;
  font-style: italic;
  color: $color-text-secondary;
  margin: $spacing-lg 0;
}

strong {
  font-weight: $font-weight-bold;
}
```

### base/_animations.scss

```scss
// ============================================
// ANIMATIONS DE BASE
// ============================================

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes fadeOut {
  from { opacity: 1; }
  to { opacity: 0; }
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideInLeft {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes slideInRight {
  from {
    opacity: 0;
    transform: translateX(20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes scaleIn {
  from {
    opacity: 0;
    transform: scale(0.9);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateX(-4px); }
  20%, 40%, 60%, 80% { transform: translateX(4px); }
}
```

---

## 11.4 Créer les fichiers de layout

### layout/_grid.scss

```scss
// ============================================
// SYSTEME DE GRILLE
// ============================================

@use '../abstracts/variables' as *;
@use '../abstracts/mixins' as *;

$grid-columns: 12;
$grid-gutter: $spacing-md;

@mixin grid-container {
  width: 100%;
  max-width: $container-max-width;
  margin-left: auto;
  margin-right: auto;
  padding-left: $grid-gutter;
  padding-right: $grid-gutter;
}

@mixin grid-row {
  display: flex;
  flex-wrap: wrap;
  margin-left: -$grid-gutter / 2;
  margin-right: -$grid-gutter / 2;
}

@mixin grid-col($columns) {
  flex: 0 0 percentage($columns / $grid-columns);
  max-width: percentage($columns / $grid-columns);
  padding-left: $grid-gutter / 2;
  padding-right: $grid-gutter / 2;
}

@mixin grid-col-auto {
  flex: 0 0 auto;
  width: auto;
}

@mixin grid-col-fill {
  flex: 1 1 0%;
}

// --- Utilitaires de grille ---
.container {
  @include grid-container;
}

.row {
  @include grid-row;
}

@for $i from 1 through $grid-columns {
  .col-#{$i} {
    @include grid-col($i);
  }
}

@include respond-to('md') {
  @for $i from 1 through $grid-columns {
    .col-md-#{$i} {
      @include grid-col($i);
    }
  }
}

@include respond-to('lg') {
  @for $i from 1 through $grid-columns {
    .col-lg-#{$i} {
      @include grid-col($i);
    }
  }
}

@include respond-to('xl') {
  @for $i from 1 through $grid-columns {
    .col-xl-#{$i} {
      @include grid-col($i);
    }
  }
}
```

### layout/_header.scss

```scss
// ============================================
// HEADER / BARRE DE NAVIGATION
// ============================================

@use '../abstracts/variables' as *;
@use '../abstracts/colors' as *;
@use '../abstracts/mixins' as *;

.header {
  position: sticky;
  top: 0;
  z-index: $z-sticky;
  height: $header-height;
  background-color: $color-white;
  box-shadow: $shadow-sm;

  &__container {
    @include flex-between;
    @include grid-container;
    height: 100%;
  }

  &__logo {
    display: flex;
    align-items: center;
    gap: $spacing-sm;
    font-size: 1.25rem;
    font-weight: $font-weight-bold;
    color: $color-primary;

    img {
      height: 32px;
      width: auto;
    }
  }

  &__nav {
    @include flex-center;
    gap: $spacing-lg;

    @include respond-to('md') {
      gap: $spacing-xl;
    }
  }

  &__link {
    position: relative;
    font-weight: $font-weight-medium;
    color: $color-text-primary;

    &:hover,
    &--active {
      color: $color-primary;
    }

    &--active::after {
      content: "";
      position: absolute;
      bottom: -4px;
      left: 0;
      width: 100%;
      height: 2px;
      background-color: $color-primary;
    }
  }

  &__actions {
    @include flex-center;
    gap: $spacing-sm;
  }

  &__menu-toggle {
    display: block;
    background: none;
    border: none;
    cursor: pointer;
    padding: $spacing-sm;

    @include respond-to('md') {
      display: none;
    }
  }
}
```

### layout/_footer.scss

```scss
// ============================================
// FOOTER
// ============================================

@use '../abstracts/variables' as *;
@use '../abstracts/colors' as *;
@use '../abstracts/mixins' as *;

.footer {
  background-color: $color-gray-900;
  color: $color-white;
  padding: $spacing-xxl 0 $spacing-lg;

  &__container {
    @include grid-container;
  }

  &__grid {
    @include auto-grid(250px, $spacing-xl);
    margin-bottom: $spacing-xl;
  }

  &__title {
    font-size: 1.125rem;
    font-weight: $font-weight-semibold;
    margin-bottom: $spacing-md;
  }

  &__text {
    color: $color-gray-400;
    line-height: 1.8;
  }

  &__link {
    display: block;
    color: $color-gray-400;
    padding: $spacing-xs 0;
    transition: color $transition-speed $transition-easing;

    &:hover {
      color: $color-white;
    }
  }

  &__bottom {
    border-top: 1px solid $color-gray-700;
    padding-top: $spacing-lg;
    text-align: center;
    color: $color-gray-500;
    font-size: $font-size-sm;
  }
}
```

---

## 11.5 Créer les fichiers de composants

### components/_buttons.scss

```scss
// ============================================
// COMPOSANT BOUTONS
// ============================================

@use '../abstracts/variables' as *;
@use '../abstracts/colors' as *;
@use '../abstracts/mixins' as *;

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: $spacing-sm;
  padding: $spacing-sm $spacing-lg;
  border: 2px solid transparent;
  border-radius: $border-radius;
  font-size: 1rem;
  font-weight: $font-weight-medium;
  line-height: 1.5;
  cursor: pointer;
  user-select: none;
  text-decoration: none;
  transition: background-color $transition-speed $transition-easing,
              color $transition-speed $transition-easing,
              border-color $transition-speed $transition-easing,
              box-shadow $transition-speed $transition-easing;

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    pointer-events: none;
  }

  &:focus-visible {
    outline: 2px solid $color-primary;
    outline-offset: 2px;
  }

  // --- Variantes ---
  &--primary {
    background-color: $color-primary;
    color: $color-white;

    &:hover {
      background-color: $color-primary-dark;
    }
  }

  &--secondary {
    background-color: $color-secondary;
    color: $color-white;

    &:hover {
      background-color: $color-secondary-dark;
    }
  }

  &--danger {
    background-color: $color-danger;
    color: $color-white;

    &:hover {
      background-color: darken($color-danger, 10%);
    }
  }

  &--outline {
    background-color: transparent;
    border-color: $color-primary;
    color: $color-primary;

    &:hover {
      background-color: $color-primary;
      color: $color-white;
    }
  }

  &--ghost {
    background-color: transparent;
    color: $color-primary;

    &:hover {
      background-color: rgba($color-primary, 0.1);
    }
  }

  // --- Tailles ---
  &--sm {
    padding: $spacing-xs $spacing-md;
    font-size: $font-size-sm;
  }

  &--lg {
    padding: $spacing-md $spacing-xl;
    font-size: 1.125rem;
  }

  &--block {
    width: 100%;
  }

  &--pill {
    border-radius: $border-radius-pill;
  }
}
```

### components/_cards.scss

```scss
// ============================================
// COMPOSANT CARTES
// ============================================

@use '../abstracts/variables' as *;
@use '../abstracts/colors' as *;
@use '../abstracts/mixins' as *;

.card {
  background-color: $color-white;
  border-radius: $border-radius-lg;
  box-shadow: $shadow-sm;
  overflow: hidden;
  transition: box-shadow $transition-speed $transition-easing;

  &:hover {
    box-shadow: $shadow-lg;
  }

  &__image {
    width: 100%;
    height: 200px;
    object-fit: cover;
  }

  &__body {
    padding: $spacing-lg;
  }

  &__title {
    font-size: 1.25rem;
    font-weight: $font-weight-semibold;
    color: $color-text-primary;
    margin-bottom: $spacing-sm;
  }

  &__subtitle {
    font-size: $font-size-sm;
    color: $color-text-secondary;
    margin-bottom: $spacing-md;
  }

  &__text {
    color: $color-text-secondary;
    margin-bottom: $spacing-md;
    @include truncate(3);
  }

  &__footer {
    padding: $spacing-md $spacing-lg;
    border-top: 1px solid $color-gray-200;
    @include flex-between;
  }

  &--outlined {
    box-shadow: none;
    border: 1px solid $color-gray-300;

    &:hover {
      border-color: $color-primary;
      box-shadow: none;
    }
  }

  &--flat {
    box-shadow: none;
    background-color: $color-gray-100;

    &:hover {
      background-color: $color-gray-200;
    }
  }

  &--horizontal {
    @include respond-to('md') {
      display: flex;

      .card__image {
        width: 300px;
        height: auto;
        flex-shrink: 0;
      }
    }
  }

  &--interactive {
    cursor: pointer;

    &:hover {
      transform: translateY(-2px);
      box-shadow: $shadow-lg;
    }

    &:active {
      transform: translateY(0);
    }
  }
}
```

### components/_alerts.scss

```scss
// ============================================
// COMPOSANT ALERTES
// ============================================

@use '../abstracts/variables' as *;
@use '../abstracts/colors' as *;
@use '../abstracts/mixins' as *;

.alert {
  display: flex;
  align-items: flex-start;
  gap: $spacing-md;
  padding: $spacing-md $spacing-lg;
  border-radius: $border-radius;
  border: 1px solid transparent;

  &__icon {
    flex-shrink: 0;
    font-size: 1.25rem;
    line-height: 1.5;
  }

  &__content {
    flex: 1;
  }

  &__title {
    font-weight: $font-weight-semibold;
    margin-bottom: $spacing-xs;
  }

  &__text {
    line-height: 1.5;
  }

  &__close {
    flex-shrink: 0;
    background: none;
    border: none;
    cursor: pointer;
    opacity: 0.7;
    font-size: 1.25rem;

    &:hover {
      opacity: 1;
    }
  }

  &--success {
    background-color: rgba($color-success, 0.1);
    border-color: $color-success;
    color: darken($color-success, 20%);
  }

  &--warning {
    background-color: rgba($color-warning, 0.1);
    border-color: $color-warning;
    color: darken($color-warning, 30%);
  }

  &--danger {
    background-color: rgba($color-danger, 0.1);
    border-color: $color-danger;
    color: darken($color-danger, 20%);
  }

  &--info {
    background-color: rgba($color-info, 0.1);
    border-color: $color-info;
    color: darken($color-info, 20%);
  }
}
```

---

## 11.6 Créer les fichiers de pages

### pages/_home.scss

```scss
// ============================================
// PAGE D'ACCUEIL
// ============================================

@use '../abstracts/variables' as *;
@use '../abstracts/colors' as *;
@use '../abstracts/mixins' as *;
@use '../abstracts/functions' as *;

.hero {
  @include flex-column-center;
  min-height: 80vh;
  padding: $spacing-xxl $spacing-md;
  background: linear-gradient(135deg, $color-primary, $color-primary-dark);
  color: $color-white;
  text-align: center;

  &__title {
    font-size: to-rem(48px);
    font-weight: $font-weight-bold;
    margin-bottom: $spacing-md;
    animation: slideUp 0.5s ease-out;

    @include respond-to('md') {
      font-size: to-rem(64px);
    }
  }

  &__subtitle {
    font-size: to-rem(20px);
    max-width: 600px;
    margin-bottom: $spacing-xl;
    opacity: 0.9;
    animation: slideUp 0.5s ease-out 0.1s both;
  }

  &__actions {
    display: flex;
    gap: $spacing-md;
    animation: fadeIn 0.5s ease-in 0.3s both;
  }
}

.features {
  padding: $spacing-xxl $spacing-md;

  &__title {
    text-align: center;
    margin-bottom: $spacing-xl;
  }

  &__grid {
    @include auto-grid(300px, $spacing-lg);
    max-width: $container-max-width;
    margin: 0 auto;
  }
}

.feature-card {
  padding: $spacing-xl;
  text-align: center;
  border-radius: $border-radius-lg;
  background-color: $color-white;
  box-shadow: $shadow-sm;
  transition: transform $transition-speed $transition-easing,
              box-shadow $transition-speed $transition-easing;

  &:hover {
    transform: translateY(-4px);
    box-shadow: $shadow-lg;
  }

  &__icon {
    width: 64px;
    height: 64px;
    margin: 0 auto $spacing-md;
    background-color: rgba($color-primary, 0.1);
    border-radius: $border-radius-circle;
    @include flex-center;
    font-size: 1.5rem;
    color: $color-primary;
  }

  &__title {
    font-size: 1.25rem;
    font-weight: $font-weight-semibold;
    margin-bottom: $spacing-sm;
  }

  &__text {
    color: $color-text-secondary;
  }
}
```

---

## 11.7 Créer les fichiers de thèmes

### themes/_light.scss

```scss
// ============================================
// THEME CLAIR
// ============================================

$theme-bg-primary: #ffffff;
$theme-bg-secondary: #f8f9fa;
$theme-bg-tertiary: #e9ecef;
$theme-text-primary: #212529;
$theme-text-secondary: #6c757d;
$theme-text-muted: #adb5bd;
$theme-border-color: #dee2e6;
$theme-shadow-color: rgba(0, 0, 0, 0.1);
```

### themes/_dark.scss

```scss
// ============================================
// THEME SOMBRE
// ============================================

$theme-bg-primary: #1a1a2e;
$theme-bg-secondary: #16213e;
$theme-bg-tertiary: #0f3460;
$theme-text-primary: #e0e0e0;
$theme-text-secondary: #a0a0a0;
$theme-text-muted: #666666;
$theme-border-color: #333355;
$theme-shadow-color: rgba(0, 0, 0, 0.3);
```

---

## 11.8 Créer les fichiers utilitaires

### utilities/_spacing.scss

```scss
// ============================================
// UTILITAIRES D'ESPACEMENT
// ============================================

@use '../abstracts/variables' as *;

$sizes: (
  "0": 0,
  "xs": $spacing-xs,
  "sm": $spacing-sm,
  "md": $spacing-md,
  "lg": $spacing-lg,
  "xl": $spacing-xl,
  "xxl": $spacing-xxl
);

@each $name, $value in $sizes {
  .m-#{$name} { margin: $value; }
  .mt-#{$name} { margin-top: $value; }
  .mr-#{$name} { margin-right: $value; }
  .mb-#{$name} { margin-bottom: $value; }
  .ml-#{$name} { margin-left: $value; }
  .mx-#{$name} { margin-left: $value; margin-right: $value; }
  .my-#{$name} { margin-top: $value; margin-bottom: $value; }

  .p-#{$name} { padding: $value; }
  .pt-#{$name} { padding-top: $value; }
  .pr-#{$name} { padding-right: $value; }
  .pb-#{$name} { padding-bottom: $value; }
  .pl-#{$name} { padding-left: $value; }
  .px-#{$name} { padding-left: $value; padding-right: $value; }
  .py-#{$name} { padding-top: $value; padding-bottom: $value; }
}

.mx-auto { margin-left: auto; margin-right: auto; }
.ml-auto { margin-left: auto; }
.mr-auto { margin-right: auto; }
```

### utilities/_text.scss

```scss
// ============================================
// UTILITAIRES DE TEXTE
// ============================================

@use '../abstracts/variables' as *;
@use '../abstracts/colors' as *;

.text-left { text-align: left; }
.text-center { text-align: center; }
.text-right { text-align: right; }

.text-xs { font-size: $font-size-sm * 0.85; }
.text-sm { font-size: $font-size-sm; }
.text-base { font-size: $font-size-base; }
.text-lg { font-size: $font-size-lg; }
.text-xl { font-size: $font-size-xl; }
.text-2xl { font-size: 1.5rem; }
.text-3xl { font-size: 2rem; }

.font-light { font-weight: $font-weight-light; }
.font-normal { font-weight: $font-weight-normal; }
.font-medium { font-weight: $font-weight-medium; }
.font-semibold { font-weight: $font-weight-semibold; }
.font-bold { font-weight: $font-weight-bold; }

.text-primary { color: $color-primary; }
.text-secondary { color: $color-text-secondary; }
.text-muted { color: $color-text-muted; }
.text-success { color: $color-success; }
.text-warning { color: $color-warning; }
.text-danger { color: $color-danger; }
.text-info { color: $color-info; }
.text-white { color: $color-white; }

.text-uppercase { text-transform: uppercase; }
.text-lowercase { text-transform: lowercase; }
.text-capitalize { text-transform: capitalize; }
.text-italic { font-style: italic; }

.text-truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.text-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.text-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

### utilities/_visibility.scss

```scss
// ============================================
// UTILITAIRES DE VISIBILITE
// ============================================

@use '../abstracts/mixins' as *;

.d-none { display: none; }
.d-block { display: block; }
.d-flex { display: flex; }
.d-grid { display: grid; }
.d-inline { display: inline; }
.d-inline-block { display: inline-block; }
.d-inline-flex { display: inline-flex; }

@include respond-to('md') {
  .d-md-none { display: none; }
  .d-md-block { display: block; }
  .d-md-flex { display: flex; }
}

@include respond-to('lg') {
  .d-lg-none { display: none; }
  .d-lg-block { display: block; }
  .d-lg-flex { display: flex; }
}

.sr-only {
  @include visually-hidden;
}

.overflow-hidden { overflow: hidden; }
.overflow-auto { overflow: auto; }

.position-static { position: static; }
.position-relative { position: relative; }
.position-absolute { position: absolute; }
.position-fixed { position: fixed; }
.position-sticky { position: sticky; }

.opacity-0 { opacity: 0; }
.opacity-50 { opacity: 0.5; }
.opacity-100 { opacity: 1; }

.cursor-pointer { cursor: pointer; }
.cursor-not-allowed { cursor: not-allowed; }
```

---

## 11.9 Creer le fichier main.scss (point d'entree)

```scss
// ============================================
// MAIN.SCSS - POINT D'ENTREE DU PROJET
// ============================================
// Ce fichier est le SEUL a compiler
// Il importe tous les autres fichiers dans l'ordre correct
// ============================================

// 1. ABSTRACTS (pas de CSS genere)
@use 'abstracts/colors';
@use 'abstracts/variables';
@use 'abstracts/mixins' as *;
@use 'abstracts/functions' as *;

// 2. BASE (styles fondamentaux)
@use 'base/reset';
@use 'base/typography';
@use 'base/animations';

// 3. VENDORS (bibliotheques tierces)
@use 'vendors/bootstrap-overrides';

// 4. LAYOUT (structure de la page)
@use 'layout/grid';
@use 'layout/header';
@use 'layout/footer';
@use 'layout/sidebar';

// 5. COMPOSANTS (elements UI)
@use 'components/buttons';
@use 'components/cards';
@use 'components/forms';
@use 'components/alerts';

// 6. PAGES (styles specifiques)
@use 'pages/home';
@use 'pages/about';
@use 'pages/contact';

// 7. THEMES (themes visuels)
@use 'themes/light';
@use 'themes/dark';

// 8. UTILITAIRES (classes helper)
@use 'utilities/spacing';
@use 'utilities/text';
@use 'utilities/visibility';
```

---

## 11.10 Compiler uniquement main.scss

### Commande de compilation

```bash
# Compiler le fichier principal
sass src/scss/main.scss dist/css/main.css

# Avec observation des changements
sass --watch src/scss/main.scss:dist/css/main.css

# Version minifiee
sass --style=compressed src/scss/main.scss dist/css/main.min.css
```

### Pourquoi ne compiler QUE main.scss ?

```
NE JAMAIS faire :
sass src/scss/components/_buttons.scss buttons.css
sass src/scss/layout/_header.scss header.css
sass src/scss/base/_reset.scss reset.css
--> Genere des fichiers CSS multiples et non gerables

TOUJOURS faire :
sass src/scss/main.scss dist/css/main.css
--> Un seul fichier CSS contenant tout
--> Les fichiers partiels ne sont JAMAIS compiles seuls
```

### Configuration avec des outils de build

#### Avec Vite

```javascript
// vite.config.js
import { defineConfig } from 'vite';

export default defineConfig({
  css: {
    preprocessorOptions: {
      scss: {
        // Vite detecte automatiquement main.scss
        // ou vous pouvez specifier :
        // api: 'modern-compiler'
      }
    }
  }
});
```

#### Avec Webpack

```javascript
// webpack.config.js
module.exports = {
  entry: {
    main: './src/scss/main.scss'
  },
  module: {
    rules: [{
      test: /\.scss$/,
      use: [
        MiniCssExtractPlugin.loader,
        'css-loader',
        'sass-loader'
      ]
    }]
  }
};
```

#### Avec package.json scripts

```json
{
  "scripts": {
    "build:css": "sass src/scss/main.scss dist/css/main.css --style=compressed",
    "watch:css": "sass --watch src/scss/main.scss:dist/css/main.css"
  }
}
```

---

## 11.11 Pourquoi cette structure fonctionne

### 1. Separation des responsabilites

Chaque dossier a UN role precis. Quand vous devez modifier les boutons, vous savez exactement ou aller : `components/_buttons.scss`.

### 2. Ordre de compilation logique

L'ordre des imports dans `main.scss` suit une logique :

1. **Abstracts** : Les outils (pas de CSS)
2. **Base** : Les fondations (reset, typographie)
3. **Vendors** : Les overrides (apres base pour overrider)
4. **Layout** : La structure (grid, header, footer)
5. **Components** : Les elements UI (boutons, cartes)
6. **Pages** : Les styles specifiques
7. **Themes** : Les themes visuels
8. **Utilities** : Les classes helper (dernier pour priorite)

### 3. Pas de CSS genere dans les abstracts

Les fichiers dans `abstracts/` ne generent jamais de CSS. Ils contiennent uniquement des variables, mixins et fonctions. C'est pourquoi on les importe avec `@use ... as *` pour les utiliser directement.

### 4. Un seul point d'entree

`main.scss` est le fichier unique a compiler. Cela signifie :

- Un seul fichier CSS en sortie
- Pas de CSS duplique
- Compilation optimisee
- Gestion simple des dependances

### 5. Scalabilite

Pour ajouter un nouveau composant :

1. Creer `components/_dropdown.scss`
2. Ajouter `@use 'components/dropdown';` dans `main.scss`
3. C'est tout !

### 6. Collaboration facile

```
Developpeur A -> travaille sur components/_forms.scss
Developpeur B -> travaille on layout/_sidebar.scss
Developpeur C -> travaille on pages/_blog.scss
--> Aucun conflit possible !
```

---

## 11.12 Conventions de nommage

### Fichiers

```
_colors.scss          --> kebab-case avec underscore
_typography.scss
_bootstrap-overrides.scss
```

### Variables

```scss
$color-primary: #3498db;     --> prefixe du type
$spacing-md: 16px;           --> nom descriptif
$border-radius-lg: 10px;     --> taille optionnelle
$z-modal: 1050;              --> prefixe z- pour z-index
$font-size-base: 16px;       --> prefixe du type
```

### Mixins

```scss
@mixin flex-center { }       --> verbe ou description
@mixin respond-to($bp) { }   --> nom descriptif
@mixin truncate($lines) { }  --> action
@mixin button-variant($bg) { } --> composant + action
```

### Classes CSS (BEM)

```scss
.block { }                   --> BEM block
.block__element { }          --> BEM element
.block--modifier { }         --> BEM modifier

// Exemples :
.card { }
.card__title { }
.card__text { }
.card--outlined { }
.card--interactive { }

.btn { }
.btn--primary { }
.btn--sm { }
.btn--lg { }
```

---

## 11.13 Exercices

### Exercice 1 : Structure complete

Creez la structure de dossiers et tous les fichiers.listes dans ce chapitre. Verifiez que chaque dossier contient au moins 2 fichiers partiels et un `_index.scss`.

### Exercice 2 : Ajouter un composant

Ajoutez un composant `components/_modal.scss` avec :

1. Un mixin `modal-backdrop()` pour l'overlay
2. Un mixin `modal-content()` pour la boite de dialogue
3. Les variantes : `--small`, `--large`, `--fullscreen`
4. Les animations : fade-in, scale-in
5. Le fichier dans `_index.scss` du dossier components

### Exercice 3 : Ajouter un theme

Creez un theme `_high-contrast.scss` pour l'accessibilite :

1. Couleurs a tres fort contraste (noir sur blanc)
2. Borders epaisses et visibles
3. Focus outlines tres visibles
4. Police plus grande par defaut

### Exercice 4 : Creer un dossier `print/`

Creez un dossier `print/` avec :

1. `_print.scss` pour les styles d'impression
2. Des media queries `@media print`
3. La cache de elements non necessaires (nav, footer, sidebar)
4. L'ajout dans `main.scss`

### Exercice 5 : Optimiser la compilation

1. Configurez Sass pour generer une version minifiee
2. Configurez les sourcemaps
3. Ajoutez un script npm pour builder et watcher
4. Testez que seul `main.scss` est compile

### Exercice 6 : Audit de structure

En vous basant sur la structure de ce chapitre, analysez un de vos projets existants et repondez :

1. Combien de fichiers CSS avez-vous ?
2. Combien de variables sont definies globalement ?
3. Pouvons-nous identifier ou se trouve chaque style ?
4. Quels fichiers pourraient etre des partiels ?

---

## Resume

| Concept | Description |
|---------|-------------|
| `abstracts/` | Variables, mixins, fonctions, tokens (pas de CSS) |
| `base/` | Reset, typographie, animations (fondations) |
| `layout/` | Grid, header, footer, sidebar (structure) |
| `components/` | Boutons, cartes, formulaires (elements UI) |
| `pages/` | Styles specifiques a chaque page |
| `themes/` | Themes clair, sombre, contraste |
| `vendors/` | Overrides de bibliotheques externes |
| `utilities/` | Classes helper (spacing, text, visibility) |
| `main.scss` | Point d'entree unique, seul fichier a compiler |
| `_index.scss` | Barrel file qui forward les modules d'un dossier |

### Regles d'or

```
1. Toujours compiler main.scss, jamais les partiels
2. Abstracts en premier, utilities en dernier
3. Chaque dossier a un _index.scss (barrel file)
4. Utiliser BEM pour les noms de classes
5. Prefixer les variables ($color-, $spacing-, $font-)
6. Un role par dossier, un dossier par role
7. Ne jamais generer de CSS dans abstracts/
```
