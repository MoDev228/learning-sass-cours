# Chapitre 26 — Architecture 7-1

## Introduction

L'architecture **7-1** est le standard de facto pour organiser un projet SCSS de taille moyenne à grande. Inventée par Hugo Giraudelin, elle propose de séparer le code dans **7 dossiers** et de ne garder qu'**un seul fichier** à la racine : `main.scss`.

Ce chapitre détaille chaque dossier, son rôle, et fournit un arbre de fichiers complet.

---

## Le principe

```
sass/
├── abstracts/
├── vendors/
├── base/
├── layout/
├── components/
├── pages/
├── themes/
└── main.scss
```

On importe tout dans `main.scss` dans un ordre précis. Le navigateur ne reçoit qu'un seul fichier CSS compilé.

---

## Pourquoi cette architecture ?

| Problème | Solution 7-1 |
|---|---|
| Fichier SCSS de 3000+ lignes | Découpage logique par dossier |
| Difficile de retrouver un style | Chaque composant a son propre fichier |
| Conflits entre développeurs | Moins de chevauchement de fichiers |
| Difficile de maintenir | Imports ordonnés, responsabilités claires |
| Pas de convention | Standard adopté par la communauté |

---

## Les 7 dossiers

### 1. `abstracts/` — Outils abstraits

Ce dossier contient **tout ce qui ne produit aucun CSS** : variables, mixins, fonctions, placeholders.

> Règle : **aucune règle CSS** ne doit être écrite dans ce dossier.

```scss
// abstracts/_variables.scss

// Couleurs
$color-primary: #3498db;
$color-secondary: #2ecc71;
$color-danger: #e74c3c;
$color-dark: #2c3e50;
$color-light: #ecf0f1;
$color-white: #ffffff;

// Typographie
$font-primary: 'Open Sans', sans-serif;
$font-secondary: 'Montserrat', sans-serif;
$font-mono: 'Fira Code', monospace;

$font-size-base: 16px;
$font-size-sm: 14px;
$font-size-lg: 18px;
$font-size-xl: 24px;
$font-size-2xl: 32px;
$font-size-3xl: 48px;

$font-weight-light: 300;
$font-weight-regular: 400;
$font-weight-medium: 500;
$font-weight-bold: 700;

$line-height-tight: 1.2;
$line-height-base: 1.5;
$line-height-loose: 1.8;

// Espacement
$spacing-xs: 4px;
$spacing-sm: 8px;
$spacing-md: 16px;
$spacing-lg: 24px;
$spacing-xl: 32px;
$spacing-2xl: 48px;
$spacing-3xl: 64px;

// Breakpoints
$breakpoint-sm: 576px;
$breakpoint-md: 768px;
$breakpoint-lg: 992px;
$breakpoint-xl: 1200px;
$breakpoint-2xl: 1400px;

// Z-index
$z-dropdown: 1000;
$z-sticky: 1020;
$z-fixed: 1030;
$z-modal-backdrop: 1040;
$z-modal: 1050;
$z-tooltip: 1060;

// Ombres
$shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
$shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
$shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
$shadow-xl: 0 20px 25px rgba(0, 0, 0, 0.15);

// Border radius
$border-radius-sm: 4px;
$border-radius-md: 8px;
$border-radius-lg: 12px;
$border-radius-xl: 16px;
$border-radius-full: 9999px;

// Transitions
$transition-fast: 150ms ease;
$transition-base: 300ms ease;
$transition-slow: 500ms ease;

// Conteneurs
$container-max-width: 1200px;
$container-padding: $spacing-md;
```

```scss
// abstracts/_mixins.scss

@mixin respond-to($breakpoint) {
  @if $breakpoint == sm {
    @media (min-width: $breakpoint-sm) { @content; }
  } @else if $breakpoint == md {
    @media (min-width: $breakpoint-md) { @content; }
  } @else if $breakpoint == lg {
    @media (min-width: $breakpoint-lg) { @content; }
  } @else if $breakpoint == xl {
    @media (min-width: $breakpoint-xl) { @content; }
  } @else if $breakpoint == 2xl {
    @media (min-width: $breakpoint-2xl) { @content; }
  }
}

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

@mixin truncate($max-width: 100%) {
  max-width: $max-width;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

@mixin card($padding: $spacing-md) {
  background-color: $color-white;
  border-radius: $border-radius-md;
  box-shadow: $shadow-md;
  padding: $padding;
  transition: box-shadow $transition-base;

  &:hover {
    box-shadow: $shadow-lg;
  }
}

@mixin btn-reset {
  display: inline-block;
  cursor: pointer;
  border: none;
  background: none;
  font-family: inherit;
  font-size: inherit;
  color: inherit;
  padding: 0;
}

@mixin scrollbar($width: 8px, $track: transparent, $thumb: $color-light) {
  &::-webkit-scrollbar {
    width: $width;
  }

  &::-webkit-scrollbar-track {
    background: $track;
  }

  &::-webkit-scrollbar-thumb {
    background: $thumb;
    border-radius: $border-radius-full;
  }
}

@mixin absolute-fill {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}

@mixin aspect-ratio($ratio: 16 / 9) {
  aspect-ratio: $ratio;

  @supports not (aspect-ratio: 1) {
    &::before {
      content: '';
      display: block;
      padding-top: calc(100% / (#{$ratio}));
    }
  }
}
```

```scss
// abstracts/_functions.scss

@function spacing($multiplier) {
  @return $spacing-md * $multiplier;
}

@function color($name, $shade: null) {
  $palette: (
    primary: (
      light: lighten($color-primary, 15%),
      base: $color-primary,
      dark: darken($color-primary, 15%),
    ),
    secondary: (
      light: lighten($color-secondary, 15%),
      base: $color-secondary,
      dark: darken($color-secondary, 15%),
    ),
    danger: (
      light: lighten($color-danger, 15%),
      base: $color-danger,
      dark: darken($color-danger, 15%),
    ),
  );

  @if $shade == null {
    @return map-get($palette, $name);
  }

  @return map-deep-get($palette, $name, $shade);
}

@function rem($px) {
  @return $px / 16px * 1rem;
}

@function em($px, $base: $font-size-base) {
  @return $px / $base * 1em;
}
```

```scss
// abstracts/_placeholders.scss

%clearfix {
  &::after {
    content: '';
    display: table;
    clear: both;
  }
}

%truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

%list-reset {
  list-style: none;
  padding: 0;
  margin: 0;
}

%link-base {
  color: $color-primary;
  text-decoration: none;
  transition: color $transition-fast;

  &:hover {
    color: darken($color-primary, 15%);
  }
}
```

### 2. `vendors/` — Bibliothèques externes

Styles de CSS ou SCSS de bibliothèques tierces : Bootstrap, Normalize,Animate.css, etc.

```scss
// vendors/_normalize.scss
// Contenu de normalize.css

// vendors/_animate.scss
// Contenu d'animate.css

// vendors/_prism.scss
// Styles personnalisés pour Prism.js (syntax highlighting)

// vendors/_flatpickr.scss
// Styles du sélecteur de date Flatpickr
```

> **Note** : Avec `@import`, on importe les vendors **avant** tout le reste. Avec `@use`, on peut les encapsuler plus proprement.

### 3. `base/` — Styles de base

Réglages globaux et styles d'éléments HTML. Pas de classes, juste des sélecteurs d'éléments.

```scss
// base/_reset.scss
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
  font-family: $font-primary;
  font-size: $font-size-base;
  font-weight: $font-weight-regular;
  line-height: $line-height-base;
  color: $color-dark;
  background-color: $color-white;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

img,
video {
  max-width: 100%;
  height: auto;
  display: block;
}

a {
  color: inherit;
  text-decoration: none;
}

button {
  cursor: pointer;
  font: inherit;
}

input,
textarea,
select {
  font: inherit;
}
```

```scss
// base/_typography.scss

h1, h2, h3, h4, h5, h6 {
  font-family: $font-secondary;
  font-weight: $font-weight-bold;
  line-height: $line-height-tight;
  color: $color-dark;
  margin-bottom: $spacing-sm;
}

h1 { font-size: $font-size-3xl; }
h2 { font-size: $font-size-2xl; }
h3 { font-size: $font-size-xl; }
h4 { font-size: $font-size-lg; }
h5 { font-size: $font-size-base; }
h6 { font-size: $font-size-sm; }

p {
  margin-bottom: $spacing-md;
  line-height: $line-height-loose;
}

strong { font-weight: $font-weight-bold; }
em { font-style: italic; }
small { font-size: $font-size-sm; }

blockquote {
  border-left: 4px solid $color-primary;
  padding-left: $spacing-md;
  margin: $spacing-lg 0;
  font-style: italic;
  color: lighten($color-dark, 20%);
}

code {
  font-family: $font-mono;
  font-size: 0.9em;
  background-color: $color-light;
  padding: 2px 6px;
  border-radius: $border-radius-sm;
}

pre {
  background-color: $color-dark;
  color: $color-light;
  padding: $spacing-md;
  border-radius: $border-radius-md;
  overflow-x: auto;
  margin-bottom: $spacing-lg;

  code {
    background: none;
    padding: 0;
    color: inherit;
  }
}

hr {
  border: none;
  border-top: 1px solid $color-light;
  margin: $spacing-xl 0;
}
```

```scss
// base/_links.scss

a {
  @extend %link-base;
}

a:focus-visible {
  outline: 2px solid $color-primary;
  outline-offset: 2px;
}
```

```scss
// base/_lists.scss

ul, ol {
  @extend %list-reset;
}

ul {
  list-style-type: disc;
  padding-left: $spacing-lg;
}

ol {
  list-style-type: decimal;
  padding-left: $spacing-lg;
}

li {
  margin-bottom: $spacing-xs;
}
```

### 4. `layout/` — Structure de la page

Styles de la structure macro : header, footer, grilles, conteneurs, sections.

```scss
// layout/_grid.scss

.container {
  width: 100%;
  max-width: $container-max-width;
  margin-left: auto;
  margin-right: auto;
  padding-left: $container-padding;
  padding-right: $container-padding;
}

.row {
  display: flex;
  flex-wrap: wrap;
  margin-left: -$spacing-sm;
  margin-right: -$spacing-sm;
}

.col {
  flex: 1;
  padding-left: $spacing-sm;
  padding-right: $spacing-sm;
}

@for $i from 1 through 12 {
  .col-#{$i} {
    flex: 0 0 calc(#{$i} / 12 * 100%);
    max-width: calc(#{$i} / 12 * 100%);
    padding-left: $spacing-sm;
    padding-right: $spacing-sm;
  }
}

@include respond-to(md) {
  @for $i from 1 through 12 {
    .col-md-#{$i} {
      flex: 0 0 calc(#{$i} / 12 * 100%);
      max-width: calc(#{$i} / 12 * 100%);
    }
  }
}

@include respond-to(lg) {
  @for $i from 1 through 12 {
    .col-lg-#{$i} {
      flex: 0 0 calc(#{$i} / 12 * 100%);
      max-width: calc(#{$i} / 12 * 100%);
    }
  }
}
```

```scss
// layout/_header.scss

.header {
  position: sticky;
  top: 0;
  z-index: $z-sticky;
  background-color: $color-white;
  box-shadow: $shadow-sm;
  padding: $spacing-md 0;

  &__inner {
    @include flex-between;
    max-width: $container-max-width;
    margin: 0 auto;
    padding: 0 $container-padding;
  }

  &__logo {
    font-size: $font-size-xl;
    font-weight: $font-weight-bold;
    color: $color-primary;
  }

  &__nav {
    @include respond-to(md) {
      display: flex;
      gap: $spacing-lg;
    }
  }

  &__link {
    font-weight: $font-weight-medium;
    transition: color $transition-fast;

    &:hover {
      color: $color-primary;
    }
  }

  &__toggle {
    display: block;

    @include respond-to(md) {
      display: none;
    }
  }
}
```

```scss
// layout/_footer.scss

.footer {
  background-color: $color-dark;
  color: $color-light;
  padding: $spacing-2xl 0 $spacing-md;

  &__inner {
    max-width: $container-max-width;
    margin: 0 auto;
    padding: 0 $container-padding;
    display: grid;
    grid-template-columns: 1fr;

    @include respond-to(md) {
      grid-template-columns: 2fr 1fr 1fr;
      gap: $spacing-xl;
    }
  }

  &__section {
    margin-bottom: $spacing-lg;
  }

  &__title {
    font-size: $font-size-lg;
    margin-bottom: $spacing-md;
    color: $color-white;
  }

  &__link {
    color: $color-light;
    opacity: 0.8;
    transition: opacity $transition-fast;
    display: block;
    margin-bottom: $spacing-xs;

    &:hover {
      opacity: 1;
    }
  }

  &__bottom {
    max-width: $container-max-width;
    margin: $spacing-xl auto 0;
    padding-top: $spacing-md;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    text-align: center;
    font-size: $font-size-sm;
    opacity: 0.7;
  }
}
```

```scss
// layout/_section.scss

.section {
  padding: $spacing-2xl 0;

  &--gray {
    background-color: $color-light;
  }

  &--dark {
    background-color: $color-dark;
    color: $color-light;
  }

  &__title {
    text-align: center;
    margin-bottom: $spacing-xl;
  }
}
```

### 5. `components/` — Composants réutilisables

Boutons, cartes, formulaires, badges, modales, etc. Chaque fichier = un composant.

```scss
// components/_buttons.scss

.btn {
  @include btn-reset;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: $spacing-xs;
  padding: $spacing-sm $spacing-lg;
  font-size: $font-size-base;
  font-weight: $font-weight-medium;
  border-radius: $border-radius-md;
  transition: all $transition-base;
  line-height: 1;

  &:focus-visible {
    outline: 2px solid $color-primary;
    outline-offset: 2px;
  }

  // Variantes
  &--primary {
    background-color: $color-primary;
    color: $color-white;

    &:hover {
      background-color: darken($color-primary, 10%);
    }

    &:active {
      background-color: darken($color-primary, 20%);
    }
  }

  &--secondary {
    background-color: $color-secondary;
    color: $color-white;

    &:hover {
      background-color: darken($color-secondary, 10%);
    }
  }

  &--outline {
    background-color: transparent;
    border: 2px solid $color-primary;
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
      background-color: $color-light;
    }
  }

  &--danger {
    background-color: $color-danger;
    color: $color-white;

    &:hover {
      background-color: darken($color-danger, 10%);
    }
  }

  // Tailles
  &--sm {
    padding: $spacing-xs $spacing-md;
    font-size: $font-size-sm;
  }

  &--lg {
    padding: $spacing-md $spacing-xl;
    font-size: $font-size-lg;
  }

  &--block {
    width: 100%;
  }

  // État disabled
  &:disabled,
  &--disabled {
    opacity: 0.5;
    cursor: not-allowed;
    pointer-events: none;
  }
}
```

```scss
// components/_cards.scss

.card {
  @include card;

  &__image {
    margin: -#{$spacing-md};
    margin-bottom: $spacing-md;
    border-radius: $border-radius-md $border-radius-md 0 0;
    overflow: hidden;

    img {
      width: 100%;
      height: auto;
      object-fit: cover;
    }
  }

  &__header {
    margin-bottom: $spacing-sm;
    padding-bottom: $spacing-sm;
    border-bottom: 1px solid $color-light;
  }

  &__title {
    font-size: $font-size-lg;
    font-weight: $font-weight-bold;
  }

  &__subtitle {
    font-size: $font-size-sm;
    color: lighten($color-dark, 30%);
  }

  &__body {
    margin-bottom: $spacing-md;
  }

  &__text {
    line-height: $line-height-loose;
  }

  &__footer {
    display: flex;
    gap: $spacing-sm;
  }

  // Variante horizontal
  &--horizontal {
    @include respond-to(md) {
      display: flex;

      .card__image {
        margin: 0;
        flex-shrink: 0;
        width: 200px;
        border-radius: $border-radius-md 0 0 $border-radius-md;
      }
    }
  }

  // Variante sans bordure
  &--flat {
    box-shadow: none;
    border: 1px solid $color-light;

    &:hover {
      border-color: $color-primary;
    }
  }
}
```

```scss
// components/_forms.scss

.form {
  &__group {
    margin-bottom: $spacing-md;
  }

  &__label {
    display: block;
    font-weight: $font-weight-medium;
    margin-bottom: $spacing-xs;
    color: $color-dark;
  }

  &__required {
    color: $color-danger;
    margin-left: 2px;
  }

  &__input,
  &__textarea,
  &__select {
    width: 100%;
    padding: $spacing-sm $spacing-md;
    font-size: $font-size-base;
    border: 2px solid $color-light;
    border-radius: $border-radius-md;
    background-color: $color-white;
    transition: border-color $transition-fast, box-shadow $transition-fast;

    &:hover {
      border-color: darken($color-light, 10%);
    }

    &:focus {
      outline: none;
      border-color: $color-primary;
      box-shadow: 0 0 0 3px rgba($color-primary, 0.15);
    }

    &::placeholder {
      color: lighten($color-dark, 40%);
    }

    &--error {
      border-color: $color-danger;

      &:focus {
        box-shadow: 0 0 0 3px rgba($color-danger, 0.15);
      }
    }
  }

  &__textarea {
    min-height: 120px;
    resize: vertical;
  }

  &__select {
    appearance: none;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%23666' d='M6 8L1 3h10z'/%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-position: right $spacing-md center;
    padding-right: $spacing-xl;
  }

  &__help {
    font-size: $font-size-sm;
    color: lighten($color-dark, 30%);
    margin-top: $spacing-xs;
  }

  &__error {
    font-size: $font-size-sm;
    color: $color-danger;
    margin-top: $spacing-xs;
  }

  &__row {
    display: grid;
    gap: $spacing-md;

    @include respond-to(md) {
      grid-template-columns: 1fr 1fr;
    }
  }
}
```

```scss
// components/_badges.scss

.badge {
  display: inline-flex;
  align-items: center;
  padding: 2px 10px;
  font-size: 12px;
  font-weight: $font-weight-bold;
  border-radius: $border-radius-full;
  line-height: 1.5;

  &--primary {
    background-color: rgba($color-primary, 0.15);
    color: $color-primary;
  }

  &--secondary {
    background-color: rgba($color-secondary, 0.15);
    color: $color-secondary;
  }

  &--danger {
    background-color: rgba($color-danger, 0.15);
    color: $color-danger;
  }

  &--dark {
    background-color: rgba($color-dark, 0.15);
    color: $color-dark;
  }
}
```

### 6. `pages/` — Styles spécifiques à une page

Un fichier par page unique du site. Ces styles ne s'appliquent qu'à une seule page.

```scss
// pages/_home.scss

.home {
  &__hero {
    background: linear-gradient(135deg, $color-primary, darken($color-primary, 20%));
    color: $color-white;
    padding: $spacing-3xl 0;
    text-align: center;

    h1 {
      font-size: $font-size-3xl;
      margin-bottom: $spacing-md;
    }

    p {
      font-size: $font-size-xl;
      opacity: 0.9;
      max-width: 600px;
      margin: 0 auto $spacing-xl;
    }
  }

  &__features {
    display: grid;
    gap: $spacing-xl;
    grid-template-columns: 1fr;

    @include respond-to(md) {
      grid-template-columns: repeat(3, 1fr);
    }
  }
}

// pages/_about.scss

.about {
  &__content {
    max-width: 800px;
    margin: 0 auto;

    h2 {
      margin-top: $spacing-2xl;
      margin-bottom: $spacing-md;
    }

    p {
      font-size: $font-size-lg;
    }
  }

  &__team {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: $spacing-xl;
    margin-top: $spacing-2xl;
  }
}

// pages/_contact.scss

.contact {
  max-width: 600px;
  margin: 0 auto;
  padding: $spacing-2xl 0;

  &__title {
    text-align: center;
    margin-bottom: $spacing-xl;
  }
}
```

### 7. `themes/` — Thèmes visuels

Variables de thème, thème sombre, thème entreprise, etc.

```scss
// themes/_default.scss

:root {
  --color-primary: #{$color-primary};
  --color-secondary: #{$color-secondary};
  --color-danger: #{$color-danger};
  --color-dark: #{$color-dark};
  --color-light: #{$color-light};
  --color-bg: #{$color-white};
  --color-text: #{$color-dark};
  --shadow-base: #{$shadow-md};
  --font-base: #{$font-primary};
}

// themes/_dark.scss

[data-theme="dark"],
.dark {
  --color-primary: #{lighten($color-primary, 10%)};
  --color-secondary: #{lighten($color-secondary, 10%)};
  --color-danger: #{lighten($color-danger, 10%)};
  --color-dark: #ecf0f1;
  --color-light: #2c3e50;
  --color-bg: #1a1a2e;
  --color-text: #ecf0f1;
  --shadow-base: 0 4px 6px rgba(0, 0, 0, 0.3);

  background-color: var(--color-bg);
  color: var(--color-text);
}

// themes/_enterprise.scss

.enterprise {
  --color-primary: #003366;
  --color-secondary: #0066cc;
  --color-danger: #cc0000;
  --font-base: 'Roboto', sans-serif;
}
```

---

## Le fichier `main.scss`

C'est le seul fichier à importer dans le HTML. Il orchestre tout.

```scss
// main.scss

// 1. Vendors
@import 'vendors/normalize';
@import 'vendors/animate';

// 2. Abstracts (aucun CSS généré)
@import 'abstracts/variables';
@import 'abstracts/functions';
@import 'abstracts/mixins';
@import 'abstracts/placeholders';

// 3. Base (styles de base globaux)
@import 'base/reset';
@import 'base/typography';
@import 'base/links';
@import 'base/lists';

// 4. Layout (structure)
@import 'layout/grid';
@import 'layout/header';
@import 'layout/footer';
@import 'layout/section';

// 5. Components (composants UI)
@import 'components/buttons';
@import 'components/cards';
@import 'components/forms';
@import 'components/badges';

// 6. Pages (styles de pages spécifiques)
@import 'pages/home';
@import 'pages/about';
@import 'pages/contact';

// 7. Themes
@import 'themes/default';
@import 'themes/dark';
```

**Ordre des imports :** Les variables et mixins doivent être importés **avant** tout ce qui les utilise. C'est pourquoi `abstracts` vient en premier (après les vendors).

---

## Arbre de fichiers complet

```
sass/
├── abstracts/
│   ├── _variables.scss
│   ├── _functions.scss
│   ├── _mixins.scss
│   └── _placeholders.scss
│
├── vendors/
│   ├── _normalize.scss
│   ├── _animate.scss
│   └── _prism.scss
│
├── base/
│   ├── _reset.scss
│   ├── _typography.scss
│   ├── _links.scss
│   └── _lists.scss
│
├── layout/
│   ├── _grid.scss
│   ├── _header.scss
│   ├── _footer.scss
│   └── _section.scss
│
├── components/
│   ├── _buttons.scss
│   ├── _cards.scss
│   ├── _forms.scss
│   └── _badges.scss
│
├── pages/
│   ├── _home.scss
│   ├── _about.scss
│   └── _contact.scss
│
├── themes/
│   ├── _default.scss
│   ├── _dark.scss
│   └── _enterprise.scss
│
└── main.scss
```

---

## Variantes de l'architecture 7-1

### 7-2 (avec `utils/`)

```
sass/
├── abstracts/
├── vendors/
├── utils/        ← helpers CSS (margin, padding, display...)
├── base/
├── layout/
├── components/
├── pages/
├── themes/
└── main.scss
```

### 8-1 (avec `trumps/`)

Certains projets ajoutent un dossier `trumps/` (ou `utilities/`) pour les classes à haute spécificité qui doivent gagner les conflits :

```scss
// trumps/_spacing.scss

.mt-0 { margin-top: 0 !important; }
.mt-1 { margin-top: $spacing-xs !important; }
.mt-2 { margin-top: $spacing-sm !important; }
.mt-3 { margin-top: $spacing-md !important; }
.mt-4 { margin-top: $spacing-lg !important; }
.mt-5 { margin-top: $spacing-xl !important; }

.mb-0 { margin-bottom: 0 !important; }
.mb-1 { margin-bottom: $spacing-xs !important; }
.mb-2 { margin-bottom: $spacing-sm !important; }
.mb-3 { margin-bottom: $spacing-md !important; }
.mb-4 { margin-bottom: $spacing-lg !important; }
.mb-5 { margin-bottom: $spacing-xl !important; }

.p-0 { padding: 0 !important; }
.p-1 { padding: $spacing-xs !important; }
.p-2 { padding: $spacing-sm !important; }
.p-3 { padding: $spacing-md !important; }
.p-4 { padding: $spacing-lg !important; }
.p-5 { padding: $spacing-xl !important; }
```

---

## Avantages de l'architecture 7-1

| Avantage | Description |
|---|---|
| **Clarté** | On sait exactement où trouver chaque style |
| **Maintenabilité** | Modifier un composant ne touche pas aux autres |
| **Scalabilité** | Le projet peut grossir sans devenir ingérable |
| **Collaboration** | Moins de conflits Git entre développeurs |
| **Onboarding** | Nouveau développeur comprend la structure immédiatement |
| **Lazy loading mental** | On ne lit que les fichiers qui nous intéressent |

---

## Bonnes pratiques

1. **Un composant = un fichier** : ne jamais mélanger deux composants dans un même fichier.
2. **Prefix avec underscore** : les fichiers partiels (`_buttons.scss`) ne compilent pas directement.
3. **Ordre constant** : toujours respecter le même ordre dans `main.scss`.
4. **Pas de CSS dans abstracts** : ce dossier ne contient que des outils.
5. **Nommer clairement** : `_header.scss` pas `_style-header.scss` ni `_header-style.scss`.
6. **Pas de surdécoupage** : un seul fichier pour les boutons, pas un fichier par variante.

---

## Résumé

L'architecture 7-1 est un standard éprouvé qui rend les projets SCSS maintenables et scalables. En séparant le code en 7 catégories logiques et un fichier d'entrée unique, elle élimine le chaos des gros fichiers monolithiques. Adoptez-la comme base, et adaptez-la selon les besoins de votre projet.
