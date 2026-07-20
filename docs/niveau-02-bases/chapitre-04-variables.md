# Chapitre 4 : Les Variables en Sass/SCSS

## Objectifs pédagogiques

À l'issue de ce chapitre, vous serez capable de :
- Déclarer et utiliser des variables dans vos fichiers Sass/SCSS
- Comprendre les différents types de données supportés
- Maîtriser l'interpolation des variables
- Distinguer les variables globales et locales
- Utiliser les valeurs par défaut avec `!default`

---

## Introduction

Les variables sont l'un des fondamentaux de tout langage de programmation, et Sass ne fait pas exception. Une variable est un **conteneur nommé** qui stocke une valeur que vous pouvez réutiliser tout au long de votre feuille de style. Elles permettent d'éviter la répétition, de centraliser les valeurs et de rendre le code plus facile à maintenir.

En Sass/SCSS, les variables sont précédées du symbole **`$`**.

---

## 1. Déclaration simple d'une variable

### Syntaxe de base

```scss
// Déclaration d'une variable
$ma-variable: valeur;
```

### Exemple concret

```scss
$primary-color: #3498db;
$font-size-base: 16px;
$border-radius: 8px;

h1 {
  color: $primary-color;
  font-size: $font-size-base;
}

.card {
  border: 1px solid darken($primary-color, 10%);
  border-radius: $border-radius;
  padding: 1rem;
}
```

**Résultat CSS compilé :**

```css
h1 {
  color: #3498db;
  font-size: 16px;
}

.card {
  border: 1px solid #2980b9;
  border-radius: 8px;
  padding: 1rem;
}
```

> **Note importante :** En Sass, une variable est définie lorsqu'elle est **assignée**, pas simplement déclarée. L'assignation crée la variable si elle n'existe pas déjà.

---

## 2. Les différents types de données

Sass supporte plusieurs types de données pour les variables. Voyons chacun d'entre eux en détail.

### 2.1 Les couleurs

Les couleurs peuvent être exprimées en différentes notations.

```scss
// Couleurs nommées
$text-color: black;
$highlight-color: tomato;
$transparent-overlay: rgba(0, 0, 0, 0.5);

// Notation hexadécimale
$brand-primary: #2ecc71;
$brand-secondary: #1abc9c;
$brand-short-hex: #fff;

// Notation rgb()
$primary-rgb: rgb(52, 152, 219);
$primary-rgb-alpha: rgba(52, 152, 219, 0.8);

// Notation hsl()
$primary-hsl: hsl(204, 70%, 53%);
$primary-hsl-alpha: hsla(204, 70%, 53%, 0.9);

// Utilisation
body {
  background-color: $highlight-color;
  color: $text-color;
}

.overlay {
  background-color: $transparent-overlay;
}

.card {
  border-color: $primary-rgb;
  box-shadow: 0 2px 4px $primary-rgb-alpha;
}
```

### 2.2 Les nombres

Les nombres peuvent être entiers, décimaux, avec ou sans unité.

```scss
// Entiers
$spacing-small: 8px;
$spacing-medium: 16px;
$spacing-large: 32px;
$spacing-xl: 48px;

// Décimaux
$font-size-small: 0.875rem;
$line-height-tight: 1.2;
$opacity-full: 1;
$opacity-half: 0.5;

// Sans unité (unitless)
$z-index-dropdown: 100;
$grid-columns: 12;
$multiplier: 2;

// Avec différentes unités
$width-percentage: 50%;
$width-viewport: 80vw;
$font-size-pixels: 14px;
$font-size-rem: 1.125rem;

.container {
  width: 50%;
  max-width: 1200px;
  margin: 0 auto;
}

.grid-item {
  width: calc(100% / $grid-columns);
  padding: $spacing-small;
  font-size: $font-size-small;
  line-height: $line-height-tight;
}
```

### 2.3 Les chaînes de caractères (Strings)

```scss
// Chaînes avec guillemets
$font-family-heading: "Montserrat", sans-serif;
$font-family-body: "Open Sans", Arial, sans-serif;
$content-before: "→";

// Chaînes sans guillemets
$font-stack: Helvetica, Arial, sans-serif;
$separator: -;

// Chaînes vides
$empty-string: "";

// Utilisation
.heading {
  font-family: $font-family-heading;
}

.body-text {
  font-family: $font-family-body;
}

.nav-item::after {
  content: $content-before;
}

.separator {
  content: $separator;
}
```

### 2.4 Les listes

Les listes peuvent être **séparées par des espaces** ou par des **virgules**.

```scss
// Liste séparée par des espaces
$box-shadow-list: 0 2px 4px rgba(0, 0, 0, 0.1), 0 4px 8px rgba(0, 0, 0, 0.1);

// Liste séparée par des virgules
$font-stacks: "Georgia", "Times New Roman", serif;
$gradient-stops: #1a1a2e, #16213e, #0f3460;

// Liste de sizes (nombres)
$sizes: 8px, 16px, 24px, 32px, 48px;

// Utilisation
.box {
  box-shadow: $box-shadow-list;
}

.text-serif {
  font-family: $font-stacks;
}

.gradient-bg {
  background: linear-gradient(135deg, $gradient-stops);
}

// Accès aux éléments individuels de la liste
.first-size {
  padding: nth($sizes, 1);
}

.second-size {
  padding: nth($sizes, 2);
}

.third-size {
  padding: nth($sizes, 3);
}
```

### 2.5 Les maps (associations)

Les maps sont des paires **clé : valeur**.

```scss
// Déclaration d'une map
$colors: (
  "primary": #3498db,
  "secondary": #2ecc71,
  "danger": #e74c3c,
  "warning": #f39c12,
  "info": #17a2b8,
  "light": #f8f9fa,
  "dark": #343a40
);

// Map de breakpoints
$breakpoints: (
  "small": 576px,
  "medium": 768px,
  "large": 992px,
  "xlarge": 1200px,
  "xxlarge": 1400px
);

// Map de typographie
$typography: (
  "h1": (font-size: 2.5rem, font-weight: 700, line-height: 1.2),
  "h2": (font-size: 2rem, font-weight: 600, line-height: 1.3),
  "h3": (font-size: 1.5rem, font-weight: 600, line-height: 1.4),
  "body": (font-size: 1rem, font-weight: 400, line-height: 1.6)
);

// Accès aux valeurs
.btn-primary {
  background-color: map-get($colors, "primary");
  border: 1px solid map-get($colors, "primary");
}

.btn-danger {
  background-color: map-get($colors, "danger");
  border: 1px solid map-get($colors, "danger");
}

// Itération sur une map
@each $name, $color in $colors {
  .text-#{$name} {
    color: $color;
  }

  .bg-#{$name} {
    background-color: $color;
  }
}
```

### 2.6 Les booléens

```scss
// Valeurs booléens
$enable-shadows: true;
$enable-gradients: false;
$enable-rounded: true;
$is-responsive: true;

// Utilisation conditionnelle
$border-radius-value: 0;

@if $enable-rounded {
  $border-radius-value: 4px;
}

.card {
  border-radius: $border-radius-value;

  @if $enable-shadows {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  }

  @if $enable-gradients {
    background: linear-gradient(135deg, #667eea, #764ba2);
  } @else {
    background-color: white;
  }
}

// Booléens pour des fonctionnalités
$use-transitions: true;
$transition-speed: 0.3s;

.button {
  transition: all $transition-speed ease;

  &:hover {
    @if $use-transitions {
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }
  }
}
```

### 2.7 La valeur null

`null`代表「aucune valeur」(aucune valeur définie).

```scss
$secondary-color: null;
$extra-padding: null;
$custom-border: null;

// null est ignoré lors de la compilation
.element {
  color: $secondary-color;  // Cette ligne sera ignorée
  padding: 10px;
  margin: $extra-padding;   // Cette ligne sera ignorée
  border: $custom-border;   // Cette ligne sera ignorée
}

// Résultat CSS :
// .element {
//   padding: 10px;
// }

// Comparaison avec null
$variable-a: null;
$variable-b: false;

.test {
  // null est falsy
  @if $variable-a {
    display: block;  // PAS exécuté
  }

  // false est aussi falsy
  @if $variable-b {
    display: none;   // PAS exécuté
  }

  // Seules les valeurs "truthy" sont exécutées
  $variable-c: true;
  @if $variable-c {
    display: flex;   // Exécuté
  }
}

// Vérification si une variable n'est pas null
$custom-bg: null;

.box {
  background-color: if($custom-bg, $custom-bg, #fff);
  // Equivalent : si $custom-bg n'est pas null, utilise $custom-bg, sinon #fff
}
```

---

## 3. L'interpolation des variables (`#{}`)

L'interpolation permet d'insérer la valeur d'une variable à l'intérieur d'un **nom de propriété**, d'un **sélecteur**, d'une **valeur**, ou d'une **chaîne de caractères**.

### 3.1 Interpolation dans les valeurs

```scss
$color-primary: #3498db;
$unit: px;

.button {
  // L'interpolation n'est pas toujours nécessaire dans les valeurs
  padding: 10px + 5px;
  color: $color-primary;  // Utilisation directe
}

// Mais elle est nécessaire pour construire des chaînes
$prefix: btn;
$size: lg;

// Ceci ne fonctionne PAS comme on pourrait le penser :
// .#{$prefix}-#{$size} { }  // Ceci crée le sélecteur .btn-lg
```

### 3.2 Interpolation dans les noms de propriétés

```scss
$property: margin;
$side: top;
$amount: 20px;

.element {
  // Nom de propriété dynamique
  #{$property}-#{$side}: $amount;
}

// Résultat : .element { margin-top: 20px; }
```

### 3.3 Interpolation dans les sélecteurs

```scss
$component: card;
$modifier: primary;

// Création dynamique de classes BEM
.#{$component} {
  padding: 1rem;
  border-radius: 8px;

  &__title {
    font-size: 1.5rem;
    font-weight: bold;
  }

  &__content {
    padding: 0.5rem;
  }

  &--#{$modifier} {
    background-color: #3498db;
    color: white;
  }

  &--danger {
    background-color: #e74c3c;
    color: white;
  }
}

// Résultat CSS :
// .card { ... }
// .card__title { ... }
// .card__content { ... }
// .card--primary { ... }
// .card--danger { ... }
```

### 3.4 Interpolation dans les chaînes de caractères

```scss
$icon-prefix: "icon";
$icon-name: "arrow-right";

.icon {
  // Concaténation de chaînes
  content: "#{$icon-prefix}-#{$icon-name}";
  // Résultat : content: "icon-arrow-right";
}

$base-url: "https://example.com/assets";
$image-name: "logo";

.logo {
  // Construction d'URL
  background-image: url("#{$base-url}/images/#{$image-name}.png");
}

$animation-name: "fadeIn";

.fade-element {
  animation-name: $animation-name;
  // Utilisation dans @keyframes via interpolation
}
```

### 3.5 Interpolation dans les arguments de fonctions

```scss
$size-base: 16px;

@function calculate-rem($size) {
  @return $size / $size-base * 1rem;
}

h1 {
  font-size: calculate-rem(32px);    // 2rem
}

h2 {
  font-size: calculate-rem(24px);    // 1.5rem
}

p {
  font-size: calculate-rem(14px);    // 0.875rem
}
```

---

## 4. Variables globales vs locales

### 4.1 Portée globale

Une variable définie **en dehors** de tout bloc est globale.

```scss
// Ceci est une variable globale
$primary-color: #3498db;
$font-size-base: 16px;
$spacing-unit: 8px;

// Elle peut être utilisée partout dans le fichier
h1 {
  color: $primary-color;
}

.sidebar {
  font-size: $font-size-base;
}
```

### 4.2 Portée locale

Une variable définie **à l'intérieur** d'un sélecteur ou d'un bloc est locale à ce bloc.

```scss
$global-color: blue;

.element {
  $local-color: red;  // Variable locale
  color: $local-color;      // Fonctionne
  background: $global-color; // Fonctionne (accessible depuis l'extérieur)
}

.another-element {
  // color: $local-color;  // ERREUR ! $local-color n'existe pas ici
  color: $global-color;     // Fonctionne
}
```

### 4.3 Scope imbriqué et portée des variables

```scss
$color: red;

.parent {
  $parent-color: green;  // Locale au parent

  .child {
    color: $parent-color;  // Fonctionne : inherited scope
    // $parent-color est accessible dans les enfants
  }

  .another-child {
    background: $parent-color;  // Fonctionne aussi
  }
}

.sibling {
  // color: $parent-color;  // ERREUR ! Pas accessible ici
  color: $color;  // Fonctionne : variable globale
}
```

### 4.4 Écrasement de variables (Shadowing)

Une variable locale peut **masquer** une variable globale du même nom.

```scss
$color: blue;  // Globale

.element {
  $color: red;  // Locale, masque la globale dans ce scope
  color: $color;  // red (pas blue)

  .child {
    color: $color;  // red (hérité du scope parent)
  }
}

.another {
  color: $color;  // blue (la globale, pas affectée)
}

// Exemple plus complexe
$margin: 10px;

.container {
  $margin: 20px;  // Masque la globale
  margin: $margin;  // 20px

  .inner {
    $margin: 30px;  // Masque à nouveau
    margin: $margin;  // 30px

    .deep {
      margin: $margin;  // 30px (hérité du scope le plus proche)
    }
  }
}
```

---

## 5. Valeurs par défaut avec `!default`

Le drapeau `!default` assigne une valeur à une variable **uniquement si elle n'a pas déjà été définie**. C'est un concept crucial pour la création de bibliothèques et de thèmes.

### 5.1 Principe de fonctionnement

```scss
// Si $primary-color n'a PAS été défini avant cette ligne...
$primary-color: #3498db !default;

// Si $primary-color A ÉTÉ défini avant, la valeur !default est ignorée
```

### 5.2 Exemple concret : Système de thème

```scss
// ============================================
// Fichier : _variables.scss (défauts du thème)
// ============================================

// Couleurs
$primary-color: #007bff !default;
$secondary-color: #6c757d !default;
$success-color: #28a745 !default;
$danger-color: #dc3545 !default;
$warning-color: #ffc107 !default;
$info-color: #17a2b8 !default;

// Typographie
$font-family-base: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto !default;
$font-size-base: 1rem !default;
$font-weight-normal: 400 !default;
$font-weight-bold: 700 !default;
$line-height-base: 1.5 !default;

// Espacement
$spacers: (
  0: 0,
  1: 0.25rem,
  2: 0.5rem,
  3: 1rem,
  4: 1.5rem,
  5: 3rem
) !default;

// Bordures
$border-color: #dee2e6 !default;
$border-width: 1px !default;
$border-radius: 0.25rem !default;

// Ombres
$box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1) !default;

// ============================================
// Fichier : main.scss (personnalisation du thème)
// ============================================

// AVANT l'import de _variables.scss, on redéfinit les valeurs
$primary-color: #e74c3c;      // Rouge au lieu de bleu
$font-family-base: "Roboto", sans-serif;
$border-radius: 0;

@import 'variables';

// Résultat :
// $primary-color = #e74c3c (personnalisé)
// $secondary-color = #6c757d (défaut)
// $font-family-base = "Roboto", sans-serif (personnalisé)
// $border-radius = 0 (personnalisé)
```

### 5.3 Hiérarchie des priorités

```scss
// Ordre de résolution :

// 1. Variable déjà définie dans le scope
$color: red;

.element {
  $color: blue !default;  // Ignoré, $color est déjà red
  color: $color;  // red
}

// 2. Variable non définie => !default prend effet
$size: null;  // Définie mais null

.box {
  $size: 20px !default;  // Utilisé car $size est null
  padding: $size;  // 20px
}

// 3. Variable jamais définie => !default crée la variable
$width: 100px !default;

.container {
  width: $width;  // 100px
}
```

### 5.4 Utilisation pratique avec des fichiers modulaires

```scss
// ============================================
// Fichier : _config.scss (configurations par défaut)
// ============================================

// Toutes les valeurs par défaut ici
$grid-columns: 12 !default;
$grid-gutter: 30px !default;
$grid-breakpoints: (
  xs: 0,
  sm: 576px,
  md: 768px,
  lg: 992px,
  xl: 1200px
) !default;

$enable-responsive: true !default;
$enable-dark-mode: false !default;

// ============================================
// Fichier : _custom-config.scss (personnalisations)
// ============================================

// On personnalise AVANT l'import
$grid-columns: 16;
$grid-gutter: 20px;
$enable-dark-mode: true;

// ============================================
// Fichier : _theme.scss
// ============================================

// Import des deux fichiers
@import 'custom-config';
@import 'config';

// $grid-columns = 16 (personnalisé)
// $grid-gutter = 20px (personnalisé)
// $enable-dark-mode = true (personnalisé)
// $enable-responsive = true (défaut)
```

---

## 6. Bonnes pratiques pour les variables

### 6.1 Conventions de nommage

```scss
// ✅ BON : Noms descriptifs et cohérents
$color-primary: #3498db;
$spacing-lg: 24px;
$font-size-heading: 2rem;
$border-radius-sm: 4px;

// ❌ MAUVAIS : Noms vagues ou inconsistants
$blue: #3498db;
$big: 24px;
$size: 2rem;
$r: 4px;

// ✅ BON : Préfixes par catégorie
$color-primary: #3498db;
$color-secondary: #2ecc71;
$color-danger: #e74c3c;

$shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
$shadow-md: 0 4px 6px rgba(0,0,0,0.1);
$shadow-lg: 0 10px 15px rgba(0,0,0,0.1);
```

### 6.2 Organisation des fichiers

```scss
// ============================================
// Fichier : _variables-base.scss
// ============================================
// Variables fondamentales du projet
$primary-color: #3498db;
$font-family-base: "Helvetica Neue", sans-serif;

// ============================================
// Fichier : _variables-components.scss
// ============================================
// Variables spécifiques aux composants
$button-padding: 10px 20px;
$card-padding: 20px;

// ============================================
// Fichier : _variables-responsive.scss
// ============================================
// Breakpoints et variables responsive
$breakpoint-sm: 576px;
$breakpoint-md: 768px;
```

---

## 7. Exercices pratiques

### Exercice 1 : Variables de base

Créez les variables suivantes et utilisez-les dans des sélecteurs :

```scss
// Créez ces variables :
// - $bg-color: #f5f5f5
// - $text-color: #333
// - $heading-color: #1a1a2e
// - $font-primary: "Arial", sans-serif
// - $spacing-base: 16px

// Puis utilisez-les :
// - body utilise $bg-color, $text-color, $font-primary
// - h1, h2, h3 utilisent $heading-color
// - .section utilise padding: $spacing-base
```

### Exercice 2 : Types de données

```scss
// Créez une map de couleurs pour un site e-commerce
// avec au moins 6 couleurs (primary, secondary, success, danger, warning, info)

// Créez une liste de 5 tailles de police
// Utilisez nth() pour appliquer chaque taille à un heading (h1 à h5)

// Créez un booléen $enable-animations: true
// Créez une variable $transition-duration: 0.3s
// Appliquez une transition aux boutons si $enable-animations est true
```

### Exercice 3 : Interpolation

```scss
// Créez des variables :
// - $prefix: "btn"
// - $sizes: ("sm", "md", "lg")
// - $paddings: (8px 16px, 12px 24px, 16px 32px)

// Utilisez une boucle @each avec l'interpolation pour créer :
// .btn-sm, .btn-md, .btn-lg avec les paddings correspondants
```

### Exercice 4 : Variables globales et locales

```scss
// Créez une variable globale $theme-color: blue
// Dans un sélecteur .parent, créez une variable locale $theme-color: green
// Dans .parent .child, utilisez $theme-color
// Dans .sibling (en dehors de .parent), utilisez $theme-color
// Quel est le résultat pour chaque cas ?
```

### Exercice 5 : Valeurs par défaut

```scss
// Créez un fichier _defaults.scss avec les variables suivantes (toutes en !default) :
// - $border-width: 1px
// - $border-color: #ccc
// - $border-radius: 4px
// - $font-size-base: 16px
// - $line-height-base: 1.5

// Dans votre fichier principal, redéfinissez $border-radius: 0
// puis importez _defaults.scss
// Vérifiez que $border-radius vaut 0 et que les autres gardent leur valeur par défaut
```

### Exercice 6 : Projet complet

Créez un système de variables complet pour un portfolio :

```scss
// Variables couleurs (6 minimum)
// Variables typographie (4 minimum)
// Variables espacement (5 minimum)
// Variables breakpoints (4 minimum)
// Variables ombres (3 niveaux)
// Variables transitions (2 minimum)
// Un booléen $dark-mode: false
// Utilisez !default pour toutes les variables
// Créez un sélecteur .portfolio-item qui utilise au moins 8 variables différentes
```

---

## Résumé

| Concept | Syntaxe | Exemple |
|---------|---------|---------|
| Variable simple | `$nom: valeur;` | `$color: blue;` |
| Interpolation | `#{$variable}` | `#{$prefix}-btn` |
| Scope local | `{ $var: val; }` | Bloc imbriqué |
| Scope global | En dehors des blocs | `$global: val;` |
| Valeur par défaut | `!default` | `$x: 10px !default;` |
| Null | `null` | `$x: null;` |
| Booléen | `true` / `false` | `$flag: true;` |

---

## Chapitre suivant

Dans le [Chapitre 5 - Imbrication (Nesting)](chapitre-05-imbrication.md), nous verrons comment structurer notre code CSS de manière hiérarchique grâce à l'imbrication Sass.
