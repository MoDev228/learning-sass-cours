# Chapitre 34 — Bonnes Pratiques SCSS

## Introduction

Écrire du SCSS propre, maintenable et performant demande de suivre des conventions et des bonnes pratiques éprouvées. Ce chapitre couvre les conventions de nommage, l'organisation, l'architecture, la lisibilité, et l'utilisation judicieuse de SCSS avec BEM.

---

## Quand utiliser SCSS ?

### Scénarios où SCSS est idéal

| Cas | Pourquoi SCSS |
|---|---|
| Design System | Variables, mixins, maps pour des tokens réutilisables |
| Grand projet | Architecture 7-1/ITCSS pour maintenabilité |
| Composants complexes | Nesting intelligent, variants via classes |
| Thèmes multiples | Variables et Custom Properties |
| Équipe grande | Conventions standardisées |
| Animations | Keyframes + variables pour timing |
| Responsive avancé | Mixins de media queries |

### Scénarios où SCSS n'est pas nécessaire

| Cas | Alternative |
|---|---|
| Petites landing pages | CSS vanilla suffit |
| Composants isolés (Web Components) | CSS Modules ou Scoped CSS |
| Projets avec CSS-in-JS | Styled Components, Emotion |
| Sites très simples | Pas besoin de compilation |
| Performance critique (minimal CSS) | CSS inline ou CSS natif |

---

## Conventions de nommage

### Variables

```scss
// MAUVAIS
$bg: #fff;
$bd: 1px solid #ccc;
$fs: 16px;
$pd: 16px;

// BON : descriptif et complet
$color-background: #ffffff;
$border-default: 1px solid $color-border;
$font-size-base: 16px;
$spacing-md: 16px;

// OU avec un préfixe de projet
$myapp-color-primary: #3498db;
$myapp-spacing-md: 16px;
```

### Mixins

```scss
// MAUVAIS
@mixin flex { }
@mixin m($p) { }
@mixin respond($bp) { }

// BON : descriptif
@mixin flex-center { }
@mixin padding-responsive($property) { }
@mixin respond-to($breakpoint) { }
```

### Classes CSS

```scss
// MAUVAIS
.card { }
.card .title { }
.card .text { }
.card.active { }

// BEM :.Block__Element--Modifier
.card { }
.card__title { }
.card__text { }
.card--featured { }
.card--dark { }
.card__title--large { }
```

### Fichiers

```
// MAUVAIS
_button-styles.scss
btn.scss
_buttonComponent.scss
button_styles.scss

// BON : un mot, kebab-case
_buttons.scss
_button-group.scss
_button-icon.scss
```

---

## Architecture guidelines

### Structure de dossiers

```
src/
├── abstracts/       ← Variables, mixins, fonctions (pas de CSS)
├── base/            ← Styles globaux, reset, typographie
├── components/      ← Composants UI réutilisables
├── layout/          ← Structure de la page
├── pages/           ← Styles spécifiques à une page
├── themes/          ← Thèmes visuels
├── utilities/       ← Classes d'aide
└── main.scss        ← Point d'entrée unique
```

### Règles d'organisation

```scss
// 1. Un composant = un fichier
// buttons.scss contient TOUT ce qui concerne les boutons
.btn { }
.btn--primary { }
.btn--sm { }
.btn--lg { }
.btn-group { }
.btn-icon { }

// 2. Pas de mélange de responsabilités
// MAUVAIS : button.scss contient aussi .card
// BON : card.scss pour .card

// 3. Les variables partagées vont dans abstracts
// MAUVAIS : $color-primary dans buttons.scss
// BON : $color-primary dans abstracts/_variables.scss

// 4. Un seul fichier d'entrée
// MAUVAIS : import de chaque fichier dans le HTML
// BON : main.scss importe tout
```

### Imports

```scss
// Mode moderne (@use/@forward) - RECOMMANDÉ

// abstracts/_index.scss
@forward 'variables';
@forward 'functions';
@forward 'mixins';

// main.scss
@use 'abstracts';
@use 'base';
@use 'components';

// Mode legacy (@import) - À ÉVITER
@import 'abstracts/variables';
@import 'abstracts/mixins';
// Les @import polluent le namespace global
```

---

## Nesting : la règle des 3 niveaux

### Profondeur maximale

```scss
// MAUVAIS : 5 niveaux de nesting
.page {
  .header {
    .nav {
      .nav-item {
        .nav-link {
          color: $color-primary;
          &:hover {
            color: darken($color-primary, 10%);
          }
        }
      }
    }
  }
}

// BON : 2-3 niveaux max
.nav-link {
  color: $color-primary;

  &:hover {
    color: darken($color-primary, 10%);
  }

  &--active {
    color: $color-primary-dark;
  }
}
```

### Alternatives au nesting profond

```scss
// MAUVAIS
.sidebar {
  .widget {
    .title {
      font-size: 18px;
      .icon {
        margin-right: 8px;
      }
    }
  }
}

// BON : sélecteurs plats avec BEM
.sidebar__widget-title {
  font-size: 18px;
}

.sidebar__widget-icon {
  margin-right: 8px;
}

// OU : nesting limité
.sidebar {
  &__widget {
    &-title {
      font-size: 18px;
    }

    &-icon {
      margin-right: 8px;
    }
  }
}
```

### Quand le nesting est utile

```scss
// 1. États d'un élément
.btn {
  background: $color-primary;

  &:hover {
    background: darken($color-primary, 10%);
  }

  &:active {
    background: darken($color-primary, 20%);
  }

  &:disabled {
    opacity: 0.5;
  }

  &--secondary {
    background: $color-secondary;
  }
}

// 2. Modificateurs d'un composant
.card {
  padding: $spacing-md;

  &--dark {
    background: $color-dark;
    color: white;
  }

  &--flat {
    box-shadow: none;
    border: 1px solid $color-border;
  }

  &__title {
    font-size: $font-size-lg;
  }

  &__body {
    line-height: 1.6;
  }
}

// 3. Contexte parent
.header {
  background: white;

  .nav {
    display: flex;
  }
}

// 4. Media queries ciblées
.sidebar {
  width: 100%;

  @include respond-to('md') {
    width: 300px;
  }
}
```

---

## BEM avec SCSS

### Structure BEM complète

```scss
// Composant : Block
.block {
  // Styles de base du bloc

  // Element
  &__element {
    // Styles de l'élément

    // Modifier de l'élément
    &--modifier {
      // Styles modifiés
    }
  }

  // Modifier du bloc
  &--modifier {
    // Styles modifiés du bloc

    // Élément dans le contexte du modifier
    .block__element {
      // Styles spécifiques
    }
  }
}
```

### Exemple complet : Card

```scss
// components/_card.scss

.card {
  background-color: $color-white;
  border-radius: $border-radius-lg;
  box-shadow: $shadow-md;
  overflow: hidden;
  transition: box-shadow 0.3s ease, transform 0.3s ease;

  &:hover {
    box-shadow: $shadow-lg;
    transform: translateY(-2px);
  }

  // Element : Image
  &__image {
    width: 100%;
    aspect-ratio: 16 / 9;
    object-fit: cover;
  }

  // Element : Contenu
  &__content {
    padding: $spacing-md;
  }

  // Element : Titre
  &__title {
    font-size: $font-size-lg;
    font-weight: $font-weight-bold;
    margin-bottom: $spacing-xs;
    color: $color-text;
  }

  // Element : Sous-titre
  &__subtitle {
    font-size: $font-size-sm;
    color: $color-text-secondary;
    margin-bottom: $spacing-sm;
  }

  // Element : Texte
  &__text {
    color: $color-text;
    line-height: $line-height-relaxed;
    margin-bottom: $spacing-md;
  }

  // Element : Pied de page
  &__footer {
    padding: $spacing-sm $spacing-md;
    border-top: 1px solid $color-border;
    display: flex;
    justify-content: flex-end;
    gap: $spacing-sm;
  }

  // MODIFIER : Horizontal
  &--horizontal {
    display: flex;
    flex-direction: row;

    .card__image {
      width: 200px;
      height: auto;
      aspect-ratio: auto;
      flex-shrink: 0;
    }

    .card__content {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
    }
  }

  // MODIFIER : Sans bordure
  &--flat {
    box-shadow: none;
    border: 1px solid $color-border;

    &:hover {
      box-shadow: none;
      border-color: $color-primary;
    }
  }

  // MODIFIER : Sombre
  &--dark {
    background-color: $color-dark;
    color: $color-white;

    .card__title {
      color: $color-white;
    }

    .card__subtitle {
      color: $color-gray-400;
    }

    .card__text {
      color: $color-gray-300;
    }

    .card__footer {
      border-top-color: $color-gray-700;
    }
  }

  // MODIFIER : Clickable
  &--clickable {
    cursor: pointer;

    &:focus-visible {
      outline: 2px solid $color-primary;
      outline-offset: 2px;
    }
  }
}
```

### Exemple : Formulaire avec BEM

```scss
// components/_form.scss

.form {
  // Block

  &__group {
    margin-bottom: $spacing-md;
  }

  &__label {
    display: block;
    font-weight: $font-weight-medium;
    margin-bottom: $spacing-xs;
    color: $color-text;
  }

  &__required {
    color: $color-danger;
    margin-left: 2px;
  }

  &__input {
    width: 100%;
    padding: $spacing-sm $spacing-md;
    font-size: $font-size-base;
    border: 2px solid $color-border;
    border-radius: $border-radius-md;
    background-color: $color-white;
    color: $color-text;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;

    &:hover {
      border-color: $color-border-hover;
    }

    &:focus {
      outline: none;
      border-color: $color-primary;
      box-shadow: 0 0 0 3px rgba($color-primary, 0.15);
    }

    &::placeholder {
      color: $color-text-muted;
    }

    // Modifier : Erreur
    &--error {
      border-color: $color-danger;

      &:focus {
        box-shadow: 0 0 0 3px rgba($color-danger, 0.15);
      }
    }

    // Modifier : Succès
    &--success {
      border-color: $color-secondary;

      &:focus {
        box-shadow: 0 0 0 3px rgba($color-secondary, 0.15);
      }
    }
  }

  &__textarea {
    @extend .form__input;
    min-height: 120px;
    resize: vertical;
  }

  &__select {
    @extend .form__input;
    appearance: none;
    background-image: url("data:image/svg+xml,...");
    background-repeat: no-repeat;
    background-position: right 12px center;
    padding-right: $spacing-xl;
  }

  &__checkbox,
  &__radio {
    display: flex;
    align-items: center;
    gap: $spacing-sm;
    cursor: pointer;

    input {
      width: 18px;
      height: 18px;
      accent-color: $color-primary;
    }
  }

  &__help {
    font-size: $font-size-sm;
    color: $color-text-secondary;
    margin-top: $spacing-xs;
  }

  &__error-message {
    font-size: $font-size-sm;
    color: $color-danger;
    margin-top: $spacing-xs;
    display: flex;
    align-items: center;
    gap: 4px;
  }

  &__row {
    display: grid;
    gap: $spacing-md;

    @include respond-to('md') {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  &__actions {
    display: flex;
    gap: $spacing-md;
    margin-top: $spacing-lg;

    @include respond-to('md') {
      justify-content: flex-end;
    }
  }
}
```

---

## Lisibilité du code

### Organisation dans un fichier

```scss
// 1. Imports
@use '../abstracts' as *;

// 2. Variables locales (si besoin)
$component-max-width: 800px;

// 3. Styles de base du composant
.component {
  // 4. Propriétés de layout
  display: flex;
  flex-direction: column;
  max-width: $component-max-width;
  margin: 0 auto;

  // 5. Éléments
  &__header {
    padding: $spacing-md;
    border-bottom: 1px solid $color-border;
  }

  &__body {
    padding: $spacing-lg;
    flex: 1;
  }

  &__footer {
    padding: $spacing-md;
    border-top: 1px solid $color-border;
  }

  // 6. Modificateurs
  &--compact {
    .component__header,
    .component__footer {
      padding: $spacing-sm;
    }
  }

  // 7. États
  &--loading {
    opacity: 0.6;
    pointer-events: none;
  }

  &--hidden {
    display: none;
  }

  // 8. Media queries
  @include respond-to('md') {
    flex-direction: row;
  }
}
```

### Espacement et organisation

```scss
// MAUVAIS : pas d'espacement, tout collé
.card{background:white;padding:16px;border-radius:8px;box-shadow:0 2px 4px rgba(0,0,0,0.1)}
.card__title{font-size:18px;font-weight:700;margin-bottom:8px}
.card__text{color:#666;line-height:1.5}

// BON : espacement et commentaires si nécessaire
.card {
  background: $color-white;
  padding: $spacing-md;
  border-radius: $border-radius-lg;
  box-shadow: $shadow-md;

  // Titre de la carte
  &__title {
    font-size: $font-size-lg;
    font-weight: $font-weight-bold;
    margin-bottom: $spacing-sm;
  }

  // Texte descriptif
  &__text {
    color: $color-text-secondary;
    line-height: $line-height-relaxed;
  }
}
```

---

## Maintenance du code

### Documentation

```scss
// components/_tooltip.scss
//
// Composant Tooltip
// =================
//
// Utilisation :
// <div class="tooltip" data-tooltip="Texte du tooltip">
//   <span class="tooltip__trigger">Element</span>
//   <span class="tooltip__content">Texte du tooltip</span>
// </div>
//
// Modificateurs :
// .tooltip--top    Position en haut (défaut)
// .tooltip--right  Position à droite
// .tooltip--bottom Position en bas
// .tooltip--left   Position à gauche
//
// Variables :
// $tooltip-bg          Couleur de fond (défaut: $color-dark)
// $tooltip-color       Couleur du texte (défaut: white)
// $tooltip-max-width   Largeur max (défaut: 250px)
// $tooltip-arrow-size  Taille de la flèche (défaut: 6px)

$tooltip-bg: $color-dark !default;
$tooltip-color: $color-white !default;
$tooltip-max-width: 250px !default;
$tooltip-arrow-size: 6px !default;

.tooltip {
  position: relative;
  display: inline-block;

  &__content {
    position: absolute;
    z-index: $z-tooltip;
    background-color: $tooltip-bg;
    color: $tooltip-color;
    padding: $spacing-xs $spacing-sm;
    border-radius: $border-radius-md;
    font-size: $font-size-sm;
    line-height: $line-height-tight;
    max-width: $tooltip-max-width;
    white-space: nowrap;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.2s ease, visibility 0.2s ease;
    pointer-events: none;

    // Flèche
    &::after {
      content: '';
      position: absolute;
      border: $tooltip-arrow-size solid transparent;
    }
  }

  // Affichage au hover
  &:hover &__content,
  &:focus-within &__content {
    opacity: 1;
    visibility: visible;
  }

  // Positions
  &--top &__content {
    bottom: calc(100% + #{$tooltip-arrow-size});
    left: 50%;
    transform: translateX(-50%);

    &::after {
      top: 100%;
      left: 50%;
      transform: translateX(-50%);
      border-top-color: $tooltip-bg;
    }
  }

  &--bottom &__content {
    top: calc(100% + #{$tooltip-arrow-size});
    left: 50%;
    transform: translateX(-50%);

    &::after {
      bottom: 100%;
      left: 50%;
      transform: translateX(-50%);
      border-bottom-color: $tooltip-bg;
    }
  }

  &--left &__content {
    right: calc(100% + #{$tooltip-arrow-size});
    top: 50%;
    transform: translateY(-50%);

    &::after {
      left: 100%;
      top: 50%;
      transform: translateY(-50%);
      border-left-color: $tooltip-bg;
    }
  }

  &--right &__content {
    left: calc(100% + #{$tooltip-arrow-size});
    top: 50%;
    transform: translateY(-50%);

    &::after {
      right: 100%;
      top: 50%;
      transform: translateY(-50%);
      border-right-color: $tooltip-bg;
    }
  }
}
```

### Convention de commentaires

```scss
// === SECTION PRINCIPALE ===
// Description de la section

// --- Sous-section ---
// Description de la sous-section

// * Note importante
// TODO: à améliorer
// FIXME: bug connu
// HACK: solution temporaire
```

---

## Checklist de qualité

### Avant de valider

```markdown
## Checklist SCSS

### Nommage
- [ ] Variables nommées de façon descriptive
- [ ] Mixins avec des noms clairs
- [ ] Classes en BEM (Block__Element--Modifier)
- [ ] Fichiers en kebab-case

### Structure
- [ ] Un composant = un fichier
- [ ] Architecture 7-1 ou ITCSS respectée
- [ ] Imports dans le bon ordre
- [ ] Pas de CSS dans abstracts/

### Qualité
- [ ] Nesting limité à 3 niveaux
- [ ] Pas de !important inutile
- [ ] Pas de sélecteurs trop spécifiques
- [ ] Variables partagées dans abstracts/

### Performance
- [ ] Pas de boucles lourdes
- [ ] Pas de récursion profonde
- [ ] Opérations optimisées
- [ ] CSS final compressé

### Maintenabilité
- [ ] Code commenté si nécessaire
- [ ] Variables configurables (!default)
- [ ] Pas de code mort
- [ ] Compatible avec les outils de build
```

---

## Résumé

Les bonnes pratiques SCSS tournent autour de trois piliers : **nommage clair** (BEM, descriptif), **structure organisée** (7-1, un composant = un fichier), et **lisibilité** (nesting limité, commentaires utiles). En suivant ces conventions, votre code SCSS reste maintenable, performant et agréable à travailler en équipe.
