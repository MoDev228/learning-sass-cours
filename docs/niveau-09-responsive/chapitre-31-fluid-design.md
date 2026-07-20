# Chapitre 31 — Fluid Design

## Introduction

Le **fluid design** permet de créer des interfaces qui s'adaptent en douceur à toutes les tailles d'écran, sans rupture visible entre les breakpoints. Contrairement au responsive classique (qui saute d'un breakpoint à un autre), le fluide interpolé les valeurs de manière continue.

Les fonctions CSS `clamp()`, `min()`, `max()` et `calc()` sont les piliers de cette approche, et SCSS permet de créer des mixins puissants pour les réutiliser.

---

## Les fonctions CSS fondamentales

### clamp()

```css
/* clamp(min, preferred, max) */
/* La valeur est bornée entre min et max, mais suit preferred quand possible */

.font-size {
  /* Entre 16px et 48px, la taille suit 4vw */
  font-size: clamp(16px, 4vw, 48px);
}
```

### min()

```css
/* min(val1, val2, ...) */
/* Retourne la plus petite valeur */

/* Ne jamais dépasser 800px, sinon 100% */
.container {
  max-width: min(100% - 40px, 800px);
}

/* Hauteur minimale qui ne dépasse pas la viewport */
.hero {
  min-height: min(100vh - 80px, 600px);
}
```

### max()

```css
/* max(val1, val2, ...) */
/* Retourne la plus grande valeur */

/* Au moins 200px, sinon 100% */
.sidebar {
  width: max(250px, 20%);
}

/* Padding qui ne descend jamais en dessous de 1rem */
.section {
  padding: max(2rem, 5vw);
}
```

### calc()

```css
/* calc() permet les opérations mathématiques */

/* 100% moins 2 conteneurs de 300px */
.grid {
  width: calc(100% - 600px);
}

/* Espacement basé sur la taille de police */
.element {
  padding: calc(var(--font-size) * 1.5);
}
```

---

## Fluid Typography (Typographie fluide)

### Principe

```scss
// Au lieu de :
h1 { font-size: 24px; }
@media (min-width: 768px) { h1 { font-size: 36px; } }
@media (min-width: 1200px) { h1 { font-size: 48px; } }

// On écrit :
h1 {
  font-size: clamp(24px, 4vw, 48px);
}
```

### Mixin fluid-type

```scss
// mixins/_fluid.scss

@mixin fluid-type($min-size, $max-size, $min-vw: 375px, $max-vw: 1440px) {
  $slope: ($max-size - $min-size) / ($max-vw - $min-vw);
  $intercept: $min-size - $slope * $min-vw;

  font-size: clamp(
    #{$min-size}px,
    calc(#{$intercept / 16}rem + #{$slope * 100} * 1vw),
    #{$max-size}px
  );
}

// Utilisation
h1 {
  @include fluid-type(28, 56);
  font-weight: 700;
  line-height: 1.1;
}

h2 {
  @include fluid-type(24, 40);
  font-weight: 700;
  line-height: 1.2;
}

h3 {
  @include fluid-type(20, 30);
  font-weight: 600;
  line-height: 1.3;
}

p {
  @include fluid-type(14, 18);
  line-height: 1.6;
}
```

### Résultat CSS

```css
h1 {
  font-size: clamp(28px, 1.5161rem + 2.2581vw, 56px);
  font-weight: 700;
  line-height: 1.1;
}

h2 {
  font-size: clamp(24px, 1.1935rem + 1.5484vw, 40px);
  font-weight: 700;
  line-height: 1.2;
}

h3 {
  font-size: clamp(20px, 1.0968rem + 0.9677vw, 30px);
  font-weight: 600;
  line-height: 1.3;
}
```

### Mixin avancé avec line-height fluide

```scss
@mixin fluid-type-lh($min-size, $max-size, $min-lh, $max-lh, $min-vw: 375px, $max-vw: 1440px) {
  $size-slope: ($max-size - $min-size) / ($max-vw - $min-vw);
  $size-intercept: $min-size - $size-slope * $min-vw;

  $lh-slope: ($max-lh - $min-lh) / ($max-vw - $min-vw);
  $lh-intercept: $min-lh - $lh-slope * $min-vw;

  font-size: clamp(
    #{$min-size}px,
    calc(#{$size-intercept / 16}rem + #{$size-slope * 100} * 1vw),
    #{$max-size}px
  );

  line-height: clamp(
    #{$min-lh},
    calc(#{$lh-intercept / 16}rem + #{$lh-slope * 100} * 1vw),
    #{$max-lh}
  );
}

.hero__title {
  @include fluid-type-lh(32, 72, 1.1, 1.05);
}
```

---

## Fluid Spacing (Espacement fluide)

### Mixin de base

```scss
@mixin fluid-spacing($property, $min, $max, $min-vw: 375px, $max-vw: 1440px) {
  $slope: ($max - $min) / ($max-vw - $min-vw);
  $intercept: $min - $slope * $min-vw;

  #{$property}: clamp(
    #{$min}px,
    calc(#{$intercept / 16}rem + #{$slope * 100} * 1vw),
    #{$max}px
  );
}

// Utilisation
.section {
  @include fluid-spacing(padding-top, 40, 120);
  @include fluid-spacing(padding-bottom, 40, 120);
}

.card {
  @include fluid-spacing(padding, 16, 32);
}

.hero {
  @include fluid-spacing(margin-bottom, 20, 60);
}
```

### Mixin pour toutes les propriétés d'espacement

```scss
@mixin fluid-space($scale: (
  'xs': (4, 8),
  'sm': (8, 16),
  'md': (16, 32),
  'lg': (24, 48),
  'xl': (32, 64),
  '2xl': (48, 96),
  '3xl': (64, 128),
)) {
  @each $name, $values in $scale {
    $min: nth($values, 1);
    $max: nth($values, 2);

    .space-#{$name} {
      @include fluid-spacing(margin, $min, $max);
    }

    .space-#{$name}-t { @include fluid-spacing(margin-top, $min, $max); }
    .space-#{$name}-r { @include fluid-spacing(margin-right, $min, $max); }
    .space-#{$name}-b { @include fluid-spacing(margin-bottom, $min, $max); }
    .space-#{$name}-l { @include fluid-spacing(margin-left, $min, $max); }

    .space-#{$name}-x {
      @include fluid-spacing(margin-left, $min, $max);
      @include fluid-spacing(margin-right, $min, $max);
    }

    .space-#{$name}-y {
      @include fluid-spacing(margin-top, $min, $max);
      @include fluid-spacing(margin-bottom, $min, $max);
    }
  }
}

@include fluid-space();
```

---

## Fluid Layout

### Container fluide

```scss
@mixin fluid-container($max-width: 1200px, $padding-min: 16, $padding-max: 40) {
  width: 100%;
  max-width: $max-width;
  margin-left: auto;
  margin-right: auto;
  padding-left: clamp(#{$padding-min}px, calc(2.5vw - 2px), #{$padding-max}px);
  padding-right: clamp(#{$padding-min}px, calc(2.5vw - 2px), #{$padding-max}px);
}

.container {
  @include fluid-container;
}
```

### Grid fluide

```scss
@mixin fluid-grid($columns, $gap-min: 16, $gap-max: 32) {
  display: grid;
  gap: clamp(#{$gap-min}px, calc(2vw - 1px), #{$gap-max}px);
  grid-template-columns: repeat($columns, 1fr);
}

@mixin fluid-grid-auto($min-col-width: 280px, $gap-min: 16, $gap-max: 32) {
  display: grid;
  gap: clamp(#{$gap-min}px, calc(2vw - 1px), #{$gap-max}px);
  grid-template-columns: repeat(auto-fill, minmax($min-col-width, 1fr));
}

// Utilisation
.features {
  @include fluid-grid-auto(300px);
}

.gallery {
  @include fluid-grid(3);
}

.dashboard {
  @include fluid-grid(4, 12, 24);
}
```

### Layout sidebar fluide

```scss
@mixin fluid-sidebar-layout($sidebar-min: 250, $sidebar-max: 300, $gap-min: 16, $gap-max: 32) {
  display: grid;
  gap: clamp(#{$gap-min}px, calc(2vw - 1px), #{$gap-max}px);
  grid-template-columns: clamp(#{$sidebar-min}px, 20vw, #{$sidebar-max}px) 1fr;
}

.layout {
  @include fluid-sidebar-layout;

  @media (max-width: 768px) {
    grid-template-columns: 1fr;
  }
}
```

---

## Fluid Images & Media

```scss
@mixin fluid-width($min-width: 100%, $max-width: 1200px) {
  width: $min-width;

  @supports (width: min($min-width, $max-width)) {
    width: min($min-width, $max-width);
  }
}

@mixin fluid-aspect-ratio($min-ratio, $max-ratio) {
  aspect-ratio: $min-ratio;

  @media (min-width: 768px) {
    aspect-ratio: $max-ratio;
  }
}

.image-wrapper {
  @include fluid-width(100%, 800px);
  margin-left: auto;
  margin-right: auto;
}

.hero-image {
  width: 100%;
  max-width: min(100%, 1200px);
  margin: 0 auto;
  aspect-ratio: 16/9;
  object-fit: cover;
}
```

---

## Fluid Colors & Opacité

```scss
@mixin fluid-color($property, $color-min, $color-max) {
  #{$property}: $color-min;

  @media (prefers-color-scheme: dark) {
    #{$property}: $color-max;
  }
}

// ou avec des stops
@mixin fluid-bg($bg-light, $bg-dark) {
  background-color: $bg-light;

  @media (prefers-color-scheme: dark) {
    background-color: $bg-dark;
  }
}

.card {
  @include fluid-bg(white, #1a1a2e);
  color: $color-text;
}
```

---

## Système complet de Fluid Design

### Fichier de configuration

```scss
// config/_fluid.scss

$fluid-min-vw: 375px !default;
$fluid-max-vw: 1440px !default;

// Échelle de tailles
$fluid-type-scale: (
  'xs': (12, 14),
  'sm': (14, 16),
  'base': (16, 18),
  'lg': (18, 22),
  'xl': (20, 28),
  '2xl': (24, 36),
  '3xl': (30, 48),
  '4xl': (36, 60),
  '5xl': (48, 80),
) !default;

// Échelle d'espacement
$fluid-spacing-scale: (
  '1': (4, 8),
  '2': (8, 16),
  '3': (16, 24),
  '4': (24, 32),
  '5': (32, 48),
  '6': (48, 64),
  '7': (64, 80),
  '8': (80, 96),
  '9': (96, 128),
  '10': (128, 160),
) !default;

// Échelle de gap
$fluid-gap-scale: (
  'sm': (8, 12),
  'md': (16, 24),
  'lg': (24, 32),
  'xl': (32, 48),
) !default;
```

### Mixins principaux

```scss
// mixins/_fluid-system.scss

@use '../config/fluid' as *;

// ---- TYPE ----
@mixin fluid-type($min, $max, $min-vw: $fluid-min-vw, $max-vw: $fluid-max-vw) {
  $slope: ($max - $min) / ($max-vw - $min-vw);
  $intercept: $min - $slope * $min-vw;

  font-size: clamp(
    #{$min}px,
    calc(#{$intercept / 16}rem + #{$slope * 100} * 1vw),
    #{$max}px
  );
}

// ---- SPACING ----
@mixin fluid-spacing($property, $min, $max, $min-vw: $fluid-min-vw, $max-vw: $fluid-max-vw) {
  $slope: ($max - $min) / ($max-vw - $min-vw);
  $intercept: $min - $slope * $min-vw;

  #{$property}: clamp(
    #{$min}px,
    calc(#{$intercept / 16}rem + #{$slope * 100} * 1vw),
    #{$max}px
  );
}

// ---- GAP ----
@mixin fluid-gap($min, $max, $min-vw: $fluid-min-vw, $max-vw: $fluid-max-vw) {
  $slope: ($max - $min) / ($max-vw - $min-vw);
  $intercept: $min - $slope * $min-vw;

  gap: clamp(
    #{$min}px,
    calc(#{$intercept / 16}rem + #{$slope * 100} * 1vw),
    #{$max}px
  );
}

// ---- WIDTH ----
@mixin fluid-width($min, $max, $min-vw: $fluid-min-vw, $max-vw: $fluid-max-vw) {
  $slope: ($max - $min) / ($max-vw - $min-vw);
  $intercept: $min - $slope * $min-vw;

  width: clamp(
    #{$min}px,
    calc(#{$intercept / 16}rem + #{$slope * 100} * 1vw),
    #{$max}px
  );
}

// ---- BORDER RADIUS ----
@mixin fluid-radius($min, $max, $min-vw: $fluid-min-vw, $max-vw: $fluid-max-vw) {
  $slope: ($max - $min) / ($max-vw - $min-vw);
  $intercept: $min - $slope * $min-vw;

  border-radius: clamp(
    #{$min}px,
    calc(#{$intercept / 16}rem + #{$slope * 100} * 1vw),
    #{$max}px
  );
}

// ---- GÉNÉRATION AUTOMATIQUE ----
@mixin generate-fluid-type-classes {
  @each $name, $sizes in $fluid-type-scale {
    $min: nth($sizes, 1);
    $max: nth($sizes, 2);

    .text-#{$name} {
      @include fluid-type($min, $max);
    }
  }
}

@mixin generate-fluid-spacing-classes {
  @each $name, $sizes in $fluid-spacing-scale {
    $min: nth($sizes, 1);
    $max: nth($sizes, 2);

    .space-#{$name} { @include fluid-spacing(margin, $min, $max); }
    .space-#{$name}-t { @include fluid-spacing(margin-top, $min, $max); }
    .space-#{$name}-r { @include fluid-spacing(margin-right, $min, $max); }
    .space-#{$name}-b { @include fluid-spacing(margin-bottom, $min, $max); }
    .space-#{$name}-l { @include fluid-spacing(margin-left, $min, $max); }

    .space-p-#{$name} { @include fluid-spacing(padding, $min, $max); }
    .space-pt-#{$name} { @include fluid-spacing(padding-top, $min, $max); }
    .space-pr-#{$name} { @include fluid-spacing(padding-right, $min, $max); }
    .space-pb-#{$name} { @include fluid-spacing(padding-bottom, $min, $max); }
    .space-pl-#{$name} { @include fluid-spacing(padding-left, $min, $max); }
  }
}

@mixin generate-fluid-gap-classes {
  @each $name, $sizes in $fluid-gap-scale {
    $min: nth($sizes, 1);
    $max: nth($sizes, 2);

    .gap-#{$name} {
      @include fluid-gap($min, $max);
    }
  }
}
```

### Fichier d'entrée

```scss
// main.scss

@use 'config/fluid';
@use 'mixins/fluid-system';

// Génération des classes
@include fluid-system.generate-fluid-type-classes;
@include fluid-system.generate-fluid-spacing-classes;
@include fluid-system.generate-fluid-gap-classes;

// Exemples d'utilisation
.hero {
  @include fluid-system.fluid-spacing(padding-top, 60, 160);
  @include fluid-system.fluid-spacing(padding-bottom, 60, 160);
  @include fluid-system.fluid-type(32, 72);
  text-align: center;
}

.section {
  @include fluid-system.fluid-spacing(padding, 40, 80);
}

.card-grid {
  display: grid;
  @include fluid-system.fluid-gap(16, 32);
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
}
```

---

## Résultat CSS

```css
/* Classes générées */
.text-xs { font-size: clamp(12px, 0.6349rem + 0.4762vw, 14px); }
.text-sm { font-size: clamp(14px, 0.8254rem + 0.6349vw, 16px); }
.text-base { font-size: clamp(16px, 1.0159rem + 0.7937vw, 18px); }
.text-lg { font-size: clamp(18px, 1.1008rem + 1.2698vw, 22px); }
.text-xl { font-size: clamp(20px, 1.1857rem + 1.7460vw, 28px); }
.text-2xl { font-size: clamp(24px, 1.3611rem + 2.5397vw, 36px); }
.text-3xl { font-size: clamp(30px, 1.5365rem + 3.3333vw, 48px); }
.text-4xl { font-size: clamp(36px, 1.7119rem + 4.1270vw, 60px); }
.text-5xl { font-size: clamp(48px, 2.0626rem + 5.7143vw, 80px); }

.space-1 { margin: clamp(4px, 0.0847rem + 0.9524vw, 8px); }
.space-2 { margin: clamp(8px, 0.1694rem + 1.9048vw, 16px); }
.space-3 { margin: clamp(16px, 0.3387rem + 3.8095vw, 32px); }
.space-4 { margin: clamp(24px, 0.5081rem + 5.7143vw, 48px); }

.gap-sm { gap: clamp(8px, 0.5556rem + 0.6349vw, 12px); }
.gap-md { gap: clamp(16px, 1.1111rem + 1.2698vw, 24px); }
.gap-lg { gap: clamp(24px, 1.6667rem + 1.9048vw, 32px); }

/* Hero fluide */
.hero {
  padding-top: clamp(60px, 3.0159rem + 12.0635vw, 160px);
  padding-bottom: clamp(60px, 3.0159rem + 12.0635vw, 160px);
  font-size: clamp(32px, 1.7119rem + 4.1270vw, 72px);
  text-align: center;
}
```

---

## Avantages du Fluid Design

| Avantage | Description |
|---|---|
| Pas de breakpoints | L'adaptation est continue, sans sauts |
| Moins de code | Un seul clamp() remplace plusieurs media queries |
| Meilleure UX | Transition douce entre toutes les tailles |
| Performance | Moins de media queries à évaluer |
| Maintenance | Moins de règles à maintenir |
| Print-friendly | S'adapte naturellement à l'impression |

---

## Bonnes pratiques

1. **Définir des min et max raisonnables** : ne jamais laisser clamp() sans limite.
2. **Utiliser des rem dans le preferred** : pour la cohérence avec les settings utilisateur.
3. **Tester sur vrais appareils** : les valeurs fluides peuvent surprendre.
4. **Combiner avec des breakpoints** : certains patterns nécessitent toujours des sauts (layout change).
5. **Préférer le min/max au clamp** quand la contrainte est d'un seul côté.

---

## Résumé

Le fluid design en SCSS permet de créer des interfaces qui s'adaptent en douceur à toutes les tailles. En combinant `clamp()`, `min()`, `max()` et `calc()` dans des mixins réutilisables, on élimine les media queries de taille et on crée une expérience visuellement cohérente sur tous les appareils.
