# Chapitre 29 — Les Media Queries

## Introduction

Les **media queries** sont le fondement du design responsive. Avec SCSS, on peut créer des mixins puissants qui rendent les media queries plus maintenables, plus lisibles et plus flexibles. Ce chapitre couvre la création de systèmes complets de media queries adaptatifs.

---

## Les media queries en CSS natif

Avant SCSS, on écrit des media queries brutes :

```css
/* Desktop first (descendant) */
.container {
  width: 100%;
}

@media (min-width: 768px) {
  .container {
    width: 750px;
  }
}

@media (min-width: 992px) {
  .container {
    width: 970px;
  }
}

@media (min-width: 1200px) {
  .container {
    width: 1170px;
  }
}
```

```css
/* Mobile first (ascendant) */
.container {
  width: 100%;
}

@media (min-width: 576px) {
  .container {
    max-width: 540px;
  }
}

@media (min-width: 768px) {
  .container {
    max-width: 720px;
  }
}

@media (min-width: 992px) {
  .container {
    max-width: 960px;
  }
}

@media (min-width: 1200px) {
  .container {
    max-width: 1140px;
  }
}
```

---

## Breakpoint Maps

### Définir les breakpoints

```scss
// config/_breakpoints.scss

// Map des breakpoints (ordre croissant pour mobile-first)
$breakpoints: (
  'xs': 0,
  'sm': 576px,
  'md': 768px,
  'lg': 992px,
  'xl': 1200px,
  '2xl': 1400px,
) !default;

// Breakpoints spécifiques au contenu
$content-breakpoints: (
  'narrow': 65ch,
  'readable': 75ch,
  'wide': 90ch,
) !default;

// Breakpoints pour les containers
$container-breakpoints: (
  sm: 540px,
  md: 720px,
  lg: 960px,
  xl: 1140px,
  '2xl': 1320px,
) !default;
```

---

## Mixins de base

### Mixin respond-to (mobile-first)

```scss
// mixins/_responsive.scss

@mixin respond-to($breakpoint) {
  $value: map-get($breakpoints, $breakpoint);

  @if $value {
    @media (min-width: $value) {
      @content;
    }
  } @else {
    @warn "Le breakpoint `#{$breakpoint}` n'existe pas dans la map $breakpoints.";
  }
}

// Utilisation
.sidebar {
  width: 100%;

  @include respond-to('md') {
    width: 300px;
  }

  @include respond-to('lg') {
    width: 350px;
  }
}
```

### Mixin respond-below (desktop-first)

```scss
@mixin respond-below($breakpoint) {
  $value: map-get($breakpoints, $breakpoint);

  @if $value {
    @media (max-width: ($value - 0.02px)) {
      @content;
    }
  } @else {
    @warn "Le breakpoint `#{$breakpoint}` n'existe pas.";
  }
}

// Utilisation
.nav__menu {
  display: flex;
  gap: 1rem;

  @include respond-below('md') {
    display: none;
  }
}
```

### Mixin respond-between

```scss
@mixin respond-between($lower, $upper) {
  $min: map-get($breakpoints, $lower);
  $max: map-get($breakpoints, $upper);

  @if $min and $max {
    @media (min-width: $min) and (max-width: ($max - 0.02px)) {
      @content;
    }
  } @else {
    @warn "Breakpoint `#{$lower}` ou `#{$upper}` introuvable.";
  }
}

// Utilisation
.hero__title {
  font-size: 2rem;

  @include respond-between('md', 'xl') {
    font-size: 3rem;
  }
}
```

### Mixin respond-above-only

```scss
@mixin respond-above-only($breakpoint) {
  $value: map-get($breakpoints, $breakpoint);
  $next-key: null;
  $found: false;

  @each $key, $val in $breakpoints {
    @if $found and $next-key == null {
      $next-key: $val;
    }
    @if $key == $breakpoint {
      $found: true;
    }
  }

  @if $value {
    @if $next-key {
      @media (min-width: $value) and (max-width: ($next-key - 0.02px)) {
        @content;
      }
    } @else {
      @media (min-width: $value) {
        @content;
      }
    }
  }
}

// Utilisation : s'applique UNIQUEMENT entre md et lg
.component {
  @include respond-above-only('md') {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
}
```

---

## Mixins de conteneur responsive

### Container standard

```scss
@mixin container($max-width: $container-max-width, $padding: $spacing-md) {
  width: 100%;
  max-width: $max-width;
  margin-left: auto;
  margin-right: auto;
  padding-left: $padding;
  padding-right: $padding;
}

// Container responsive avec breakpoints
@mixin container-responsive {
  @include container(100%, $spacing-md);

  @include respond-to('sm') {
    max-width: 540px;
  }

  @include respond-to('md') {
    max-width: 720px;
  }

  @include respond-to('lg') {
    max-width: 960px;
  }

  @include respond-to('xl') {
    max-width: 1140px;
  }

  @include respond-to('2xl') {
    max-width: 1320px;
  }
}
```

```scss
.container {
  @include container-responsive;
}
```

### Résultat CSS généré

```css
.container {
  width: 100%;
  max-width: 100%;
  margin-left: auto;
  margin-right: auto;
  padding-left: 16px;
  padding-right: 16px;
}

@media (min-width: 576px) {
  .container { max-width: 540px; }
}

@media (min-width: 768px) {
  .container { max-width: 720px; }
}

@media (min-width: 992px) {
  .container { max-width: 960px; }
}

@media (min-width: 1200px) {
  .container { max-width: 1140px; }
}

@media (min-width: 1400px) {
  .container { max-width: 1320px; }
}
```

---

## Système de grilles responsive

### Grille auto-générée

```scss
// mixins/_grid.scss

@mixin grid($columns: 12, $gap: $spacing-md) {
  display: grid;
  gap: $gap;
  grid-template-columns: repeat($columns, 1fr);
}

@mixin grid-auto {
  display: grid;
  gap: $spacing-md;

  @include respond-to('sm') {
    grid-template-columns: repeat(2, 1fr);
  }

  @include respond-to('md') {
    grid-template-columns: repeat(3, 1fr);
  }

  @include respond-to('lg') {
    grid-template-columns: repeat(4, 1fr);
  }
}

@mixin grid-responsive($columns: (
  'xs': 1,
  'sm': 2,
  'md': 3,
  'lg': 4,
  'xl': 4,
)) {
  display: grid;
  gap: $spacing-md;

  @each $breakpoint, $cols in $columns {
    @if $breakpoint == 'xs' {
      grid-template-columns: repeat($cols, 1fr);
    } @else {
      @include respond-to($breakpoint) {
        grid-template-columns: repeat($cols, 1fr);
      }
    }
  }
}
```

```scss
// Génération de classes de grille responsive
@each $breakpoint, $value in $breakpoints {
  $prefix: if($breakpoint == 'xs', '', '#{$breakpoint}-');

  @include respond-to($breakpoint) {
    @for $i from 1 through 12 {
      .col-#{$prefix}#{$i} {
        grid-column: span $i;
      }
    }
  }
}
```

### Utilisation

```scss
// Layout auto-adaptatif
.features-grid {
  @include grid-responsive((
    'xs': 1,
    'sm': 2,
    'md': 2,
    'lg': 3,
    'xl': 4,
  ));
}

// Layout qui change selon le breakpoint
.dashboard {
  @include grid-responsive((
    'xs': 1,
    'md': 2,
    'lg': 4,
  ));
}

// Layout sidebar + content
.layout {
  display: grid;
  gap: $spacing-lg;

  @include respond-to('lg') {
    grid-template-columns: 250px 1fr;
  }
}
```

---

## Patterns modernes de responsive design

### 1. Layout switch (pas de colonnes flexibles)

```scss
.card-row {
  display: flex;
  flex-direction: column;
  gap: $spacing-md;

  @include respond-to('md') {
    flex-direction: row;
  }

  &__image {
    @include respond-below('md') {
      width: 100%;
      aspect-ratio: 16/9;
      object-fit: cover;
    }

    @include respond-to('md') {
      flex-shrink: 0;
      width: 200px;
    }
  }

  &__content {
    flex: 1;
  }
}
```

### 2. Holy grail layout

```scss
.layout-holy-grail {
  display: grid;
  gap: $spacing-md;
  min-height: 100vh;
  grid-template-areas:
    "header"
    "main"
    "sidebar"
    "footer";

  @include respond-to('md') {
    grid-template-columns: 200px 1fr 200px;
    grid-template-areas:
      "header header header"
      "nav    main   aside"
      "footer footer footer";
  }

  @include respond-to('xl') {
    grid-template-columns: 250px 1fr 300px;
  }

  &__header { grid-area: header; }
  &__nav    { grid-area: nav; }
  &__main   { grid-area: main; }
  &__aside  { grid-area: aside; }
  &__footer { grid-area: footer; }
}
```

### 3. Responsive typography

```scss
@mixin responsive-type($min-size, $max-size, $min-vw: 320px, $max-vw: 1200px) {
  $slope: ($max-size - $min-size) / ($max-vw - $min-vw);
  $intercept: $min-size - $slope * $min-vw;

  font-size: clamp(
    #{$min-size}px,
    #{$intercept / 16}rem + #{$slope * 100} * 1vw,
    #{$max-size}px
  );
}

.hero__title {
  @include responsive-type(28, 56);
  font-weight: 700;
  line-height: 1.1;
}

.hero__subtitle {
  @include responsive-type(16, 22);
  line-height: 1.5;
  color: $color-text-secondary;
}
```

### 4. Responsive spacing

```scss
@mixin responsive-spacing($property, $min, $max) {
  #{$property}: clamp(#{$min}, calc(#{$min} + (#{$max} - #{$min}) * ((100vw - 320px) / (1200 - 320px))), #{$max});
}

.section {
  @include responsive-spacing(padding-top, 2rem, 6rem);
  @include responsive-spacing(padding-bottom, 2rem, 6rem);
}
```

### 5. Responsive visibility

```scss
@mixin hide-at($breakpoint) {
  @include respond-to($breakpoint) {
    display: none !important;
  }
}

@mixin show-at($breakpoint) {
  display: none !important;

  @include respond-to($breakpoint) {
    display: block !important;
  }
}

// Utilisation
.desktop-only {
  @include hide-at('lg'); // Visible au-dessus de lg
  display: none;

  @include respond-to('lg') {
    display: block;
  }
}

.mobile-only {
  display: block;

  @include respond-to('md') {
    display: none !important;
  }
}

.tablet-up {
  display: none;

  @include respond-to('md') {
    display: block;
  }
}
```

### 6. Orientation

```scss
@mixin landscape {
  @media (orientation: landscape) {
    @content;
  }
}

@mixin portrait {
  @media (orientation: portrait) {
    @content;
  }
}

.gallery {
  columns: 3;

  @include portrait {
    columns: 2;
  }

  @include landscape {
    columns: 4;
  }
}
```

### 7. prefers-reduced-motion

```scss
@mixin motion-ok {
  @media (prefers-reduced-motion: no-preference) {
    @content;
  }
}

@mixin motion-reduce {
  @media (prefers-reduced-motion: reduce) {
    @content;
  }
}

.animated-element {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.3s ease, transform 0.3s ease;

  @include motion-reduce {
    opacity: 1;
    transform: none;
    transition: none;
  }
}
```

### 8. Hover capability

```scss
@mixin hover-capable {
  @media (hover: hover) and (pointer: fine) {
    @content;
  }
}

.btn {
  background-color: $color-primary;

  @include hover-capable {
    &:hover {
      background-color: darken($color-primary, 10%);
    }
  }
}
```

### 9. Dark mode

```scss
@mixin dark-mode {
  @media (prefers-color-scheme: dark) {
    @content;
  }
}

@mixin light-mode {
  @media (prefers-color-scheme: light) {
    @content;
  }
}

.card {
  background-color: white;
  color: #333;

  @include dark-mode {
    background-color: #1a1a2e;
    color: #e2e8f0;
  }
}
```

### 10. Print

```scss
@mixin print {
  @media print {
    @content;
  }
}

.header,
.footer,
.sidebar,
.nav {
  @include print {
    display: none;
  }
}

.article {
  @include print {
    max-width: 100%;
    box-shadow: none;
    border: none;
  }
}
```

---

## Mixin avancé : respond-map

```scss
@mixin respond-map($map) {
  @each $breakpoint, $styles in $map {
    @if $breakpoint == 'base' {
      // Styles de base (pas de media query)
      @each $property, $value in $styles {
        #{$property}: $value;
      }
    } @else {
      @include respond-to($breakpoint) {
        @each $property, $value in $styles {
          #{$property}: $value;
        }
      }
    }
  }
}

// Utilisation ultra-concise
.hero {
  @include respond-map((
    base: (
      padding: 1rem,
      font-size: 1.5rem,
    ),
    sm: (
      padding: 2rem,
      font-size: 2rem,
    ),
    md: (
      padding: 3rem,
      font-size: 2.5rem,
    ),
    lg: (
      padding: 4rem,
      font-size: 3.5rem,
    ),
    xl: (
      padding: 6rem,
      font-size: 4.5rem,
    ),
  ));
}
```

---

## Patterns de layouts responsive avancés

### 1. Responsive flex layout

```scss
@mixin flex-responsive($direction: row, $breakpoint: 'md') {
  display: flex;
  flex-direction: if($direction == row, column, row);
  gap: $spacing-md;

  @include respond-to($breakpoint) {
    flex-direction: $direction;
  }
}

.card-list {
  @include flex-responsive(row, 'md');
}

.stacked-cards {
  @include flex-responsive(column, 'lg');
}
```

### 2. Aspect ratio responsive

```scss
@mixin responsive-aspect-ratio($ratios: (
  'xs': 4/3,
  'sm': 16/9,
  'lg': 21/9,
)) {
  @each $breakpoint, $ratio in $ratios {
    @if $breakpoint == 'xs' {
      aspect-ratio: $ratio;
    } @else {
      @include respond-to($breakpoint) {
        aspect-ratio: $ratio;
      }
    }
  }
}

.video-wrapper {
  @include responsive-aspect-ratio((
    'xs': 16/9,
    'lg': 21/9,
  ));
  border-radius: $border-radius-lg;
  overflow: hidden;
}
```

### 3. Responsive typography scale

```scss
@mixin fluid-type($min, $max, $min-vw: 375px, $max-vw: 1440px) {
  font-size: clamp(
    #{$min},
    calc(#{$min} + #{($max - $min)} * (100vw - #{$min-vw}) / #{($max-vw - $min-vw)}),
    #{$max}
  );
}

.typography-scale {
  h1 { @include fluid-type(2rem, 4.5rem); }
  h2 { @include fluid-type(1.75rem, 3rem); }
  h3 { @include fluid-type(1.5rem, 2.25rem); }
  h4 { @include fluid-type(1.25rem, 1.75rem); }
  p  { @include fluid-type(1rem, 1.25rem); }
}
```

---

## Organisation des fichiers

```
sass/
├── config/
│   └── _breakpoints.scss
├── mixins/
│   ├── _responsive.scss
│   └── _grid.scss
├── utilities/
│   └── _responsive.scss
└── main.scss
```

```scss
// main.scss
@import 'config/breakpoints';
@import 'mixins/responsive';
@import 'mixins/grid';
```

---

## Bonnes pratiques

1. **Mobile-first** : utilisez `min-width` par défaut, c'est plus naturel.
2. **Map centralisée** : un seul fichier pour tous les breakpoints.
3. **Nommer clairement** : `respond-to('md')` pas `bp(768px)`.
4. **Éviter les magic numbers** : toujours passer par la map.
5. **Un breakpoint = une décision** : ne pas mélanger les breakpoints dans un même mixin.
6. **Tester sur vrais appareils** : les media queries ne suffisent pas, testez sur mobile.

---

## Résumé

Les media queries en SCSS deviennent un outil puissant grâce aux mixins et aux maps. Le système `respond-to` est le standard, mais les variantes `respond-below`, `respond-between` et `respond-map` offrent une flexibilité complète. En combinant avec des patterns modernes (clamp, grid, prefers-reduced-motion), on crée des designs truly responsive.
