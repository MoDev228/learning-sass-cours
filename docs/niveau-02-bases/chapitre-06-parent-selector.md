# Chapitre 6 : Le parent selector (`&`) en Sass/SCSS

## Objectifs pédagogiques

À l'issue de ce chapitre, vous serez capable de :
- Utiliser le sélecteur `&` pour créer des variantes de classes
- Combiner `&` avec des pseudo-classes et pseudo-éléments
- Créer des préfixes et des suffixes avec `&`
- Maîtriser les combinaisons avancées du parent selector
- Appliquer `&` dans des cas d'utilisation complexes

---

## Introduction

Le sélecteur parent `&` est une référence au **sélecteur parent le plus proche** dans une imbrication Sass. Il est extrêmement polyvalent et constitue l'un des outils les plus utilisés dans le Sass moderne, notamment avec la méthodologie **BEM** (Block Element Modifier).

```scss
// Syntaxe de base
.parent {
  color: red;

  &__child {
    color: blue;
  }

  &:hover {
    color: green;
  }
}

// Résultat CSS :
// .parent { color: red; }
// .parent__child { color: blue; }
// .parent:hover { color: green; }
```

---

## 1. Exemples de base

### 1.1 Variantes de classes avec BEM

Le cas d'utilisation le plus courant de `&` est la création de classes BEM.

```scss
// BEM : Block Element Modifier
.button {
  padding: 12px 24px;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;

  // Element
  &__icon {
    margin-right: 8px;
    font-size: 1.2em;
  }

  &__text {
    display: inline-block;
  }

  // Modifier : taille
  &--small {
    padding: 8px 16px;
    font-size: 0.875rem;
  }

  &--large {
    padding: 16px 32px;
    font-size: 1.125rem;
  }

  // Modifier : couleur
  &--primary {
    background-color: #3498db;
    color: white;

    &:hover {
      background-color: #2980b9;
    }
  }

  &--secondary {
    background-color: #95a5a6;
    color: white;

    &:hover {
      background-color: #7f8c8d;
    }
  }

  &--danger {
    background-color: #e74c3c;
    color: white;

    &:hover {
      background-color: #c0392b;
    }
  }

  // Modifier : état
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    pointer-events: none;
  }

  &.is-loading {
    .button__text {
      visibility: hidden;
    }

    &::after {
      content: "";
      position: absolute;
      width: 20px;
      height: 20px;
      border: 2px solid white;
      border-top-color: transparent;
      border-radius: 50%;
      animation: spin 0.8s linear infinite;
    }
  }
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

**CSS compilé (extrait) :**

```css
.button {
  padding: 12px 24px;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.button__icon {
  margin-right: 8px;
  font-size: 1.2em;
}

.button__text {
  display: inline-block;
}

.button--small {
  padding: 8px 16px;
  font-size: 0.875rem;
}

.button--large {
  padding: 16px 32px;
  font-size: 1.125rem;
}

.button--primary {
  background-color: #3498db;
  color: white;
}

.button--primary:hover {
  background-color: #2980b9;
}

.button--secondary {
  background-color: #95a5a6;
  color: white;
}

.button--secondary:hover {
  background-color: #7f8c8d;
}

.button--danger {
  background-color: #e74c3c;
  color: white;
}

.button--danger:hover {
  background-color: #c0392b;
}

.button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
}

.button.is-loading .button__text {
  visibility: hidden;
}

.button.is-loading::after {
  content: "";
  position: absolute;
  width: 20px;
  height: 20px;
  border: 2px solid white;
  border-top-color: transparent;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}
```

### 1.2 Les pseudo-classes avec `&`

```scss
.link {
  color: #3498db;
  text-decoration: none;
  transition: color 0.3s ease;

  // État hover
  &:hover {
    color: #2980b9;
    text-decoration: underline;
  }

  // État visited
  &:visited {
    color: #8e44ad;
  }

  // État active (cliqué)
  &:active {
    color: #e74c3c;
  }

  // État focus (accessibilité)
  &:focus {
    outline: 2px solid #3498db;
    outline-offset: 2px;
  }

  // Combinaison focus-visible (navigateurs modernes)
  &:focus-visible {
    outline: 3px solid #3498db;
    outline-offset: 3px;
  }
}
```

**CSS compilé :**

```css
.link {
  color: #3498db;
  text-decoration: none;
  transition: color 0.3s ease;
}

.link:hover {
  color: #2980b9;
  text-decoration: underline;
}

.link:visited {
  color: #8e44ad;
}

.link:active {
  color: #e74c3c;
}

.link:focus {
  outline: 2px solid #3498db;
  outline-offset: 2px;
}

.link:focus-visible {
  outline: 3px solid #3498db;
  outline-offset: 3px;
}
```

### 1.3 Les pseudo-éléments avec `&`

```scss
.quote {
  position: relative;
  padding: 20px 30px;
  font-style: italic;
  border-left: 4px solid #3498db;

  // Guillemet d'ouverture
  &::before {
    content: "\201C";
    position: absolute;
    left: 10px;
    top: -5px;
    font-size: 3rem;
    color: #3498db;
    opacity: 0.3;
    font-style: normal;
  }

  // Guillemet de fermeture
  &::after {
    content: "\201D";
    position: absolute;
    right: 10px;
    bottom: -20px;
    font-size: 3rem;
    color: #3498db;
    opacity: 0.3;
    font-style: normal;
  }
}

.breadcrumb {
  display: flex;
  list-style: none;
  padding: 0;

  li {
    display: flex;
    align-items: center;

    // Séparateur entre les éléments
    &:not(:last-child)::after {
      content: "/";
      margin: 0 10px;
      color: #ccc;
    }

    a {
      color: #3498db;
      text-decoration: none;

      &:hover {
        text-decoration: underline;
      }
    }

    &.is-active {
      color: #333;
      font-weight: bold;

      // Point d'élément actif
      &::before {
        content: "•";
        margin-right: 8px;
        color: #3498db;
      }
    }
  }
}
```

---

## 2. Les suffixes avec `&`

### 2.1 Créer des classes en suffixe

Le `&` peut être utilisé pour créer des classes qui **commencent** par le nom du parent.

```scss
// Le sélecteur parent est utilisé comme PRÉFIXE
.card {
  padding: 20px;

  // Crée .card--primary, .card--dark, etc.
  &--primary {
    background: #3498db;
    color: white;
  }

  &--dark {
    background: #2c3e50;
    color: white;
  }

  &--outlined {
    border: 2px solid currentColor;
    background: transparent;
  }
}
```

### 2.2 Suffixes multiples

```scss
.form-field {
  margin-bottom: 16px;

  // Suffixe d'état
  &.is-required {
    .label {
      &::after {
        content: " *";
        color: red;
      }
    }
  }

  &.is-disabled {
    opacity: 0.5;

    input {
      pointer-events: none;
    }
  }

  &.has-error {
    input {
      border-color: red;
    }

    .error-message {
      display: block;
    }
  }

  &.has-success {
    input {
      border-color: green;
    }
  }
}
```

---

## 3. Les préfixes avec `&`

### 3.1 Créer des classes en préfixe

Inversement, on peut utiliser `&` pour créer des classes qui **finissent** par le nom du parent.

```scss
// Le sélecteur parent est utilisé comme SUFFIXE
// & précède le texte pour le placer AVANT le parent
.element {
  padding: 10px;

  // Crée .is-active.element, .is-hidden.element, etc.
  &.is-active {
    background: blue;
  }

  &.is-hidden {
    display: none;
  }

  &.is-loading {
    opacity: 0.6;
    pointer-events: none;
  }
}
```

### 3.2 Préfixe avec classes utilitaires

```scss
// Création de classes utilitaires avec préfixe
$spacing-values: (
  0: 0,
  1: 0.25rem,
  2: 0.5rem,
  3: 1rem,
  4: 1.5rem,
  5: 3rem
);

@each $name, $value in $spacing-values {
  .mt-#{$name} {
    margin-top: $value;
  }

  .mb-#{$name} {
    margin-bottom: $value;
  }

  .pt-#{$name} {
    padding-top: $value;
  }

  .pb-#{$name} {
    padding-bottom: $value;
  }
}

// Avec & pour créer des variantes contextuelles
.card {
  .mt-0 {
    margin-top: 0;
  }

  .mb-0 {
    margin-bottom: 0;
  }
}
```

---

## 4. Les combinaisons du parent selector (`&-combinators`)

### 4.1 Child combinator (`>`)

```scss
.nav {
  > ul {
    display: flex;
    list-style: none;
    margin: 0;
    padding: 0;

    > li {
      position: relative;

      > a {
        display: block;
        padding: 12px 20px;
        color: white;
        text-decoration: none;
        transition: background-color 0.3s;

        &:hover {
          background-color: rgba(255, 255, 255, 0.1);
        }
      }
    }
  }
}
```

**CSS compilé :**

```css
.nav > ul {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav > ul > li {
  position: relative;
}

.nav > ul > li > a {
  display: block;
  padding: 12px 20px;
  color: white;
  text-decoration: none;
  transition: background-color 0.3s;
}

.nav > ul > li > a:hover {
  background-color: rgba(255, 255, 255, 0.1);
}
```

### 4.2 Adjacent sibling combinator (`+`)

```scss
// Éléments adjacents (directement suivant)
.content {
  h1 {
    margin-bottom: 20px;

    // h1 suivi immédiatement de p
    & + p {
      font-size: 1.2rem;
      color: #555;
      margin-top: 0;
    }
  }

  h2 {
    margin-top: 40px;
    margin-bottom: 15px;

    // h2 suivi immédiatement de ul
    & + ul {
      padding-left: 20px;
    }
  }

  p {
    line-height: 1.6;

    // p suivi immédiatement de p
    & + p {
      margin-top: 16px;
    }
  }
}

// Résultat CSS :
// .content h1 { ... }
// .content h1 + p { ... }
// .content h2 { ... }
// .content h2 + ul { ... }
// .content p { ... }
// .content p + p { ... }
```

### 4.3 General sibling combinator (`~`)

```scss
// Éléments siblings (pas nécessairement adjacents)
.form-group {
  margin-bottom: 20px;

  // Tous les .form-group qui suivent un autre .form-group
  & + & {
    padding-top: 20px;
    border-top: 1px solid #eee;
  }
}

// Exemple plus complexe
.tabs {
  &__nav {
    display: flex;
    border-bottom: 2px solid #eee;

    .tab {
      padding: 12px 24px;
      cursor: pointer;
      border-bottom: 2px solid transparent;
      margin-bottom: -2px;
      transition: all 0.3s;

      &.is-active {
        border-bottom-color: #3498db;
        color: #3498db;
        font-weight: bold;
      }

      &:hover:not(.is-active) {
        border-bottom-color: #ccc;
      }
    }
  }

  &__content {
    padding: 20px 0;

    .tab-panel {
      display: none;

      &.is-active {
        display: block;
        animation: fadeIn 0.3s ease;
      }
    }
  }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
```

### 4.4 Combinaisons multiples

```scss
// Combiner & avec différents sélecteurs
.card {
  padding: 20px;
  transition: all 0.3s;

  // Descendant
  .title {
    font-size: 1.5rem;
    font-weight: bold;
  }

  // Child
  > .content {
    padding: 10px 0;
  }

  // Adjacent sibling (dans un contexte)
  & + .card {
    margin-top: 20px;
  }

  // General sibling
  & ~ .card {
    border-top: 1px solid #eee;
  }

  // Pseudo-classes
  &:hover {
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  }

  &:first-child {
    border-radius: 8px 8px 0 0;
  }

  &:last-child {
    border-radius: 0 0 8px 8px;
  }

  &:nth-child(even) {
    background-color: #f8f9fa;
  }

  // Pseudo-éléments
  &::before {
    content: "";
    display: block;
    width: 40px;
    height: 3px;
    background: #3498db;
    margin-bottom: 15px;
  }
}
```

---

## 5. Cas d'utilisation avancés

### 5.1 Thème avec `&`

```scss
$themes: (
  light: (
    bg: #ffffff,
    text: #333333,
    border: #dee2e6,
    primary: #3498db
  ),
  dark: (
    bg: #1a1a2e,
    text: #e0e0e0,
    border: #444444,
    primary: #5dade2
  )
);

@each $name, $theme in $themes {
  .theme-#{$name} {
    background-color: map-get($theme, bg);
    color: map-get($theme, text);

    .card {
      background-color: lighten(map-get($theme, bg), 5%);
      border: 1px solid map-get($theme, border);

      &__title {
        color: map-get($theme, primary);
      }

      &__text {
        color: map-get($theme, text);
      }

      &:hover {
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
      }
    }

    .link {
      color: map-get($theme, primary);

      &:hover {
        color: lighten(map-get($theme, primary), 10%);
      }
    }
  }
}
```

### 5.2 Composant Accordion

```scss
.accordion {
  border: 1px solid #dee2e6;
  border-radius: 8px;
  overflow: hidden;

  &__item {
    border-bottom: 1px solid #dee2e6;

    &:last-child {
      border-bottom: none;
    }

    // Quand l'item est ouvert
    &.is-open {
      .accordion__header {
        background-color: #f8f9fa;
      }

      .accordion__icon {
        transform: rotate(180deg);
      }

      .accordion__body {
        max-height: 500px;
        padding: 20px;
      }
    }
  }

  &__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px 20px;
    cursor: pointer;
    transition: background-color 0.3s;
    user-select: none;

    &:hover {
      background-color: #f8f9fa;
    }
  }

  &__title {
    font-weight: 600;
    font-size: 1.1rem;
    margin: 0;
  }

  &__icon {
    font-size: 0.8rem;
    transition: transform 0.3s ease;
    color: #666;
  }

  &__body {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.3s ease, padding 0.3s ease;
    padding: 0 20px;
    color: #555;
    line-height: 1.6;
  }
}
```

### 5.3 Menu avec sous-menus

```scss
.dropdown {
  position: relative;
  display: inline-block;

  // Le menu déroulant
  &__menu {
    position: absolute;
    top: 100%;
    left: 0;
    min-width: 200px;
    background: white;
    border: 1px solid #dee2e6;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    padding: 8px 0;
    opacity: 0;
    visibility: hidden;
    transform: translateY(-10px);
    transition: all 0.3s ease;
    z-index: 1000;
  }

  // Affichage du menu au hover
  &:hover > &__menu,
  &.is-open > &__menu {
    opacity: 1;
    visibility: visible;
    transform: translateY(0);
  }

  &__item {
    // Chaque élément du menu
    a {
      display: block;
      padding: 10px 20px;
      color: #333;
      text-decoration: none;
      transition: all 0.2s;

      &:hover {
        background-color: #f8f9fa;
        color: #3498db;
      }
    }

    // Élément avec sous-menu
    &.has-submenu {
      position: relative;

      &::after {
        content: "▸";
        position: absolute;
        right: 15px;
        top: 50%;
        transform: translateY(-50%);
        font-size: 0.8rem;
        color: #999;
      }

      // Sous-menu
      > .dropdown__menu {
        left: 100%;
        top: 0;
        margin-left: 8px;
      }
    }

    // Élément actif
    &.is-active > a {
      color: #3498db;
      font-weight: 600;
    }

    // Séparateur
    &.is-divider {
      height: 1px;
      background: #eee;
      margin: 8px 0;
      padding: 0;
    }
  }

  // Taille du dropdown
  &--sm > &__menu {
    min-width: 150px;
  }

  &--lg > &__menu {
    min-width: 300px;
  }

  // Alignement
  &--right > &__menu {
    left: auto;
    right: 0;
  }

  &--center > &__menu {
    left: 50%;
    transform: translateX(-50%) translateY(-10px);

    &:hover,
    &.is-open {
      transform: translateX(-50%) translateY(0);
    }
  }
}
```

### 5.4 Tableau de bord (Dashboard)

```scss
.dashboard {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 24px;
  padding: 24px;

  &__card {
    background: white;
    border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
    overflow: hidden;
    transition: transform 0.3s, box-shadow 0.3s;

    &:hover {
      transform: translateY(-4px);
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
    }

    // Variantes de cartes
    &--stats {
      .dashboard__card-header {
        background: linear-gradient(135deg, #667eea, #764ba2);
        color: white;
      }
    }

    &--chart {
      .dashboard__card-body {
        padding: 16px;
      }
    }

    &--full {
      grid-column: 1 / -1;
    }
  }

  &__card-header {
    padding: 20px;
    border-bottom: 1px solid #eee;

    h3 {
      margin: 0;
      font-size: 1.1rem;
      font-weight: 600;
    }
  }

  &__card-body {
    padding: 24px;
  }

  &__stat {
    display: flex;
    justify-content: space-between;
    align-items: center;

    &-value {
      font-size: 2.5rem;
      font-weight: bold;
      line-height: 1;
    }

    &-label {
      font-size: 0.9rem;
      color: #666;
      margin-top: 4px;
    }

    &-change {
      font-size: 0.85rem;
      padding: 4px 8px;
      border-radius: 20px;

      &.is-positive {
        background: #d4edda;
        color: #155724;
      }

      &.is-negative {
        background: #f8d7da;
        color: #721c24;
      }
    }
  }
}
```

---

## 6. Pièges courants et comment les éviter

### 6.1 `&` dans un contexte non imbriqué

```scss
// ⚠️ Ceci est un piège courant
.element {
  color: red;

  // Ceci ne CRÉE PAS un nouveau sélecteur
  // & fait référence au parent, donc ça crée ".element" à nouveau
  & {
    padding: 10px;
  }
}

// ✅ Pour créer un sélecteur qui n'est pas un descendant
// Utilisez plutôt des sélecteurs séparés
.element {
  color: red;
}

.another-element {
  padding: 10px;
}
```

### 6.2 Multiples `&` dans un même bloc

```scss
// ✅ Utilisation correcte de plusieurs & dans un même bloc
.card {
  // Crée à la fois .card:hover ET .card:focus
  &:hover,
  &:focus {
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  }
}

// ✅ Autre exemple
.button {
  // Cible .button.btn-primary ET .button.btn-secondary
  &.btn-primary,
  &.btn-secondary {
    padding: 10px 20px;
  }
}
```

### 6.3 `&` avec des sélecteurs composés

```scss
// & peut contenir des sélecteurs complets
.element {
  // Crée ".element.active" et ".element.visible"
  &.active.visible {
    display: block;
  }

  // Crée ".dark .element"
  .dark & {
    color: white;
  }

  // Crée ":root .element"
  :root & {
    --spacing: 20px;
  }
}
```

---

## 7. Exercices pratiques

### Exercice 1 : Composant BEM

Créez un composant `.notification` avec les éléments et modifiers BEM suivants :

```scss
// Éléments :
// - __icon (icône à gauche)
// - __content (contenu principal)
//   - __title (titre gras)
//   - __message (texte)
// - __close (bouton fermer à droite)

// Modifiers :
// --info (fond bleu clair)
// --success (fond vert clair)
// --warning (fond orange clair)
// --error (fond rouge clair)

// États :
// &:hover (légère ombre)
// &.is-dismissible (le bouton close est visible)
// &.is-static (pas d'animation)
```

### Exercice 2 : Pseudo-classes

```scss
// Créez un composant .input avec :
// - :focus (bordure bleue, ombre légère)
// - :focus-visible (bordure plus épaisse)
// - :disabled (grisé, curseur interdit)
// - :read-only (fond légèrement gris)
// - :invalid (bordure rouge)
// - :valid (bordure verte)
// - + .input-hint (texte d'aide en dessous)
// - + .input-error (message d'erreur en rouge)
```

### Exercice 3 : Pseudo-éléments

```scss
// Créez un composant .badge avec :
// - &::before (point colored à gauche)
// - &::after (compteur ou texte à droite)
// - Chaque badge a une variante de couleur (--primary, --danger, --success)
// - Les pseudo-éléments changent de couleur avec le parent
```

### Exercise 4 : Combinators

```scss
// Créez un布局 avec :
// - .sidebar + .main-content (margin-left: 250px)
// - .section > .section-title (font-size: 1.5rem)
// - .list > .list-item + .list-item (border-top: 1px solid #eee)
// - .grid > .grid-item:hover (z-index: 10, ombre)
```

### Exercice 5 : Menu complet

```scss
// Créez un menu avec sous-menus utilisant & :
// - .menu (list-style: none, padding: 0)
// - .menu__item (position: relative)
//   - .menu__link (padding, color, text-decoration)
//   - .menu__link:hover (background color)
//   - .menu__link.is-active (font-weight: bold, border-bottom)
//   - .menu__submenu (position: absolute, hidden by default)
//   - .menu__item:hover > .menu__submenu (visible)
//   - &.has-mega-menu (largeur pleine)
//     - .menu__mega-content (grid layout)
```

### Exercice 6 : Dashboard

```scss
// Créez un dashboard avec & :
// - .dashboard (grid layout)
// - .dashboard__card (white bg, shadow, border-radius)
//   - &:hover (shadow plus forte)
//   - &--featured (border-left: 4px solid blue)
//   - &--compact (padding réduit)
// - .dashboard__card-header
//   - &.is-collapsible (cursor pointer)
//     - &::after (chevron icon)
// - .dashboard__card-body
//   - .stat + .stat (border-top separator)
//   - &.is-loading (animation pulse)
```

---

## Résumé

| Utilisation | Syntaxe | Résultat |
|-------------|---------|----------|
| BEM Element | `&__child` | `.parent__child` |
| BEM Modifier | `&--mod` | `.parent--mod` |
| Pseudo-classe | `&:hover` | `.parent:hover` |
| Pseudo-élément | `&::before` | `.parent::before` |
| Child combinator | `> .child` | `.parent > .child` |
| Adjacent sibling | `& + &` | `.parent + .parent` |
| General sibling | `& ~ &` | `.parent ~ .parent` |
| Classe conditionnelle | `.is-active &` | `.is-active .parent` |
| Sélecteur multiple | `&.class` | `.parent.class` |

---

## Chapitre suivant

Dans le [Chapitre 7 - Les commentaires](chapitre-07-commentaires.md), nous verrons comment documenter votre code Sass efficacement avec les différents types de commentaires disponibles.
