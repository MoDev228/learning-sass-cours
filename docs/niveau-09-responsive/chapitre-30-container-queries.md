# Chapitre 30 — Container Queries

## Introduction

Les **container queries** sont la révolution du responsive design moderne. Contrairement aux media queries qui interoggent la taille du viewport, les container queries interoggent la taille du **conteneur parent**. Cela signifie qu'un composant peut s'adapter à son espace disponible, pas à l'écran.

```css
/* Media query : "comment est l'écran ?" */
@media (min-width: 768px) { ... }

/* Container query : "comment est mon conteneur ?" */
@container (min-width: 500px) { ... }
```

---

## Concept fondamental

### Le problème des media queries

```scss
// Avec les media queries, un composant card
// s'adapte à la taille de l'ÉCRAN, pas à son conteneur
.card {
  display: flex;
  flex-direction: column;

  @media (min-width: 768px) {
    flex-direction: row;  // Problème : le sidebar fait 300px
  }                       // mais l'écran fait 900px → le card
}                         // passe en row alors qu'il n'a pas la place
```

### La solution des container queries

```scss
// Avec les container queries, le card s'adapte
// à la taille de SON conteneur
.card-container {
  container-type: inline-size;
}

.card {
  display: flex;
  flex-direction: column;

  @container (min-width: 500px) {
    flex-direction: row;  // Ne s'active que si le conteneur
  }                       // fait au moins 500px, quel que soit
}                         // l'écran
```

---

## CSS natif des container queries

```css
/* Déclarer un conteneur */
.card-wrapper {
  container-type: inline-size;
  container-name: card;
}

/* Styles conditionnels */
@container card (min-width: 500px) {
  .card {
    flex-direction: row;
  }
}

@container card (min-width: 700px) {
  .card__image {
    width: 300px;
  }
}
```

### container-type

```css
/* inline-size : width uniquement */
.container {
  container-type: inline-size;
}

/* size : width et height */
.container {
  container-type: size;
}

/* normal : pas de conteneur */
.container {
  container-type: normal;
}
```

### container-name

```css
/* Nommer un conteneur pour le cibler */
.sidebar {
  container-type: inline-size;
  container-name: sidebar;
}

@container sidebar (min-width: 250px) {
  .nav-item {
    display: flex;
    align-items: center;
  }
}
```

---

## Container Queries en SCSS

### Mixin de base

```scss
// mixins/_container-queries.scss

@mixin container-query($property: inline-size, $name: null) {
  container-type: $property;

  @if $name {
    container-name: $name;
  }
}

@mixin cq($breakpoint, $name: null) {
  $query: if($name, '#{$name} (min-width: #{$breakpoint})', '(min-width: #{$breakpoint})');

  @container #{$query} {
    @content;
  }
}

@mixin cq-max($breakpoint, $name: null) {
  $query: if($name, '#{$name} (max-width: #{$breakpoint - 0.02px})', '(max-width: #{$breakpoint - 0.02px})');

  @container #{$query} {
    @content;
  }
}

@mixin cq-between($min, $max, $name: null) {
  $query: if(
    $name,
    '#{$name} (min-width: #{$min}) and (max-width: #{$max - 0.02px})',
    '(min-width: #{$min}) and (max-width: #{$max - 0.02px})'
  );

  @container #{$query} {
    @content;
  }
}
```

### Map de breakpoints de conteneur

```scss
// config/_container-breakpoints.scss

$container-breakpoints: (
  'xs': 0,
  'sm': 320px,
  'md': 480px,
  'lg': 640px,
  'xl': 800px,
  '2xl': 960px,
) !default;

@mixin container-query-map($breakpoint, $name: null) {
  $value: map-get($container-breakpoints, $breakpoint);

  @if $value {
    @if $value == 0 {
      // Pas de media query, styles de base
      @content;
    } @else {
      @include cq($value, $name) {
        @content;
      }
    }
  } @else {
    @warn "Le breakpoint de conteneur `#{$breakpoint}` n'existe pas.";
  }
}
```

---

## Exemples pratiques

### 1. Card responsive au conteneur

```scss
// composants/_card-cq.scss

.card-container {
  @include container-query(inline-size, card);
}

.card {
  display: flex;
  flex-direction: column;
  padding: $spacing-md;
  border: 1px solid $color-border;
  border-radius: $border-radius-lg;
  transition: box-shadow 0.3s ease;

  // Mobile : colonne par défaut
  &__header {
    display: flex;
    align-items: center;
    gap: $spacing-sm;
    margin-bottom: $spacing-sm;
  }

  &__image {
    width: 100%;
    aspect-ratio: 16/9;
    object-fit: cover;
    border-radius: $border-radius-md;
    margin-bottom: $spacing-md;
  }

  &__title {
    font-size: $font-size-lg;
    font-weight: $font-weight-bold;
  }

  &__text {
    color: $color-text-secondary;
    line-height: 1.6;
  }

  &__footer {
    display: flex;
    gap: $spacing-sm;
    margin-top: $spacing-md;
  }

  // Conteneur sm (320px+) : image à gauche
  @include cq(320px, card) {
    flex-direction: row;
    align-items: flex-start;

    .card__image {
      width: 120px;
      height: 120px;
      aspect-ratio: 1;
      flex-shrink: 0;
      margin-bottom: 0;
      margin-right: $spacing-md;
    }

    .card__content {
      flex: 1;
    }
  }

  // Conteneur md (480px+) : image plus grande
  @include cq(480px, card) {
    .card__image {
      width: 160px;
      height: 160px;
    }

    .card__title {
      font-size: $font-size-xl;
    }
  }

  // Conteneur lg (640px+) : layout horizontal complet
  @include cq(640px, card) {
    padding: $spacing-lg;

    .card__image {
      width: 200px;
      height: 150px;
    }

    .card__footer {
      margin-top: $spacing-lg;
    }
  }

  // Conteneur xl (800px+) : layout large
  @include cq(800px, card) {
    .card__image {
      width: 250px;
      height: 180px;
    }

    .card__title {
      font-size: $font-size-2xl;
    }
  }
}
```

### Résultat en HTML

```html
<div class="card-container">
  <div class="card">
    <img class="card__image" src="image.jpg" alt="...">
    <div class="card__content">
      <h3 class="card__title">Titre</h3>
      <p class="card__text">Description...</p>
      <div class="card__footer">
        <button class="btn btn--primary">Voir</button>
      </div>
    </div>
  </div>
</div>
```

### 2. Grid responsive au conteneur

```scss
// composants/_grid-cq.scss

.grid-container {
  @include container-query(inline-size, grid);
}

.grid {
  display: grid;
  gap: $spacing-md;

  // Par défaut : 1 colonne
  grid-template-columns: 1fr;

  // Conteneur sm
  @include cq(400px, grid) {
    grid-template-columns: repeat(2, 1fr);
  }

  // Conteneur md
  @include cq(600px, grid) {
    grid-template-columns: repeat(3, 1fr);
  }

  // Conteneur lg
  @include cq(800px, grid) {
    grid-template-columns: repeat(4, 1fr);
  }

  // Conteneur xl
  @include cq(1000px, grid) {
    grid-template-columns: repeat(5, 1fr);
  }

  &--items {
    .grid__item {
      background-color: $color-bg-secondary;
      padding: $spacing-md;
      border-radius: $border-radius-md;
    }
  }
}
```

### 3. Sidebar responsive

```scss
// layouts/_sidebar-cq.scss

.layout {
  display: grid;
  gap: $spacing-lg;
  min-height: 100vh;

  &__sidebar {
    @include container-query(inline-size, sidebar);
    background-color: $color-bg-secondary;
    padding: $spacing-md;
    border-radius: $border-radius-lg;
  }

  &__main {
    padding: $spacing-md;
  }
}

.sidebar-nav {
  list-style: none;
  padding: 0;

  &__item {
    margin-bottom: $spacing-xs;
  }

  &__link {
    display: block;
    padding: $spacing-sm $spacing-md;
    border-radius: $border-radius-md;
    color: $color-text;
    transition: background-color 0.2s ease;

    &:hover {
      background-color: $color-bg-tertiary;
    }

    &--active {
      background-color: $color-primary;
      color: white;
    }
  }

  // Sidebar étroite : icônes seulement
  @include cq-max(200px, sidebar) {
    &__link {
      display: flex;
      align-items: center;
      justify-content: center;
      padding: $spacing-sm;

      .sidebar-nav__text {
        display: none;
      }
    }
  }

  // Sidebar moyenne : texte raccourci
  @include cq-between(200px, 300px, sidebar) {
    &__link {
      font-size: $font-size-sm;
    }
  }

  // Sidebar large : layout complet
  @include cq(300px, sidebar) {
    &__link {
      display: flex;
      align-items: center;
      gap: $spacing-sm;
    }

    &__icon {
      flex-shrink: 0;
      width: 20px;
      height: 20px;
    }

    &__text {
      white-space: nowrap;
    }
  }
}
```

### 4. Header responsive

```scss
// layouts/_header-cq.scss

.header {
  @include container-query(inline-size, header);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: $spacing-sm $spacing-md;
  background-color: white;
  box-shadow: $shadow-sm;

  &__brand {
    font-size: $font-size-lg;
    font-weight: $font-weight-bold;
    color: $color-primary;
  }

  &__nav {
    display: flex;
    gap: $spacing-md;
  }

  &__toggle {
    display: block;
    padding: $spacing-sm;
    cursor: pointer;

    // Header large : cacher le toggle
    @include cq(600px, header) {
      display: none;
    }
  }

  // Header étroit : cacher la navigation
  @include cq-max(500px, header) {
    &__nav {
      display: none;

      &--open {
        display: flex;
        flex-direction: column;
        position: absolute;
        top: 100%;
        left: 0;
        right: 0;
        background-color: white;
        box-shadow: $shadow-md;
        padding: $spacing-md;
        z-index: 100;
      }
    }
  }

  // Header moyen : navigation compacte
  @include cq-between(500px, 800px, header) {
    &__nav {
      gap: $spacing-sm;
    }

    &__link {
      font-size: $font-size-sm;
    }
  }

  // Header large : navigation complète
  @include cq(800px, header) {
    &__nav {
      gap: $spacing-lg;
    }
  }
}
```

### 5. Pricing card

```scss
// composants/_pricing-cq.scss

.pricing-container {
  @include container-query(inline-size, pricing);
}

.pricing {
  text-align: center;
  padding: $spacing-lg;
  border: 2px solid $color-border;
  border-radius: $border-radius-xl;
  background-color: white;

  &__name {
    font-size: $font-size-xl;
    font-weight: $font-weight-bold;
    margin-bottom: $spacing-sm;
  }

  &__price {
    font-size: $font-size-4xl;
    font-weight: $font-weight-black;
    color: $color-primary;
    margin-bottom: $spacing-md;

    span {
      font-size: $font-size-md;
      font-weight: $font-weight-normal;
      color: $color-text-muted;
    }
  }

  &__features {
    list-style: none;
    padding: 0;
    margin-bottom: $spacing-lg;

    li {
      padding: $spacing-sm 0;
      border-bottom: 1px solid $color-border-light;

      &:last-child {
        border-bottom: none;
      }
    }
  }

  &__cta {
    width: 100%;
    padding: $spacing-md;
    font-size: $font-size-lg;
    font-weight: $font-weight-bold;
    border-radius: $border-radius-md;
    background-color: $color-primary;
    color: white;
    border: none;
    cursor: pointer;
  }

  // Conteneur sm : colonnes
  @include cq(400px, pricing) {
    display: flex;
    flex-direction: column;
    align-items: center;

    &__features {
      width: 100%;
      max-width: 300px;
    }
  }

  // Conteneur md : layout horizontal
  @include cq(600px, pricing) {
    flex-direction: row;
    text-align: left;
    align-items: center;
    gap: $spacing-xl;

    &__info {
      flex: 1;
    }

    &__features {
      flex: 1;
    }

    &__cta {
      width: auto;
      padding: $spacing-md $spacing-2xl;
    }
  }
}
```

---

## Container Query Units

Les unités de container query s'utilisent dans les conteneurs déclarés :

```scss
.card-container {
  container-type: inline-size;
}

.card {
  // cqi : 1% de la largeur du conteneur
  padding: 2cqi;

  // cqw : 1% de la largeur du conteneur
  font-size: clamp(1rem, 3cqw, 2rem);

  // cqh : 1% de la hauteur du conteneur
  min-height: 50cqh;

  // cqmin : min(cqi, cqh)
  // cqmax : max(cqi, cqh)
}

// Exemple pratique
.hero-card {
  container-type: inline-size;

  &__title {
    // Typographie fluide basée sur le conteneur
    font-size: clamp(1.5rem, 5cqi, 4rem);
    line-height: 1.1;
  }

  &__text {
    font-size: clamp(0.875rem, 2.5cqi, 1.25rem);
    max-width: 60cqi;
  }

  &__badge {
    font-size: 1cqi;
    padding: 0.5cqi 2cqi;
  }
}
```

---

## Container Queries + SCSS : Système complet

### Fichier de configuration

```scss
// config/_container-queries.scss

$container-query-breakpoints: (
  'xs': 0,
  'sm': 320px,
  'md': 480px,
  'lg': 640px,
  'xl': 800px,
  '2xl': 960px,
) !default;
```

### Fichier de mixins

```scss
// mixins/_container-queries.scss

@use '../config/container-queries' as *;

// Déclarer un conteneur
@mixin cq-init($name: null) {
  container-type: inline-size;

  @if $name {
    container-name: $name;
  }
}

// Query par breakpoint nommé
@mixin cq-at($breakpoint, $name: null) {
  $value: map-get($container-query-breakpoints, $breakpoint);

  @if $value == 0 {
    @content;
  } @else if $value {
    @container (min-width: $value) {
      @content;
    }
  } @else {
    @warn "Container breakpoint `#{$breakpoint}` introuvable.";
  }
}

// Query par valeur directe
@mixin cq-min($width) {
  @container (min-width: $width) {
    @content;
  }
}

@mixin cq-max($width) {
  @container (max-width: $width) {
    @content;
  }
}

// Query combinée
@mixin cq-range($min, $max) {
  @container (min-width: $min) and (max-width: $max) {
    @content;
  }
}
```

### Utilisation dans un composant

```scss
// components/_product-card.scss

@use '../mixins/container-queries' as *;

.product-card {
  @include cq-init(product);
  display: grid;
  gap: $spacing-md;
  padding: $spacing-md;
  border: 1px solid $color-border;
  border-radius: $border-radius-lg;

  // Par défaut : colonne
  &__image {
    aspect-ratio: 1;
    object-fit: cover;
    border-radius: $border-radius-md;
  }

  &__info {
    display: flex;
    flex-direction: column;
    gap: $spacing-xs;
  }

  &__title {
    font-size: $font-size-lg;
    font-weight: $font-weight-bold;
  }

  &__price {
    font-size: $font-size-2xl;
    font-weight: $font-weight-black;
    color: $color-primary;
  }

  &__actions {
    display: flex;
    gap: $spacing-sm;
    margin-top: $spacing-sm;
  }

  // sm : image plus grande
  @include cq-at('sm', product) {
    &__image {
      aspect-ratio: 4/3;
    }

    &__title {
      font-size: $font-size-xl;
    }
  }

  // md : layout horizontal
  @include cq-at('md', product) {
    grid-template-columns: 200px 1fr;

    &__image {
      aspect-ratio: auto;
      height: 100%;
      min-height: 150px;
    }

    &__info {
      justify-content: center;
    }
  }

  // lg : image plus large
  @include cq-at('lg', product) {
    grid-template-columns: 280px 1fr;
    padding: $spacing-lg;

    &__title {
      font-size: $font-size-2xl;
    }

    &__price {
      font-size: $font-size-3xl;
    }
  }

  // xl : layout premium
  @include cq-at('xl', product) {
    grid-template-columns: 350px 1fr;
    gap: $spacing-xl;

    &__actions {
      margin-top: $spacing-lg;
    }
  }
}
```

---

## Fallback pour navigateurs non supportés

```scss
// Mixin avec fallback
@mixin cq-fallback($breakpoint) {
  // Styles de base (fallback)
  @content;

  // Container query si supportée
  @supports (container-type: inline-size) {
    container-type: inline-size;

    @media (min-width: $breakpoint) {
      @content;
    }
  }
}

// Alternative : utiliser @container uniquement si supporté
@supports (container-type: inline-size) {
  .card-container {
    container-type: inline-size;
  }

  @container (min-width: 500px) {
    .card {
      flex-direction: row;
    }
  }
}
```

---

## Arbre des fichiers

```
sass/
├── config/
│   └── _container-queries.scss
├── mixins/
│   └── _container-queries.scss
├── components/
│   ├── _card-cq.scss
│   ├── _grid-cq.scss
│   ├── _pricing-cq.scss
│   └── _product-card.scss
├── layouts/
│   ├── _sidebar-cq.scss
│   └── _header-cq.scss
└── main.scss
```

---

## Compatibilité navigateurs

| Navigateur | Support |
|---|---|
| Chrome 105+ | Oui |
| Firefox 110+ | Oui |
| Safari 16+ | Oui |
| Edge 105+ | Oui |
| iOS Safari 16+ | Oui |
| Chrome Android 105+ | Oui |

> Toujours prévoir un fallback avec les media queries pour les navigateurs plus anciens.

---

## Résumé

Les container queries transforment le responsive design en rendant les composants véritablement modulaires. Avec des mixins SCSS bien conçus, on crée des composants qui s'adaptent à leur conteneur, pas à l'écran. C'est le futur du design responsive, et SCSS est l'outil parfait pour en faciliter l'usage.
