# Chapitre 17 : Les Boucles

## Introduction

Les **boucles** sont l'un des outils les plus puissants de Sass pour generer du CSS de maniere programmatique. Au lieu d'ecrire les meme proprietes dizaines de fois, vous pouvez utiliser des boucles pour les generer automatiquement.

Sass propose trois types de boucles :
- **`@for`** : itere un nombre defini de fois
- **`@while`** : itere tant qu'une condition est vraie
- **`@each`** : itere sur les elements d'une liste ou d'une map

Ces boucles sont essentielles pour creer des **systemes utilitaires** (comme Tailwind CSS), des **grilles**, des **echelles de couleurs**, et tout autre ensemble de styles repete et previsible.

---

## 1. La boucle `@for`

### Syntaxe avec `through`

```scss
@for $variable from $debut through $fin {
  // code a repeter
}
```

**`through`** signifie que la boucle inclut la valeur finale.

```scss
@for $i from 1 through 3 {
  .element-#{$i} {
    width: $i * 100px;
  }
}
```

**CSS genere :**

```css
.element-1 { width: 100px; }
.element-2 { width: 200px; }
.element-3 { width: 300px; }
```

### Syntaxe avec `to`

```scss
@for $variable from $debut to $fin {
  // code a repeter
}
```

**`to`** signifie que la boucle **n'inclut PAS** la valeur finale.

```scss
@for $i from 1 to 4 {
  .element-#{$i} {
    width: $i * 100px;
  }
}
```

**CSS genere (identique ici car 1 to 4 = 1, 2, 3) :**

```css
.element-1 { width: 100px; }
.element-2 { width: 200px; }
.element-3 { width: 300px; }
```

### Différence `through` vs `to`

```scss
// through : inclut 5
@for $i from 1 through 5 {
  .col-#{$i} { width: percentage($i / 5); }
}
// Genere: .col-1, .col-2, .col-3, .col-4, .col-5

// to : n'inclut PAS 5
@for $i from 1 to 5 {
  .col-#{$i} { width: percentage($i / 5); }
}
// Genere: .col-1, .col-2, .col-3, .col-4
```

### Exemple : colonnes de grille

```scss
$colonnes: 12;

@for $i from 1 through $colonnes {
  .col-#{$i} {
    width: percentage($i / $colonnes);
    float: left;
    padding: 0 0.5rem;
  }
}
```

**CSS genere (extraits) :**

```css
.col-1 { width: 8.33333%; float: left; padding: 0 0.5rem; }
.col-2 { width: 16.66667%; float: left; padding: 0 0.5rem; }
.col-3 { width: 25%; float: left; padding: 0 0.5rem; }
.col-4 { width: 33.33333%; float: left; padding: 0 0.5rem; }
/* ... jusqu'a .col-12 */
```

### Exemple : espacement progressif

```scss
@for $i from 1 through 10 {
  .espace-#{$i} {
    margin: #{$i * 0.25}rem;
  }

  .espace-x-#{$i} {
    margin-left: #{$i * 0.25}rem;
    margin-right: #{$i * 0.25}rem;
  }

  .espace-y-#{$i} {
    margin-top: #{$i * 0.25}rem;
    margin-bottom: #{$i * 0.25}rem;
  }
}
```

### Exemple : z-index

```scss
@for $i from 1 through 50 {
  .z-index-#{$i} {
    z-index: $i;
  }
}

.z-index-auto { z-index: auto; }
.z-index-negatif { z-index: -1; }
```

---

## 2. La boucle `@while`

### Syntaxe

```scss
@while condition {
  // code a repeter
  // IMPORTANT : modifier la condition pour eviter une boucle infinie
}
```

### Exemple : generer des puissances de 2

```scss
$i: 1;

@while $i <= 128 {
  .taille-#{$i} {
    width: $i * 1px;
    height: $i * 1px;
  }
  $i: $i * 2;
}
```

**CSS genere :**

```css
.taille-1 { width: 1px; height: 1px; }
.taille-2 { width: 2px; height: 2px; }
.taille-4 { width: 4px; height: 4px; }
.taille-8 { width: 8px; height: 8px; }
.taille-16 { width: 16px; height: 16px; }
.taille-32 { width: 32px; height: 32px; }
.taille-64 { width: 64px; height: 64px; }
.taille-128 { width: 128px; height: 128px; }
```

### Exemple : series de Fibonacci

```scss
$a: 0;
$b: 1;
$i: 1;

@while $i <= 15 {
  .fib-#{$i} {
    width: $b * 1px;
  }

  $temp: $a + $b;
  $a: $b;
  $b: $temp;
  $i: $i + 1;
}
```

### Exemple : grilles en cascade

```scss
$max-profondeur: 5;
$profondeur: 1;

@while $profondeur <= $max-profondeur {
  .enfant-niveau-#{$profondeur} {
    margin-left: $profondeur * 20px;
    padding-left: $profondeur * 0.5rem;
  }

  $profondeur: $profondeur + 1;
}
```

### Attention aux boucles infinies

```scss
// MAUVAIS - boucle infinie !
$i: 1;
@while $i > 0 {
  .element { width: $i * 10px; }
  // $i n'est jamais modifie, la condition est toujours vraie
}

// BON - la condition finira par etre fausse
$i: 1;
@while $i > 0 {
  .element { width: $i * 10px; }
  $i: $i - 1;  // Decrementer $i
}
```

---

## 3. La boucle `@each`

### `@each` sur une liste

```scss
$couleurs: #3498db, #2ecc71, #e74c3c, #f39c12;

@each $couleur in $couleurs {
  $index: index($couleurs, $couleur);
  .texte-couleur-#{$index} {
    color: $couleur;
  }
}
```

### `@each` sur une map

```scss
$espace: (
  xs: 0.25rem,
  sm: 0.5rem,
  md: 1rem,
  lg: 1.5rem,
  xl: 2rem
);

@each $nom, $valeur in $espace {
  .p-#{$nom} { padding: $valeur; }
  .m-#{$nom} { margin: $valeur; }
}
```

### `@each` sur une liste de listes

```scss
$points: (
  (0, 0),
  (100, 50),
  (200, 100),
  (300, 150)
);

@each $x, $y in $points {
  .point-#{$x}-#{$y} {
    position: absolute;
    left: $x * 1px;
    top: $y * 1px;
  }
}
```

---

## 4. Generer des colonnes automatiquement

### Systeme de grille a 12 colonnes

```scss
$total-colonnes: 12;

@for $i from 1 through $total-colonnes {
  .col-#{$i} {
    width: percentage($i / $total-colonnes);
    float: left;
    min-height: 1px;
    padding: 0 0.75rem;
  }
}

// Generer aussi les offsets
@for $i from 0 through $total-colonnes - 1 {
  .offset-#{$i} {
    margin-left: percentage($i / $total-colonnes);
  }
}
```

### Grille CSS avec `@for`

```scss
$colonnes: 12;

@for $i from 1 through $colonnes {
  .grid-col-#{$i} {
    grid-column: span $i;
  }
}

@for $i from 1 through $colonnes {
  .grid-row-#{$i} {
    grid-row: span $i;
  }
}
```

### Colonnes avec grilles Flexbox

```scss
@for $i from 1 through 6 {
  .flex-#{$i} {
    flex: $i;
  }

  .flex-grow-#{$i} {
    flex-grow: $i;
  }

  .flex-shrink-#{$i} {
    flex-shrink: $i;
  }
}
```

---

## 5. Generer des classes d'espacement

### Systeme d'espacement complet

```scss
$base-espace: 0.25rem;
$niveaux: 0, 1, 2, 3, 4, 5, 6, 8, 10, 12, 16, 20, 24, 32;

@each $niveau in $niveaux {
  @if $niveau == 0 {
    .m-0 { margin: 0 !important; }
    .p-0 { padding: 0 !important; }
    .mt-0 { margin-top: 0 !important; }
    .mb-0 { margin-bottom: 0 !important; }
    .ml-0 { margin-left: 0 !important; }
    .mr-0 { margin-right: 0 !important; }
    .pt-0 { padding-top: 0 !important; }
    .pb-0 { padding-bottom: 0 !important; }
    .pl-0 { padding-left: 0 !important; }
    .pr-0 { padding-right: 0 !important; }
    .mx-0 { margin-left: 0 !important; margin-right: 0 !important; }
    .my-0 { margin-top: 0 !important; margin-bottom: 0 !important; }
    .px-0 { padding-left: 0 !important; padding-right: 0 !important; }
    .py-0 { padding-top: 0 !important; padding-bottom: 0 !important; }
  } @else {
    $valeur: $base-espace * $niveau;

    // Marges
    .m-#{$niveau} { margin: $valeur !important; }
    .mt-#{$niveau} { margin-top: $valeur !important; }
    .mb-#{$niveau} { margin-bottom: $valeur !important; }
    .ml-#{$niveau} { margin-left: $valeur !important; }
    .mr-#{$niveau} { margin-right: $valeur !important; }
    .mx-#{$niveau} { margin-left: $valeur !important; margin-right: $valeur !important; }
    .my-#{$niveau} { margin-top: $valeur !important; margin-bottom: $valeur !important; }
    .mx-auto { margin-left: auto !important; margin-right: auto !important; }

    // Paddings
    .p-#{$niveau} { padding: $valeur !important; }
    .pt-#{$niveau} { padding-top: $valeur !important; }
    .pb-#{$niveau} { padding-bottom: $valeur !important; }
    .pl-#{$niveau} { padding-left: $valeur !important; }
    .pr-#{$niveau} { padding-right: $valeur !important; }
    .px-#{$niveau} { padding-left: $valeur !important; padding-right: $valeur !important; }
    .py-#{$niveau} { padding-top: $valeur !important; padding-bottom: $valeur !important; }
  }
}

// Negatifs
@for $i from 1 through 12 {
  .-m-#{$i} { margin: -$base-espace * $i !important; }
  .-mt-#{$i} { margin-top: -$base-espace * $i !important; }
  .-mb-#{$i} { margin-bottom: -$base-espace * $i !important; }
  .-ml-#{$i} { margin-left: -$base-espace * $i !important; }
  .-mr-#{$i} { margin-right: -$base-espace * $i !important; }
}
```

---

## 6. Generer des classes utilitaires

### Tailles de texte

```scss
$echelle-texte: (
  xs: 0.75rem,
  sm: 0.875rem,
  base: 1rem,
  lg: 1.125rem,
  xl: 1.25rem,
  "2xl": 1.5rem,
  "3xl": 1.875rem,
  "4xl": 2.25rem,
  "5xl": 3rem,
  "6xl": 3.75rem
);

@each $nom, $valeur in $echelle-texte {
  .texte-#{$nom} {
    font-size: $valeur !important;
  }
}

// Poids de police
$poids: (
  thin: 100,
  extra-light: 200,
  light: 300,
  normal: 400,
  medium: 500,
  semi-bold: 600,
  bold: 700,
  extra-bold: 800,
  black: 900
);

@each $nom, $valeur in $poids {
  .font-#{$nom} {
    font-weight: $valeur !important;
  }
}
```

### Alignement de texte

```scss
$alignements: left, center, right, justify;

@each $alignement in $alignements {
  .texte-#{$alignement} {
    text-align: $alignement !important;
  }
}

$transforms: uppercase, lowercase, capitalize, none;

@each $transform in $transforms {
  .texte-#{$transform} {
    text-transform: $transform !important;
  }
}
```

### Display et position

```scss
$displays: block, inline-block, inline, flex, grid, inline-flex, inline-grid, none;

@each $display in $displays {
  .d-#{$display} {
    display: $display !important;
  }
}

$positions: static, relative, absolute, fixed, sticky;

@each $position in $positions {
  .position-#{$position} {
    position: $position !important;
  }
}

// Positions top/right/bottom/left
@for $i from 0 through 24 {
  .top-#{$i} { top: $i * 0.25rem !important; }
  .right-#{$i} { right: $i * 0.25rem !important; }
  .bottom-#{$i} { bottom: $i * 0.25rem !important; }
  .left-#{$i} { left: $i * 0.25rem !important; }
}
```

### Overflow et cursors

```scss
$overflows: visible, hidden, scroll, auto;

@each $overflow in $overflows {
  .overflow-#{$overflow} {
    overflow: $overflow !important;
  }
}

$cursors: default, pointer, text, move, not-allowed, grab, zoom-in, zoom-out;

@each $cursor in $cursors {
  .cursor-#{$cursor} {
    cursor: $cursor !important;
  }
}
```

### Border radius

```scss
@for $i from 0 through 8 {
  .rounded-#{$i} {
    border-radius: $i * 0.25rem !important;
  }
}

.rounded-full {
  border-radius: 9999px !important;
}

.rounded-none {
  border-radius: 0 !important;
}
```

### Largeur et hauteur

```scss
@for $i from 0 through 100 {
  @if $i % 5 == 0 {
    .w-#{$i} {
      width: #{$i}% !important;
    }
    .h-#{$i} {
      height: #{$i}% !important;
    }
  }
}

// Sizes specifiques
$sizes: (
  auto: auto,
  half: 50%,
  third: 33.333%,
  quarter: 25%,
  full: 100%
);

@each $nom, $valeur in $sizes {
  .w-#{$nom} { width: $valeur !important; }
  .h-#{$nom} { height: $valeur !important; }
  .min-w-#{$nom} { min-width: $valeur !important; }
  .max-w-#{$nom} { max-width: $valeur !important; }
  .min-h-#{$nom} { min-height: $valeur !important; }
  .max-h-#{$nom} { max-height: $valeur !important; }
}
```

### Ombres

```scss
$ombres: (
  none: none,
  sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05),
  base: 0 1px 3px 0 rgba(0, 0, 0, 0.1),
  md: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
  lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
  xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1),
  "2xl": 0 25px 50px -12px rgba(0, 0, 0, 0.25)
);

@each $nom, $valeur in $ombres {
  .shadow-#{$nom} {
    box-shadow: $valeur !important;
  }
}
```

### Transitions

```scss
$transitions: (
  none: none,
  rapide: 150ms ease-in-out,
  normale: 300ms ease-in-out,
  lente: 500ms ease-in-out
);

@each $nom, $valeur in $transitions {
  .transition-#{$nom} {
    transition: $valeur !important;
  }
}

// Transition sur des proprietes specifiques
$proprietes-transition: all, color, background-color, border-color, opacity, transform, box-shadow;

@each $prop in $proprietes-transition {
  .transition-#{$prop} {
    transition-property: $prop !important;
    transition-duration: 300ms !important;
    transition-timing-function: ease-in-out !important;
  }
}
```

### Opacite

```scss
@for $i from 0 through 100 {
  @if $i % 10 == 0 {
    .opacite-#{$i} {
      opacity: #{$i / 100} !important;
    }
  }
}
```

---

## 7. Generer des media queries responsives

### Boucle pour les breakpoints

```scss
$breakpoints: (
  mobile: 576px,
  tablette: 768px,
  desktop: 992px,
  large: 1200px,
  xlarge: 1400px
);

@each $nom, $valeur in $breakpoints {
  @media (max-width: $valeur) {
    // Display mobile-first
    .d-#{$nom}-none { display: none !important; }
    .d-#{$nom}-block { display: block !important; }
    .d-#{$nom}-flex { display: flex !important; }
    .d-#{$nom}-grid { display: grid !important; }
    .d-#{$nom}-inline { display: inline !important; }
    .d-#{$nom}-inline-block { display: inline-block !important; }

    // Grille responsive
    @for $i from 1 through 12 {
      .col-#{$nom}-#{$i} {
        width: percentage($i / 12);
        float: left;
      }
    }
  }
}
```

### Generer des grilles responsive

```scss
$breakpoints-liste: (
  sm: 576px,
  md: 768px,
  lg: 992px,
  xl: 1200px
);

// Grille de base
.grille {
  display: flex;
  flex-wrap: wrap;
  margin: 0 -0.5rem;
}

@each $bp, $valeur in $breakpoints-liste {
  @media (min-width: $valeur) {
    @for $i from 1 through 12 {
      .grille-#{$bp}-#{$i} {
        flex: 0 0 calc(#{$i / 12 * 100%} - 1rem);
        max-width: calc(#{$i / 12 * 100%} - 1rem);
        padding: 0 0.5rem;
      }
    }
  }
}
```

---

## 8. Generer des classes de couleur avec echelle

### Echelle de couleurs automatisee

```scss
$couleurs-base: (
  rouge: #e74c3c,
  bleu: #3498db,
  vert: #27ae60,
  orange: #f39c12,
  violet: #9b59b6,
  rose: #e91e63,
  cyan: #00bcd4
);

// Pour chaque couleur, generer des variantes de clair a sombre
@each $nom, $couleur in $couleurs-base {
  // Variantes claires (100 a 500)
  @for $i from 1 through 5 {
    $luminosite: (6 - $i) * 10%;
    .#{$nom}-#{$i * 100} {
      color: darken(lighten($couleur, 40%), $luminosite) !important;
    }
  }

  // Couleur de base (600)
  .#{$nom}-600 {
    color: $couleur !important;
  }

  // Variantes sombres (700 a 900)
  @for $i from 7 through 9 {
    $assombrissement: ($i - 6) * 10%;
    .#{$nom}-#{$i * 100} {
      color: darken($couleur, $assombrissement) !important;
    }
  }

  // Background variants
  .#{$nom}-bg {
    background-color: $couleur !important;
  }

  .#{$nom}-bg-light {
    background-color: lighten($couleur, 40%) !important;
  }

  .#{$nom}-bg-dark {
    background-color: darken($couleur, 10%) !important;
  }
}
```

---

## 9. Exemples pratiques avances

### 9.1 Systeme de grilles complet

```scss
// ==========================================
// SYSTEME DE GRILLES
// ==========================================

$grid-columns: 12;
$grid-gap: 1rem;

// Container
.container {
  width: 100%;
  margin: 0 auto;
  padding: 0 $grid-gap;

  @media (min-width: 576px) { max-width: 540px; }
  @media (min-width: 768px) { max-width: 720px; }
  @media (min-width: 992px) { max-width: 960px; }
  @media (min-width: 1200px) { max-width: 1140px; }
}

// Grille
.row {
  display: flex;
  flex-wrap: wrap;
  margin: 0 -($grid-gap / 2);
}

// Colonnes
@for $i from 1 through $grid-columns {
  .col-#{$i} {
    flex: 0 0 calc(#{$i / $grid-columns * 100%} - #{$grid-gap});
    max-width: calc(#{$i / $grid-columns * 100%} - #{$grid-gap});
    padding: 0 $grid-gap / 2;
  }

  // Offset
  .offset-#{$i} {
    margin-left: #{$i / $grid-columns * 100%};
  }
}

// Auto
.col {
  flex: 1 0 0%;
}

// Responsive columns
$breakpoints: (
  sm: 576px,
  md: 768px,
  lg: 992px,
  xl: 1200px
);

@each $bp, $valeur in $breakpoints {
  @media (min-width: $valeur) {
    @for $i from 1 through $grid-columns {
      .col-#{$bp}-#{$i} {
        flex: 0 0 calc(#{$i / $grid-columns * 100%} - #{$grid-gap});
        max-width: calc(#{$i / $grid-columns * 100%} - #{$grid-gap});
      }

      .offset-#{$bp}-#{$i} {
        margin-left: #{$i / $grid-columns * 100%};
      }
    }
  }
}
```

### 9.2 Echelle de typographie

```scss
// ==========================================
// ECHELLE TYPOGRAPHIQUE
// ==========================================

$base-size: 16px;
$ratio: 1.25; // ratio majeur

$echelle: (
  "2xs": -2,
  "xs": -1,
  "sm": 0,
  "base": 1,
  "lg": 2,
  "xl": 3,
  "2xl": 4,
  "3xl": 5,
  "4xl": 6,
  "5xl": 7,
  "6xl": 8
);

@each $nom, $niveau in $echelle {
  @if $niveau == 0 {
    .texte-#{$nom} {
      font-size: $base-size;
      line-height: 1.5;
    }
  } @else if $niveau < 0 {
    $facteur: math.pow($ratio, -$niveau);
    .texte-#{$nom} {
      font-size: $base-size / $facteur;
      line-height: 1.4;
    }
  } @else {
    $facteur: math.pow($ratio, $niveau);
    .texte-#{$nom} {
      font-size: $base-size * $facteur;
      line-height: 1.2;
    }
  }
}

// Line heights
$line-heights: (none: 1, tight: 1.25, snug: 1.375, normal: 1.5, relaxed: 1.625, loose: 2);

@each $nom, $valeur in $line-heights {
  .leading-#{$nom} {
    line-height: $valeur !important;
  }
}

// Letter spacing
$letter-spacings: (tighter: -0.05em, tight: -0.025em, normal: 0, wide: 0.025em, wider: 0.05em, widest: 0.1em);

@each $nom, $valeur in $letter-spacings {
  .tracking-#{$nom} {
    letter-spacing: $valeur !important;
  }
}
```

### 9.3 Systeme de grilles CSS avance

```scss
// ==========================================
// CSS GRID SYSTEM
// ==========================================

$grid-columns: 12;
$grid-gap-default: 1.5rem;

// Generer les colonnes de grille
@for $i from 1 through $grid-columns {
  .grid-col-#{$i} {
    grid-column: span $i;
  }

  .grid-row-#{$i} {
    grid-row: span $i;
  }
}

// Generer les positions de depart
@for $i from 1 through $grid-columns {
  .grid-col-start-#{$i} {
    grid-column-start: $i;
  }

  .grid-row-start-#{$i} {
    grid-row-start: $i;
  }
}

// Gap utilities
$gaps: (
  0: 0,
  xs: 0.25rem,
  sm: 0.5rem,
  md: 1rem,
  lg: 1.5rem,
  xl: 2rem,
  "2xl": 3rem
);

@each $nom, $valeur in $gaps {
  .gap-#{$nom} {
    gap: $valeur;
  }

  .col-gap-#{$nom} {
    column-gap: $valeur;
  }

  .row-gap-#{$nom} {
    row-gap: $valeur;
  }
}

// Grilles template responsive
@each $bp, $valeur in $breakpoints {
  @media (min-width: $valeur) {
    .grid-auto-#{$bp} {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    }

    @for $i from 2 through 6 {
      .grid-#{$bp}-#{$i} {
        display: grid;
        grid-template-columns: repeat($i, 1fr);
      }
    }
  }
}
```

### 9.4 Bundles de composants

```scss
// ==========================================
// BOUTONS AUTOMATISES
// ==========================================

$btn-sizes: (
  xs: (padding: 4px 8px, font-size: 0.75rem, height: 24px),
  sm: (padding: 6px 12px, font-size: 0.875rem, height: 32px),
  md: (padding: 8px 16px, font-size: 1rem, height: 40px),
  lg: (padding: 12px 24px, font-size: 1.125rem, height: 48px),
  xl: (padding: 16px 32px, font-size: 1.25rem, height: 56px)
);

$btn-colors: (
  primaire: #3498db,
  secondaire: #95a5a6,
  succes: #27ae60,
  danger: #e74c3c,
  avertissement: #f39c12,
  info: #17a2b8,
  sombre: #2c3e50,
  clair: #ecf0f1
);

// Bouton de base
%btn-base {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  border: 2px solid transparent;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s ease;
  text-decoration: none;
  line-height: 1;

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}

// Generer les tailles
@each $nom, $params in $btn-sizes {
  .btn-#{$nom} {
    @extend %btn-base;
    padding: map-get($params, padding);
    font-size: map-get($params, font-size);
    min-height: map-get($params, height);
  }
}

// Generer les couleurs (plein)
@each $nom, $couleur in $btn-colors {
  .btn-#{$nom} {
    background-color: $couleur;
    color: lighten($couleur, 40%);

    &:hover {
      background-color: darken($couleur, 8%);
    }

    &:active {
      background-color: darken($couleur, 15%);
    }
  }
}

// Generer les variantes outline
@each $nom, $couleur in $btn-colors {
  .btn-outline-#{$nom} {
    background-color: transparent;
    color: $couleur;
    border-color: $couleur;

    &:hover {
      background-color: $couleur;
      color: lighten($couleur, 40%);
    }
  }
}

// Generer les variantes ghost
@each $nom, $couleur in $btn-colors {
  .btn-ghost-#{$nom} {
    background-color: transparent;
    color: $couleur;

    &:hover {
      background-color: rgba($couleur, 0.1);
    }
  }
}

// Bouton avec icone
.btn-icon {
  padding: 0;

  &.btn-sm { width: 32px; height: 32px; }
  &.btn-md { width: 40px; height: 40px; }
  &.btn-lg { width: 48px; height: 48px; }

  border-radius: 50%;
}
```

---

## 10. Exercices

### Exercice 1 : Boucle `@for` de base

Utilisez `@for` pour generer des classes `.carre-{n}` avec une largeur et hauteur de `n * 20px` (de 1 a 10).

```scss
// Votre reponse ici
```

### Exercice 2 : Generer des puissances de 2

Avec `@while`, generer des classes `.taille-{n}` pour les puissances de 2 de 1 a 1024.

```scss
// Votre reponse ici
```

### Exercice 3 : Systeme d'espacement

Generer un systeme d'espacement complet (marges et paddings) avec `@for` ou `@each` pour les niveaux 0 a 12, avec un base de 4px.

```scss
// Votre reponse ici
```

### Exercice 4 : Grilles responsive

Generer un systeme de grilles responsive qui :
- A 12 colonnes de base
- Change de 1 col (mobile) a 4 col (desktop)
- Utilise des `@media` queries generees avec `@each`

```scss
// Votre reponse ici
```

### Exercice 5 : Systeme de couleurs

Generer un systeme de couleurs avec 5 couleurs de base, chacune avec 10 niveaux (100 a 1000), en variant de clair a sombre.

```scss
$base: (
  rouge: #ef4444,
  bleu: #3b82f6,
  vert: #22c55e,
  orange: #f97316,
  violet: #8b5cf6
);

// Votre reponse ici
```

---

## Resume

| Boucle | Syntaxe | Cas d'usage |
|---|---|---|
| `@for ... through` | `@for $i from 1 through 10` | Iterer un nombre defini de fois (inclus) |
| `@for ... to` | `@for $i from 1 to 10` | Iterer un nombre defini de fois (exclus) |
| `@while` | `@while condition { }` | Iterer tant qu'une condition est vraie |
| `@each` | `@each $x in $liste` | Parcourir une liste |
| `@each` (map) | `@each $k, $v in $map` | Parcourir une map cle/valeur |

---

## Conclusion du cours

Feliculations ! Vous avez termine le cours complet sur Sass/SCSS. Vous maitrisez maintenant :

- **Les variables** pour stocker des valeurs reutilisables
- **L'imbrication** pour organiser votre CSS
- **Les directives** (`@import`, `@use`, `@forward`)
- **Les opérateurs** pour les calculs
- **Les parts** pour le code modulaire
- **Les regles @at-root** pour sortir de l'imbrication
- **Les extensions** avec `@extend`
- **Les conditions** (`@if`, `@else if`, `@else`)
- **Les mixins** pour generer des blocs de styles
- **Les fonctions** pour calculer des valeurs
- **Les placeholders** pour reutiliser sans duplication
- **Les listes** pour gerer des collections
- **Les maps** pour des donnees structurees
- **Les boucles** pour generer du CSS dynamique

Ces outils vous permettent de creer des **systemes de design professionnels**, des **bibliotheques de composants**, et du **CSS maintenable et evolutif**. Continuez a pratiquer et a explorer les possibilites infinies de Sass !
