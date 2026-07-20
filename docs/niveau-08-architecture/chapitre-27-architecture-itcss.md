# Chapitre 27 — Architecture ITCSS

## Introduction

**ITCSS** (Inverted Triangle CSS) est une méthode d'architecture CSS créée par Harry Roberts. Elle organise le code en **7 couches** disposées en triangle inversé, de la plus générique à la plus spécifique.

```
         ← Générique                    Spécifique →
        ┌─────────────────────────────────────────────┐
        │              Utilities                       │
        │            ┌─────────────────────┐           │
        │            │     Components       │          │
        │          ┌─┴───────────────────┴─┐          │
        │          │       Objects          │          │
        │        ┌─┴───────────────────────┴─┐        │
        │        │        Elements           │        │
        │      ┌─┴───────────────────────────┴─┐      │
        │      │          Generic              │      │
        │    ┌─┴───────────────────────────────┴─┐    │
        │    │            Tools                   │    │
        │  ┌─┴───────────────────────────────────┴─┐  │
        │  │             Settings                   │  │
        │  └───────────────────────────────────────┘  │
        └─────────────────────────────────────────────┘
```

Chaque couche a un **pouvoir de spécificité** croissant, et on ne doit jamais revenir à une couche précédente.

---

## Les 7 couches

### Couche 1 : Settings (Paramètres)

Variables globales, config du projet. **Aucun CSS généré.**

```scss
// settings/_colors.scss

$color-primary: #3498db !default;
$color-secondary: #2ecc71 !default;
$color-danger: #e74c3c !default;
$color-warning: #f39c12 !default;
$color-dark: #2c3e50 !default;
$color-light: #ecf0f1 !default;
$color-white: #ffffff !default;

$color-text: $color-dark !default;
$color-bg: $color-white !default;
$color-link: $color-primary !default;
```

```scss
// settings/_typography.scss

$font-stack-sans: (
  'Helvetica Neue',
  'Arial',
  sans-serif
) !default;

$font-stack-serif: (
  'Georgia',
  'Times New Roman',
  serif
) !default;

$font-stack-mono: (
  'Fira Code',
  'Courier New',
  monospace
) !default;

$font-size-base: 16px !default;
$font-size-ratio: 1.25 !default;

$font-size-xs: $font-size-base * 0.8 !default;    // 12.8px
$font-size-sm: $font-size-base * 0.9 !default;    // 14.4px
$font-size-md: $font-size-base !default;           // 16px
$font-size-lg: $font-size-base * 1.125 !default;   // 18px
$font-size-xl: $font-size-base * 1.25 !default;    // 20px
$font-size-2xl: $font-size-base * 1.5 !default;    // 24px
$font-size-3xl: $font-size-base * 2 !default;      // 32px
$font-size-4xl: $font-size-base * 2.5 !default;    // 40px

$line-height-base: 1.5 !default;
$font-weight-normal: 400 !default;
$font-weight-bold: 700 !default;
```

```scss
// settings/_spacing.scss

$spacing-unit: 8px !default;
$spacing-0: 0 !default;
$spacing-1: $spacing-unit !default;         // 8px
$spacing-2: $spacing-unit * 2 !default;     // 16px
$spacing-3: $spacing-unit * 3 !default;     // 24px
$spacing-4: $spacing-unit * 4 !default;     // 32px
$spacing-5: $spacing-unit * 5 !default;     // 40px
$spacing-6: $spacing-unit * 6 !default;     // 48px
$spacing-8: $spacing-unit * 8 !default;     // 64px

$container-max-width: 1200px !default;
$container-padding: $spacing-2 !default;
```

```scss
// settings/_breakpoints.scss

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
  2xl: $breakpoint-2xl,
) !default;
```

```scss
// settings/_shadows.scss

$shadow-1: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.06) !default;
$shadow-2: 0 4px 6px rgba(0, 0, 0, 0.1), 0 2px 4px rgba(0, 0, 0, 0.06) !default;
$shadow-3: 0 10px 15px rgba(0, 0, 0, 0.1), 0 4px 6px rgba(0, 0, 0, 0.05) !default;
$shadow-4: 0 20px 25px rgba(0, 0, 0, 0.1), 0 10px 10px rgba(0, 0, 0, 0.04) !default;
$shadow-5: 0 25px 50px rgba(0, 0, 0, 0.25) !default;

$shadow-inner: inset 0 2px 4px rgba(0, 0, 0, 0.06) !default;
```

```scss
// settings/_borders.scss

$border-width: 1px !default;
$border-color: $color-light !default;
$border-radius-sm: 3px !default;
$border-radius-md: 6px !default;
$border-radius-lg: 12px !default;
$border-radius-xl: 16px !default;
$border-radius-full: 9999px !default;
```

```scss
// settings/_transitions.scss

$transition-duration: 300ms !default;
$transition-easing: cubic-bezier(0.25, 0.46, 0.45, 0.94) !default;
$transition-easing-bounce: cubic-bezier(0.68, -0.55, 0.27, 1.55) !default;
$transition-property: all !default;
```

```scss
// settings/_z-index.scss

$z-index-dropdown: 1000 !default;
$z-index-sticky: 1020 !default;
$z-index-fixed: 1030 !default;
$z-index-modal-backdrop: 1040 !default;
$z-index-modal: 1050 !default;
$z-index-tooltip: 1060 !default;
```

---

### Couche 2 : Tools (Outils)

Mixins, fonctions, helpers Sass. **Toujours aucun CSS généré.**

```scss
// tools/_mixins.scss

@mixin respond-above($breakpoint) {
  $value: map-get($breakpoints, $breakpoint);

  @if $value {
    @media (min-width: $value) {
      @content;
    }
  } @else {
    @warn "Pas de breakpoint `#{$breakpoint}` trouvé dans la map.";
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

@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

@mixin visually-hidden {
  border: 0 !important;
  clip: rect(1px, 1px, 1px, 1px) !important;
  clip-path: inset(50%) !important;
  height: 1px !important;
  overflow: hidden !important;
  padding: 0 !important;
  position: absolute !important;
  width: 1px !important;
  white-space: nowrap !important;
}

@mixin text-truncate {
  display: block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

@mixin absolute-fill {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}

@mixin fixed-fill {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}

@mixin font-size($size) {
  font-size: $size;
  line-height: $line-height-base;
}

@mixin hover-lift($distance: -3px, $shadow: $shadow-3) {
  transition: transform $transition-duration $transition-easing,
              box-shadow $transition-duration $transition-easing;

  &:hover {
    transform: translateY($distance);
    box-shadow: $shadow;
  }
}
```

```scss
// tools/_functions.scss

@function spacing($times: 1) {
  @return $spacing-unit * $times;
}

@function z($layer) {
  $value: map-get((
    dropdown: $z-index-dropdown,
    sticky: $z-index-sticky,
    fixed: $z-index-fixed,
    modal-backdrop: $z-index-modal-backdrop,
    modal: $z-index-modal,
    tooltip: $z-index-tooltip,
  ), $layer);

  @if $value {
    @return $value;
  }

  @warn "Couche z-index `#{$layer}` inconnue.";
  @return null;
}

@function rem($px) {
  @return $px / 16 * 1rem;
}

@function em($px, $base: 16) {
  @return $px / $base * 1em;
}

@function darken-mixin($color, $amount) {
  @return darken($color, $amount);
}

@function contrast-color($bg-color) {
  $r: red($bg-color);
  $g: green($bg-color);
  $b: blue($bg-color);
  $luminance: (0.299 * $r + 0.587 * $g + 0.114 * $b) / 255;

  @if $luminance > 0.5 {
    @return $color-dark;
  } @else {
    @return $color-white;
  }
}
```

```scss
// tools/_placeholders.scss

%clearfix {
  &::after {
    content: '';
    display: table;
    clear: both;
  }
}

%list-reset {
  list-style: none;
  margin: 0;
  padding: 0;
}

%link-base {
  color: $color-link;
  text-decoration: none;

  &:hover {
    text-decoration: underline;
  }
}

%absolute-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

%scrollbar-hidden {
  -ms-overflow-style: none;
  scrollbar-width: none;

  &::-webkit-scrollbar {
    display: none;
  }
}
```

---

### Couche 3 : Generic (Générique)

Styles de base à haut impact, Reset/Normalize. Premiers CSS générés.

```scss
// generic/_reset.scss

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: 100%;
  -webkit-text-size-adjust: 100%;
  scroll-behavior: smooth;
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

p,
h1,
h2,
h3,
h4,
h5,
h6 {
  overflow-wrap: break-word;
}

a {
  color: inherit;
  text-decoration: none;
}

ul,
ol {
  @extend %list-reset;
}
```

```scss
// generic/_normalize.scss
// (Contenu de normalize.css ou modern-normalize)

html {
  line-height: 1.15;
  -webkit-text-size-adjust: 100%;
}

body {
  margin: 0;
}

main {
  display: block;
}

h1 {
  font-size: 2em;
  margin: 0.67em 0;
}

hr {
  box-sizing: content-box;
  height: 0;
  overflow: visible;
  border-top: 1px solid lightgray;
  border-right: 0;
  border-bottom: 0;
  border-left: 0;
  margin: 1em 0;
}

pre {
  font-family: monospace, monospace;
  font-size: 1em;
}

abbr[title] {
  text-decoration: underline dotted;
}

b,
strong {
  font-weight: 700;
}

code,
kbd,
samp {
  font-family: monospace, monospace;
  font-size: 1em;
}

small {
  font-size: 80%;
}

sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}

sub {
  bottom: -0.25em;
}

sup {
  top: -0.5em;
}
```

```scss
// generic/_shared.scss

body {
  font-family: $font-stack-sans;
  font-size: $font-size-base;
  line-height: $line-height-base;
  color: $color-text;
  background-color: $color-bg;
}

::selection {
  background-color: $color-primary;
  color: $color-white;
}

:focus-visible {
  outline: 2px solid $color-primary;
  outline-offset: 2px;
}

.sr-only {
  @extend %visually-hidden;
}
```

---

### Couche 4 : Elements (Éléments)

Styles des éléments HTML nus (pas de classes). C'est le cœur typographique.

```scss
// elements/_headings.scss

h1, h2, h3, h4, h5, h6 {
  font-family: $font-stack-sans;
  font-weight: $font-weight-bold;
  line-height: 1.2;
  margin-bottom: 0.5em;
}

h1 { font-size: $font-size-4xl; }
h2 { font-size: $font-size-3xl; }
h3 { font-size: $font-size-2xl; }
h4 { font-size: $font-size-xl; }
h5 { font-size: $font-size-lg; }
h6 { font-size: $font-size-md; }
```

```scss
// elements/_links.scss

a {
  @extend %link-base;
}

a:focus-visible {
  outline: 2px solid $color-primary;
  outline-offset: 2px;
}
```

```scss
// elements/_buttons.scss

button {
  cursor: pointer;
  font: inherit;
  border: none;
  background: none;
}

[type="submit"],
[type="button"],
button {
  appearance: none;
  cursor: pointer;
}
```

```scss
// elements/_forms.scss

input,
textarea,
select {
  display: block;
  width: 100%;
  padding: $spacing-1 $spacing-2;
  font-size: $font-size-base;
  border: $border-width solid $border-color;
  border-radius: $border-radius-md;
  background-color: $color-bg;
  color: $color-text;
  transition: border-color $transition-duration $transition-easing,
              box-shadow $transition-duration $transition-easing;

  &:focus {
    outline: none;
    border-color: $color-primary;
    box-shadow: 0 0 0 3px rgba($color-primary, 0.2);
  }

  &::placeholder {
    color: lighten($color-text, 40%);
  }
}

textarea {
  min-height: 120px;
  resize: vertical;
}
```

```scss
// elements/_misc.scss

hr {
  border: none;
  border-top: $border-width solid $border-color;
  margin: $spacing-3 0;
}

blockquote {
  border-left: 4px solid $color-primary;
  padding: $spacing-2 $spacing-3;
  margin: $spacing-3 0;
  font-style: italic;
  background-color: rgba($color-primary, 0.05);
  border-radius: 0 $border-radius-md $border-radius-md 0;
}

code {
  font-family: $font-stack-mono;
  font-size: 0.9em;
  background-color: rgba($color-dark, 0.05);
  padding: 2px 6px;
  border-radius: $border-radius-sm;
}

pre {
  background-color: $color-dark;
  color: $color-light;
  padding: $spacing-3;
  border-radius: $border-radius-md;
  overflow-x: auto;
  margin: $spacing-3 0;

  code {
    background: none;
    padding: 0;
    color: inherit;
    font-size: 1em;
  }
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: $spacing-1 $spacing-2;
  text-align: left;
  border-bottom: $border-width solid $border-color;
}

th {
  font-weight: $font-weight-bold;
}

img {
  max-width: 100%;
  height: auto;
}
```

---

### Couche 5 : Objects (Objets)

Patterns structurels sans apparence visuelle. Layouts, grilles, conteneurs.

```scss
// objects/_container.scss

.o-container {
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

```scss
// objects/_grid.scss

.o-grid {
  display: grid;
  gap: $spacing-2;

  &--2 {
    grid-template-columns: repeat(2, 1fr);
  }

  &--3 {
    grid-template-columns: repeat(3, 1fr);
  }

  &--4 {
    grid-template-columns: repeat(4, 1fr);
  }

  &--auto {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  }

  @include respond-above(md) {
    &--2\@md {
      grid-template-columns: repeat(2, 1fr);
    }

    &--3\@md {
      grid-template-columns: repeat(3, 1fr);
    }

    &--4\@md {
      grid-template-columns: repeat(4, 1fr);
    }
  }
}
```

```scss
// objects/_flex.scss

.o-flex {
  display: flex;

  &--center {
    align-items: center;
    justify-content: center;
  }

  &--between {
    align-items: center;
    justify-content: space-between;
  }

  &--wrap {
    flex-wrap: wrap;
  }

  &--column {
    flex-direction: column;
  }

  &--gap-1 { gap: $spacing-1; }
  &--gap-2 { gap: $spacing-2; }
  &--gap-3 { gap: $spacing-3; }
}
```

```scss
// objects/_media.scss

.o-media {
  display: flex;
  align-items: flex-start;

  &__body {
    flex: 1;
  }

  &__img {
    flex-shrink: 0;

    &--left {
      margin-right: $spacing-2;
    }

    &--right {
      margin-left: $spacing-2;
      order: 1;
    }
  }
}
```

```scss
// objects/_stack.scss

.o-stack {
  display: flex;
  flex-direction: column;

  & > * + * {
    margin-top: $spacing-2;
  }

  &--sm > * + * {
    margin-top: $spacing-1;
  }

  &--lg > * + * {
    margin-top: $spacing-4;
  }

  &--flush > * + * {
    margin-top: 0;
  }
}
```

```scss
// objects/_cover.scss

.o-cover {
  display: flex;
  flex-direction: column;
  min-height: 100vh;

  &__main {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: $spacing-4 0;
  }
}
```

```scss
// objects/_center.scss

.o-center {
  max-width: $container-max-width;
  margin-left: auto;
  margin-right: auto;
  padding-left: $container-padding;
  padding-right: $container-padding;

  &--text {
    text-align: center;
  }
}
```

---

### Couche 6 : Components (Composants)

Éléments d'interface avec apparence visuelle. Boutons, cartes, nav, modales, etc.

```scss
// components/_button.scss

.c-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  padding: $spacing-1 $spacing-2;
  font-size: $font-size-base;
  font-weight: $font-weight-bold;
  border: $border-width solid transparent;
  border-radius: $border-radius-md;
  cursor: pointer;
  transition: all $transition-duration $transition-easing;

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  &--primary {
    background-color: $color-primary;
    color: contrast-color($color-primary);

    &:hover:not(:disabled) {
      background-color: darken($color-primary, 8%);
    }
  }

  &--secondary {
    background-color: $color-secondary;
    color: contrast-color($color-secondary);

    &:hover:not(:disabled) {
      background-color: darken($color-secondary, 8%);
    }
  }

  &--danger {
    background-color: $color-danger;
    color: contrast-color($color-danger);

    &:hover:not(:disabled) {
      background-color: darken($color-danger, 8%);
    }
  }

  &--outline {
    background-color: transparent;
    border-color: $color-primary;
    color: $color-primary;

    &:hover:not(:disabled) {
      background-color: $color-primary;
      color: $color-white;
    }
  }

  &--ghost {
    background-color: transparent;
    color: $color-primary;

    &:hover:not(:disabled) {
      background-color: rgba($color-primary, 0.1);
    }
  }

  &--sm {
    padding: $spacing-0 $spacing-1;
    font-size: $font-size-sm;
  }

  &--lg {
    padding: $spacing-2 $spacing-3;
    font-size: $font-size-lg;
  }

  &--block {
    display: flex;
    width: 100%;
  }

  &--icon {
    padding: $spacing-1;

    svg {
      width: 20px;
      height: 20px;
    }
  }
}
```

```scss
// components/_card.scss

.c-card {
  background-color: $color-bg;
  border: $border-width solid $border-color;
  border-radius: $border-radius-lg;
  overflow: hidden;
  transition: box-shadow $transition-duration $transition-easing;

  &:hover {
    box-shadow: $shadow-3;
  }

  &__image {
    width: 100%;
    aspect-ratio: 16 / 9;
    object-fit: cover;
  }

  &__body {
    padding: $spacing-3;
  }

  &__title {
    font-size: $font-size-xl;
    font-weight: $font-weight-bold;
    margin-bottom: $spacing-1;
  }

  &__text {
    color: lighten($color-text, 20%);
    line-height: $line-height-base;
  }

  &__footer {
    padding: $spacing-2 $spacing-3;
    border-top: $border-width solid $border-color;
    display: flex;
    gap: $spacing-1;
  }
}
```

```scss
// components/_nav.scss

.c-nav {
  display: flex;
  align-items: center;
  gap: $spacing-2;
  padding: $spacing-1 0;

  &__brand {
    font-size: $font-size-xl;
    font-weight: $font-weight-bold;
    color: $color-primary;
    margin-right: auto;
  }

  &__link {
    color: $color-text;
    font-weight: $font-weight-normal;
    padding: $spacing-1 $spacing-2;
    border-radius: $border-radius-md;
    transition: background-color $transition-duration $transition-easing;

    &:hover {
      background-color: rgba($color-dark, 0.05);
      text-decoration: none;
    }

    &--active {
      color: $color-primary;
      font-weight: $font-weight-bold;
    }
  }

  &__toggle {
    display: block;

    @include respond-above(md) {
      display: none;
    }
  }

  &__list {
    display: none;
    flex-direction: column;
    gap: $spacing-1;

    @include respond-above(md) {
      display: flex;
      flex-direction: row;
    }

    &--open {
      display: flex;
    }
  }
}
```

```scss
// components/_badge.scss

.c-badge {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 20px;
  height: 20px;
  padding: 0 6px;
  font-size: 11px;
  font-weight: $font-weight-bold;
  border-radius: $border-radius-full;
  line-height: 1;

  &--primary {
    background-color: $color-primary;
    color: $color-white;
  }

  &--danger {
    background-color: $color-danger;
    color: $color-white;
  }

  &--warning {
    background-color: $color-warning;
    color: $color-dark;
  }

  &--pill {
    border-radius: $border-radius-full;
    padding: 0 10px;
    height: 24px;
  }
}
```

```scss
// components/_alert.scss

.c-alert {
  padding: $spacing-2 $spacing-3;
  border-radius: $border-radius-md;
  border-left: 4px solid;
  margin-bottom: $spacing-2;

  &--success {
    background-color: rgba($color-secondary, 0.1);
    border-color: $color-secondary;
    color: darken($color-secondary, 20%);
  }

  &--danger {
    background-color: rgba($color-danger, 0.1);
    border-color: $color-danger;
    color: darken($color-danger, 20%);
  }

  &--warning {
    background-color: rgba($color-warning, 0.1);
    border-color: $color-warning;
    color: darken($color-warning, 20%);
  }

  &--info {
    background-color: rgba($color-primary, 0.1);
    border-color: $color-primary;
    color: darken($color-primary, 20%);
  }

  &__title {
    font-weight: $font-weight-bold;
    margin-bottom: $spacing-1;
  }

  &__text {
    line-height: $line-height-base;
  }
}
```

---

### Couche 7 : Utilities (Utilitaires)

Classes d'aide à haute spécificité pour les ajustements finaux.

```scss
// utilities/_display.scss

.u-hidden { display: none !important; }
.u-block { display: block !important; }
.u-flex { display: flex !important; }
.u-grid { display: grid !important; }
.u-inline { display: inline !important; }
.u-inline-block { display: inline-block !important; }

@media (min-width: $breakpoint-md) {
  .u-md-hidden { display: none !important; }
  .u-md-block { display: block !important; }
  .u-md-flex { display: flex !important; }
}

@media (min-width: $breakpoint-lg) {
  .u-lg-hidden { display: none !important; }
  .u-lg-block { display: block !important; }
  .u-lg-flex { display: flex !important; }
}
```

```scss
// utilities/_text.scss

.u-text-center { text-align: center !important; }
.u-text-left { text-align: left !important; }
.u-text-right { text-align: right !important; }

.u-text-uppercase { text-transform: uppercase !important; }
.u-text-lowercase { text-transform: lowercase !important; }
.u-text-capitalize { text-transform: capitalize !important; }
.u-text-truncate { @extend %text-truncate; }

.u-text-primary { color: $color-primary !important; }
.u-text-danger { color: $color-danger !important; }
.u-text-success { color: $color-secondary !important; }
.u-text-muted { color: lighten($color-text, 30%) !important; }
.u-text-white { color: $color-white !important; }
.u-text-dark { color: $color-dark !important; }

.u-font-bold { font-weight: $font-weight-bold !important; }
.u-font-normal { font-weight: $font-weight-normal !important; }

.u-font-xs { font-size: $font-size-xs !important; }
.u-font-sm { font-size: $font-size-sm !important; }
.u-font-md { font-size: $font-size-md !important; }
.u-font-lg { font-size: $font-size-lg !important; }
.u-font-xl { font-size: $font-size-xl !important; }
.u-font-2xl { font-size: $font-size-2xl !important; }
```

```scss
// utilities/_spacing.scss

$directions: (
  t: top,
  r: right,
  b: bottom,
  l: left,
  x: (left, right),
  y: (top, bottom),
  '': (top, right, bottom, left),
);

@each $shorthand, $sides in $directions {
  @each $size-name, $size-value in (
    0: 0,
    1: $spacing-1,
    2: $spacing-2,
    3: $spacing-3,
    4: $spacing-4,
    5: $spacing-5,
    6: $spacing-6,
    8: $spacing-8,
  ) {
    @if $shorthand == '' {
      .u-m#{$size-name} { margin: $size-value !important; }
      .u-p#{$size-name} { padding: $size-value !important; }
    } @else {
      @if index($sides, top) {
        .u-mt#{$size-name} { margin-top: $size-value !important; }
        .u-pt#{$size-name} { padding-top: $size-value !important; }
      }
      @if index($sides, right) {
        .u-mr#{$size-name} { margin-right: $size-value !important; }
        .u-pr#{$size-name} { padding-right: $size-value !important; }
      }
      @if index($sides, bottom) {
        .u-mb#{$size-name} { margin-bottom: $size-value !important; }
        .u-pb#{$size-name} { padding-bottom: $size-value !important; }
      }
      @if index($sides, left) {
        .u-ml#{$size-name} { margin-left: $size-value !important; }
        .u-pl#{$size-name} { padding-left: $size-value !important; }
      }
    }
  }
}

.u-mx-auto { margin-left: auto !important; margin-right: auto !important; }
```

```scss
// utilities/_visibility.scss

.u-sr-only {
  @extend %visually-hidden;
}

.u-invisible { visibility: hidden !important; }
.u-visible { visibility: visible !important; }
.u-opacity-0 { opacity: 0 !important; }
.u-opacity-50 { opacity: 0.5 !important; }
.u-opacity-100 { opacity: 1 !important; }
```

```scss
// utilities/_width.scss

.u-w-auto { width: auto !important; }
.u-w-full { width: 100% !important; }
.u-w-screen { width: 100vw !important; }
.u-w-half { width: 50% !important; }
.u-w-third { width: 33.333% !important; }
.u-w-quarter { width: 25% !important; }

.u-max-w-sm { max-width: 540px !important; }
.u-max-w-md { max-width: 720px !important; }
.u-max-w-lg { max-width: 960px !important; }
.u-max-w-xl { max-width: 1140px !important; }
```

---

## Fichier d'entrée ITCSS

```scss
// main.scss

// === 1. SETTINGS ===
@import 'settings/colors';
@import 'settings/typography';
@import 'settings/spacing';
@import 'settings/breakpoints';
@import 'settings/shadows';
@import 'settings/borders';
@import 'settings/transitions';
@import 'settings/z-index';

// === 2. TOOLS ===
@import 'tools/mixins';
@import 'tools/functions';
@import 'tools/placeholders';

// === 3. GENERIC ===
@import 'generic/reset';
@import 'generic/normalize';
@import 'generic/shared';

// === 4. ELEMENTS ===
@import 'elements/headings';
@import 'elements/links';
@import 'elements/buttons';
@import 'elements/forms';
@import 'elements/misc';

// === 5. OBJECTS ===
@import 'objects/container';
@import 'objects/grid';
@import 'objects/flex';
@import 'objects/media';
@import 'objects/stack';
@import 'objects/cover';
@import 'objects/center';

// === 6. COMPONENTS ===
@import 'components/button';
@import 'components/card';
@import 'components/nav';
@import 'components/badge';
@import 'components/alert';

// === 7. UTILITIES ===
@import 'utilities/display';
@import 'utilities/text';
@import 'utilities/spacing';
@import 'utilities/visibility';
@import 'utilities/width';
```

---

## Comparaison ITCSS vs 7-1

| Critère | 7-1 | ITCSS |
|---|---|---|
| **Nombre de couches** | 7 dossiers | 7 couches |
| **Approche** | Logique (par type) | Spécificité (par impact) |
| **Règle d'or** | Organisation par rôle | Ne jamais remonter dans les couches |
| **Utilities** | Parfois dans `trumps/` | Toujours en couche 7 |
| **Courbe d'apprentissage** | Plus simple | Plus conceptuel |
| **Idéal pour** | Projets moyens | Projets grands et complexes |
| **Écosystème** | Plus adopté | Plus rigoureux |
| **Scalabilité** | Bonne | Excellente |
| **Conflits de spécificité** | Possible | Minimisé par conception |

### Correspondance approximative

| ITCSS | 7-1 |
|---|---|
| Settings | `abstracts/_variables` |
| Tools | `abstracts/_mixins`, `_functions` |
| Generic | `base/_reset` |
| Elements | `base/_typography`, `_links` |
| Objects | `layout/` |
| Components | `components/` |
| Utilities | `trumps/` ou `utils/` |

### Quand choisir quoi ?

- **7-1** : projets personnels, petits équipes, applications simples.
- **ITCSS** : projets d'entreprise, design systems, bibliothèques CSS.
- **Hybride** : les deux fonctionnent ensemble. Utilisez la structure 7-1 avec la mentalité ITCSS (spécificité croissante).

---

## Arbre de fichiers complet ITCSS

```
sass/
├── settings/
│   ├── _colors.scss
│   ├── _typography.scss
│   ├── _spacing.scss
│   ├── _breakpoints.scss
│   ├── _shadows.scss
│   ├── _borders.scss
│   ├── _transitions.scss
│   └── _z-index.scss
│
├── tools/
│   ├── _mixins.scss
│   ├── _functions.scss
│   └── _placeholders.scss
│
├── generic/
│   ├── _reset.scss
│   ├── _normalize.scss
│   └── _shared.scss
│
├── elements/
│   ├── _headings.scss
│   ├── _links.scss
│   ├── _buttons.scss
│   ├── _forms.scss
│   └── _misc.scss
│
├── objects/
│   ├── _container.scss
│   ├── _grid.scss
│   ├── _flex.scss
│   ├── _media.scss
│   ├── _stack.scss
│   ├── _cover.scss
│   └── _center.scss
│
├── components/
│   ├── _button.scss
│   ├── _card.scss
│   ├── _nav.scss
│   ├── _badge.scss
│   └── _alert.scss
│
├── utilities/
│   ├── _display.scss
│   ├── _text.scss
│   ├── _spacing.scss
│   ├── _visibility.scss
│   └── _width.scss
│
└── main.scss
```

---

## Résumé

ITCSS apporte une rigueur supplémentaire à l'organisation CSS. En imposant un ordre strict de spécificité croissante, elle élimine les problèmes de cascade et rend le code prévisible. Combinée à l'esprit de la 7-1, elle constitue la fondation idéale pour tout projet CSS sérieux.
