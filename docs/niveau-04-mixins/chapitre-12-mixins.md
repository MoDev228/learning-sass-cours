# Chapitre 12 : Les Mixins

## Introduction

Les **mixins** sont l'un des outils les plus puissants de Sass/SCSS. Une mixin (abréviation de "miscellaneous" ou "mélange") est un bloc de styles réutilisable qui peut être défini une fois et inclus n'importe où dans votre code Sass. Pensez-y comme à une **fonction CSS** qui peut générer des blocs de déclarations entiers.

Les mixins résolvent un problème fondamental du CSS : la répétition. Sans Sass, si vous aviez besoin d'un effet `clearfix` sur 10 éléments différents, vous devriez écrire les mêmes propriétés 10 fois. Avec les mixins, vous écrivez une seule fois et vous incluez autant de fois que nécessaire.

---

## 1. Créer une mixin avec `@mixin`

La syntaxe pour définir une mixin est la suivante :

```scss
@mixin nom-de-la-mixin {
  // propriétés CSS ici
}
```

### Exemple simple

```scss
@mixin clearfix {
  &::after {
    content: "";
    display: table;
    clear: both;
  }
}
```

Ici, nous avons défini une mixin qui génère automatiquement un `clearfix`. Ce pattern classique de CSS permet de contenir les éléments flottants.

### Exemple avec des propriétés multiples

```scss
@mixin card-base {
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  margin-bottom: 1rem;
}
```

Cette mixin définit les styles de base pour une carte (card). Elle contient cinq propriétés différentes qui seront toutes générées à chaque fois que la mixin est incluse.

### Exemple avec des styles de texte

```scss
@mixin heading-style {
  font-family: "Montserrat", sans-serif;
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: -0.02em;
  color: #1a1a2e;
}
```

### Exemple avec des animations

```scss
@mixin fade-in {
  animation: fadeIn 0.3s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

---

## 2. Inclure une mixin avec `@include`

Pour utiliser une mixin définie précédemment, on utilise la directive `@include` suivie du nom de la mixin :

```scss
@include nom-de-la-mixin;
```

### Utilisation des exemples précédents

```scss
.mon-conteneur {
  @include clearfix;
}

.ma-carte {
  @include card-base;
  // Ajouter d'autres styles spécifiques
  width: 300px;
  border: 1px solid #e0e0e0;
}

.mon-titre {
  @include heading-style;
  font-size: 2rem;
  margin-bottom: 0.5rem;
}

.mon-element {
  @include fade-in;
}
```

### Résultat CSS généré

```css
.mon-conteneur::after {
  content: "";
  display: table;
  clear: both;
}

.ma-carte {
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  margin-bottom: 1rem;
  width: 300px;
  border: 1px solid #e0e0e0;
}

.mon-titre {
  font-family: "Montserrat", sans-serif;
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: -0.02em;
  color: #1a1a2e;
  font-size: 2rem;
  margin-bottom: 0.5rem;
}

.mon-element {
  animation: fadeIn 0.3s ease-in-out;
}
```

---

## 3. Arguments et paramètres

Les mixins deviennent vraiment puissantes lorsqu'elles acceptent des **arguments** (aussi appelés paramètres). Cela permet de personnaliser le comportement de la mixin à chaque utilisation.

### Syntaxe de base avec un argument

```scss
@mixin border-radius($rayon) {
  border-radius: $rayon;
  -webkit-border-radius: $rayon; // Compatibilité navigateurs
  -moz-border-radius: $rayon;    // Compatibilité navigateurs
}
```

**Utilisation :**

```scss
.bouton {
  @include border-radius(8px);
}

.avatar {
  @include border-radius(50%); // Cercle parfait
}

.carte {
  @include border-radius(4px);
}
```

**CSS généré :**

```css
.bouton {
  border-radius: 8px;
  -webkit-border-radius: 8px;
  -moz-border-radius: 8px;
}

.avatar {
  border-radius: 50%;
  -webkit-border-radius: 50%;
  -moz-border-radius: 50%;
}
```

### Plusieurs arguments

Les mixins peuvent accepter plusieurs arguments, séparés par des virgules :

```scss
@mixin espace-marge($vertical, $horizontal) {
  margin-top: $vertical;
  margin-bottom: $vertical;
  margin-left: $horizontal;
  margin-right: $horizontal;
}
```

**Utilisation :**

```scss
.card {
  @include espace-marge(1rem, 2rem);
}
```

**Résultat :**

```css
.card {
  margin-top: 1rem;
  margin-bottom: 1rem;
  margin-left: 2rem;
  margin-right: 2rem;
}
```

### Exemple plus complexe : ombre personnalisable

```scss
@mixin ombre-personnalisee($x, $y, $blur, $spread: 0, $couleur: rgba(0, 0, 0, 0.25)) {
  box-shadow: $x $y $blur $spread $couleur;
  // Compatibilité avec les anciens navigateurs
  -webkit-box-shadow: $x $y $blur $spread $couleur;
  -moz-box-shadow: $x $y $blur $spread $couleur;
}
```

**Utilisation avec différents niveaux de personnalisation :**

```scss
.element1 {
  @include ombre-personnalisee(0, 2px, 4px);
}

.element2 {
  @include ombre-personnalisee(0, 4px, 8px, 0, rgba(0, 0, 0, 0.15));
}

.element3 {
  @include ombre-personnalisee(2px, 2px, 0, 2px, #333);
}
```

---

## 4. Valeurs par défaut pour les arguments

Vous pouvez définir des **valeurs par défaut** pour les arguments de vos mixins. Si l'appelant ne fournit pas une valeur pour cet argument, la valeur par défaut sera utilisée.

### Syntaxe

```scss
@mixin nom-de-la-mixin($argument: valeur-par-defaut) {
  // ...
}
```

### Exemple : mixin de bouton

```scss
@mixin bouton($couleur-bg: #3498db, $couleur-texte: #ffffff, $taille: "moyen") {
  display: inline-block;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.3s ease;

  @if $taille == "petit" {
    padding: 6px 12px;
    font-size: 0.85rem;
  } @else if $taille == "moyen" {
    padding: 10px 20px;
    font-size: 1rem;
  } @else if $taille == "grand" {
    padding: 14px 28px;
    font-size: 1.15rem;
  }

  background-color: $couleur-bg;
  color: $couleur-texte;

  &:hover {
    background-color: darken($couleur-bg, 10%);
    transform: translateY(-1px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  }

  &:active {
    transform: translateY(0);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
}
```

**Utilisation :**

```scss
// Utiliser toutes les valeurs par défaut
.btn-primaire {
  @include bouton();
}

// Personnaliser seulement la couleur
.btn-success {
  @include bouton(#27ae60);
}

// Personnaliser couleur et taille
.btn-danger-grand {
  @include bouton(#e74c3c, #fff, "grand");
}

// Tout personnaliser
.btn-custom {
  @include bouton(#8e44ad, #ecf0f1, "petit");
}
```

### Exemple : mixin media query responsive

```scss
@mixin responsive($breakpoint: "tablette") {
  @if $breakpoint == "mobile" {
    @media (max-width: 576px) {
      @content;
    }
  } @else if $breakpoint == "tablette" {
    @media (max-width: 768px) {
      @content;
    }
  } @else if $breakpoint == "desktop" {
    @media (max-width: 992px) {
      @content;
    }
  } @else if $breakpoint == "large" {
    @media (max-width: 1200px) {
      @content;
    }
  }
}
```

**Utilisation :**

```scss
.sidebar {
  width: 300px;
  float: left;

  @include responsive("tablette") {
    width: 100%;
    float: none;
  }
}

.grille {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;

  @include responsive("mobile") {
    grid-template-columns: 1fr;
  }

  @include responsive("tablette") {
    grid-template-columns: repeat(2, 1fr);
  }
}
```

---

## 5. Arguments nommés

Sass supporte les **arguments nommés**, ce qui signifie que vous pouvez spécifier le nom de l'argument lors de l'appel. Cela rend le code plus lisible et vous permet de passer les arguments dans n'importe quel ordre.

### Syntaxe

```scss
@include nom-de-la-mixin(nom-argument: valeur);
```

### Exemple

```scss
@mixin texte-style($taille: 1rem, $couleur: #333, $famille: "Roboto", $ poids: 400) {
  font-size: $taille;
  color: $couleur;
  font-family: $famille;
  font-weight: $poids;
}
```

**Appels avec arguments nommés :**

```scss
.texte-defaut {
  @include texte-style();
}

.texte-titre {
  @include texte-style(
    $taille: 2rem,
    $famille: "Montserrat",
    $poids: 700
  );
}

.texte-corps {
  @include texte-style(
    $couleur: #555,
    $taille: 1rem,
    $famille: "Source Sans Pro"
  );
}

.texte-accent {
  @include texte-style(
    $couleur: #e74c3c,
    $poids: 700,
    $taille: 1.25rem
  );
}
```

### Avantages des arguments nommés

1. **Lisibilité** : on comprend immédiatement quelle valeur correspond à quel paramètre
2. **Flexibilité** : on peut passer les arguments dans n'importe quel ordre
3. **Maintenance** : si l'ordre des arguments change dans la mixin, les appels existants ne cassent pas

---

## 6. Nombre variable d'arguments (`...args`)

Parfois, vous ne savez pas à l'avance combien d'arguments une mixin recevra. Sass permet de définir des mixins avec un **nombre variable d'arguments** en utilisant l'opérateur `...` (spread/rest).

### Syntaxe

```scss
@mixin nom-de-la-mixin($args...) {
  // $args est une liste de tous les arguments passés
}
```

### Exemple : marge automatique

```scss
@mixin marge($args...) {
  margin: $args;
}
```

**Utilisation :**

```scss
.element1 {
  @include marge(10px);           // margin: 10px;
}

.element2 {
  @include marge(10px, 20px);     // margin: 10px 20px;
}

.element3 {
  @include marge(10px, 20px, 30px); // margin: 10px 20px 30px;
}

.element4 {
  @include marge(10px, 20px, 30px, 40px); // margin: 10px 20px 30px 40px;
}
```

### Exemple : boîte d'ombre avec ombres multiples

```scss
@mixin ombres-multiples($ombres...) {
  box-shadow: $ombres;
}
```

**Utilisation :**

```scss
.carte-avancee {
  @include ombres-multiples(
    0 2px 4px rgba(0, 0, 0, 0.1),
    0 8px 16px rgba(0, 0, 0, 0.05),
    inset 0 -2px 4px rgba(0, 0, 0, 0.02)
  );
}
```

### Exemple : mixin de positionnement flexible

```scss
@mixin position($position, $args...) {
  position: $position;
  @if $args {
    $offsets: nth($args, 1);
    @if length($args) == 1 {
      top: $offsets;
      right: $offsets;
      bottom: $offsets;
      left: $offsets;
    } @else if length($args) == 2 {
      top: nth($args, 1);
      bottom: nth($args, 1);
      right: nth($args, 2);
      left: nth($args, 2);
    } @else if length($args) == 3 {
      top: nth($args, 1);
      right: nth($args, 2);
      bottom: nth($args, 3);
      left: nth($args, 2);
    } @else if length($args) == 4 {
      top: nth($args, 1);
      right: nth($args, 2);
      bottom: nth($args, 3);
      left: nth($args, 4);
    }
  }
}
```

**Utilisation :**

```scss
.overlay {
  @include position(fixed, 0);              // top, right, bottom, left = 0
}

.bandeau {
  @include position(absolute, 0, 20px);     // top/bottom = 0, right/left = 20px
}

.bulle {
  @include position(absolute, 10px, 20px, 30px); // top=10, right/left=20, bottom=30
}

.badge {
  @include position(absolute, 0, 10px, 10px, 0);
}
```

---

## 7. Quand utiliser les mixins vs les variables

C'est une question fréquente. Voici un guide pratique :

### Utilisez les **variables** quand :

- Vous stockez une **valeur unique** (une couleur, une taille, un espace)
- La valeur est utilisée directement comme propriété
- Vous voulez easily modifier une valeur à plusieurs endroits

```scss
// Variables : stocker des valeurs
$couleur-primaire: #3498db;
$couleur-secondaire: #2ecc71;
$taille-police: 16px;
$espace-base: 1rem;
$border-radius-base: 4px;
$transition-rapide: 0.2s ease;

// Utilisation directe
.bouton {
  background-color: $couleur-primaire;
  font-size: $taille-police;
  border-radius: $border-radius-base;
}
```

### Utilisez les **mixins** quand :

- Vous génerez un **bloc de styles complet** (plusieurs propriétés)
- Vous avez besoin de **logique conditionnelle** (`@if`, `@else`)
- Vous avez besoin de **paramètres** avec des valeurs par défaut
- Vous voulez réutiliser un **pattern CSS** commun

```scss
// Mixins : générer des blocs de styles
@mixin centre-absolu {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

@mixin texte-avec-ellipsis($lignes: 1) {
  overflow: hidden;
  @if $lignes == 1 {
    white-space: nowrap;
    text-overflow: ellipsis;
  } @else {
    display: -webkit-box;
    -webkit-line-clamp: $lignes;
    -webkit-box-orient: vertical;
  }
}
```

### Tableau comparatif

| Critère | Variables | Mixins |
|---|---|---|
| Stockage de valeurs simples | Oui | Non (overkill) |
| Blocs de styles complets | Non | Oui |
| Paramètres / arguments | Non | Oui |
| Valeurs par défaut | Non | Oui |
| Logique conditionnelle | Non | Oui |
| Itération | Non | Oui |
| Réutilisation de patterns | Non | Oui |
| Légèreté du CSS généré | Très léger | Peut générer beaucoup |

### Règle d'or

> Si vous remplacez **une seule propriété**, utilisez une **variable**.
> Si vous remplacez **plusieurs propriétés** ou un **pattern**, utilisez une **mixin**.

---

## 8. Exemples pratiques

### 8.1 Mixin de bouton complet

```scss
@mixin bouton-complet(
  $couleur-bg: #3498db,
  $couleur-texte: #ffffff,
  $couleur-bordure: null,
  $rayon: 4px,
  $taille: "moyen",
  $plein: true
) {
  // Tailles
  $padding-vertical: 10px;
  $padding-horizontal: 20px;
  $font-size: 1rem;

  @if $taille == "xs" {
    $padding-vertical: 4px;
    $padding-horizontal: 8px;
    $font-size: 0.75rem;
  } @else if $taille == "sm" {
    $padding-vertical: 6px;
    $padding-horizontal: 12px;
    $font-size: 0.85rem;
  } @else if $taille == "lg" {
    $padding-vertical: 14px;
    $padding-horizontal: 28px;
    $font-size: 1.15rem;
  } @else if $taille == "xl" {
    $padding-vertical: 18px;
    $padding-horizontal: 36px;
    $font-size: 1.3rem;
  }

  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: $padding-vertical $padding-horizontal;
  font-size: $font-size;
  font-weight: 600;
  text-decoration: none;
  border: 2px solid transparent;
  border-radius: $rayon;
  cursor: pointer;
  transition: all 0.3s ease;
  user-select: none;
  line-height: 1.5;

  @if $plein {
    background-color: $couleur-bg;
    color: $couleur-texte;

    &:hover {
      background-color: darken($couleur-bg, 10%);
      color: $couleur-texte;
    }
  } @else {
    background-color: transparent;
    color: $couleur-bg;
    border-color: $couleur-bg;

    &:hover {
      background-color: $couleur-bg;
      color: $couleur-texte;
    }
  }

  &:focus {
    outline: 3px solid rgba($couleur-bg, 0.3);
    outline-offset: 2px;
  }

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    pointer-events: none;
  }
}
```

### 8.2 Mixin Flexbox

```scss
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

@mixin flex-wrap($gap: 0) {
  display: flex;
  flex-wrap: wrap;
  gap: $gap;
}

@mixin flex-grid($items-per-row: 3, $gap: 1rem) {
  display: flex;
  flex-wrap: wrap;
  gap: $gap;

  > * {
    flex: 0 0 calc((100% - #{$gap * ($items-per-row - 1)}) / $items-per-row);
  }
}
```

**Utilisation :**

```scss
.navbar {
  @include flex-between;
  padding: 1rem 2rem;
}

.hero-content {
  @include flex-column-center;
  min-height: 100vh;
}

.galerie {
  @include flex-wrap(1rem);
}

.grille-produits {
  @include flex-grid(3, 1.5rem);

  @include responsive("tablette") {
    @include flex-grid(2, 1rem);
  }

  @include responsive("mobile") {
    @include flex-grid(1, 1rem);
  }
}
```

### 8.3 Mixin de grille CSS

```scss
@mixin grille($colonnes: 12, $gap: 1rem) {
  display: grid;
  grid-template-columns: repeat($colonnes, 1fr);
  gap: $gap;
}

@mixin colonne($start, $end: null) {
  @if $end {
    grid-column: #{$start} / #{$end};
  } @else {
    grid-column: span $start;
  }
}

@mixin rangee($start, $end: null) {
  @if $end {
    grid-row: #{$start} / #{$end};
  } @else {
    grid-row: span $start;
  }
}
```

### 8.4 Mixin d'animation

```scss
@mixin animation-nom($nom, $duree: 0.3s, $timing: ease) {
  animation: $nom $duree $timing;
}

@mixin transition($proprietes: all, $duree: 0.3s, $timing: ease, $delai: 0s) {
  transition: $proprietes $duree $timing $delai;
}

@mixin transition-personnalisee($props...) {
  @if length($props) == 1 {
    transition: nth($props, 1);
  } @else if length($props) == 2 {
    transition: nth($props, 1) nth($props, 2);
  } @else if length($props) == 3 {
    transition: nth($props, 1) nth($props, 2) nth($props, 3);
  } @else if length($props) >= 4 {
    transition: nth($props, 1) nth($props, 2) nth($props, 3) nth($props, 4);
  }
}

@mixin hover-scale($scale: 1.05) {
  transition: transform 0.3s ease;

  &:hover {
    transform: scale($scale);
  }
}

@mixin hover-lift($y: -4px) {
  transition: transform 0.3s ease, box-shadow 0.3s ease;

  &:hover {
    transform: translateY($y);
    box-shadow: 0 #{$y * -1} 20px rgba(0, 0, 0, 0.15);
  }
}
```

### 8.5 Mixin de typo responsive

```scss
@mixin typo-responsive($taille-min: 1rem, $taille-max: 2rem, $vmin: 1rem, $vmax: 4rem) {
  font-size: clamp(#{$taille-min}, calc(#{$taille-min} + (#{$taille-max} - #{$taille-min}) * ((100vw - #{$vmin}) / (#{$vmax} - #{$vmin}))), #{$taille-max});
}

@mixin text-gradient($couleur1, $couleur2, $direction: 135deg) {
  background: linear-gradient($direction, $couleur1, $couleur2);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

### 8.6 Mixin de grille d'espacement (utility classes)

```scss
@mixin generer-espacement($base: 0.25rem, $max: 10, $suffixe: "") {
  @for $i from 1 through $max {
    .m#{$suffixe}-#{$i * 4} {
      margin: #{$base * $i};
    }
    .mt#{$suffixe}-#{$i * 4} {
      margin-top: #{$base * $i};
    }
    .mb#{$suffixe}-#{$i * 4} {
      margin-bottom: #{$base * $i};
    }
    .p#{$suffixe}-#{$i * 4} {
      padding: #{$base * $i};
    }
    .pt#{$suffixe}-#{$i * 4} {
      padding-top: #{$base * $i};
    }
    .pb#{$suffixe}-#{$i * 4} {
      padding-bottom: #{$base * $i};
    }
  }
}
```

### 8.7 Mixin de dark mode

```scss
@mixin dark-mode {
  @media (prefers-color-scheme: dark) {
    @content;
  }
}

// OU avec un switch manuel
@mixin theme-dark {
  [data-theme="dark"] & {
    @content;
  }
}
```

**Utilisation :**

```scss
body {
  background-color: #ffffff;
  color: #333333;

  @include dark-mode {
    background-color: #1a1a2e;
    color: #e0e0e0;
  }
}

.carte {
  background-color: #f8f9fa;
  border: 1px solid #dee2e6;

  @include dark-mode {
    background-color: #16213e;
    border-color: #0f3460;
  }
}
```

---

## 9. Les blocs `@content` dans les mixins

Le mot-clé `@content` permet de passer un bloc de styles à une mixin lors de l'appel. C'est comme passer une fonction anonyme à une mixin.

### Syntaxe

```scss
@mixin nom-de-la-mixin {
  // styles avant
  @content;
  // styles après
}
```

### Exemple

```scss
@mixin conteneur-responsive {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;

  @content;
}

.page {
  @include conteneur-responsive {
    display: flex;
    gap: 2rem;
  }
}

.sidebar {
  @include conteneur-responsive {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  }
}
```

### Exemple avec plusieurs `@content`

Sass permet de passer **deux blocs** à une mixin :

```scss
@mixin conteneur-avec-media($breakpoint: "mobile") {
  max-width: 1200px;
  margin: 0 auto;

  @content; // Premier bloc

  @media (max-width: $breakpoint) {
    @content; // Deuxième bloc
  }
}

.sidebar {
  @include conteneur-avec-media("768px") {
    padding: 2rem;
  }
  // Résulté : padding: 2rem dans le conteneur
  // ET padding: 2rem dans le media query
}
```

---

## 10. Exercices

### Exercice 1 : Mixin de texte tronqué

Créez une mixin `$texte-tronque($lignes: 1)` qui :
- Pour 1 ligne : utilise `text-overflow: ellipsis`
- Pour plusieurs lignes : utilise `-webkit-line-clamp`

```scss
// Votre réponse ici
@mixin texte-tronque($lignes: 1) {
  // ...
}

.titre {
  @include texte-tronque(1);
}

.description {
  @include texte-tronque(3);
}
```

### Exercice 2 : Mixin de grille responsive

Créez une mixin `$grille-responsive($colonnes: 3, $gap: 1rem)` qui :
- Crée une grille avec le nombre de colonnes spécifié
- Utilise `auto-fit` et `minmax` pour être responsive
- Inclut un gap configurable

```scss
// Votre réponse ici
```

### Exercice 3 : Mixin de bouton complet

Créez une mixin `$btn($type: "primaire")` qui génère un bouton avec :
- Différentes couleurs selon le type (primaire, secondaire, succès, danger, avertissement)
- Un état hover qui assombrit la couleur de 10%
- Un état disabled avec opacité réduite
- Un état focus visible

```scss
// Votre réponse ici
```

### Exercice 4 : Mixin de responsive avancé

Créez une mixin `$responsive($breakpoint)` qui accepte les breakpoints suivants :
- `"mobile"` : max-width 576px
- `"tablette"` : max-width 768px
- `"desktop"` : max-width 992px
- `"large"` : max-width 1200px

Utilisez `@content` pour permettre de passer des styles.

```scss
// Votre réponse ici

.grille {
  display: grid;
  grid-template-columns: repeat(4, 1fr);

  @include responsive("mobile") {
    grid-template-columns: 1fr;
  }

  @include responsive("tablette") {
    grid-template-columns: repeat(2, 1fr);
  }
}
```

### Exercice 5 : Mixin d'ombre progressive

Créez une mixin `$ombre-progressive($niveau: 1)` qui génère des ombres de plus en plus fortes :
- Niveau 1 : ombre légère
- Niveau 2 : ombre moyenne
- Niveau 3 : ombre forte
- Niveau 4 : ombre très forte

```scss
// Votre réponse ici

.carte-legere {
  @include ombre-progressive(1);
}

.carte-forte {
  @include ombre-progressive(4);
}
```

---

## Résumé

| Concept | Syntaxe | Description |
|---|---|---|
| Définir une mixin | `@mixin nom { ... }` | Crée un bloc de styles réutilisable |
| Inclure une mixin | `@include nom;` | Utilise la mixin définie |
| Arguments | `@mixin nom($arg) { ... }` | Passe des valeurs à la mixin |
| Valeurs par défaut | `@mixin nom($arg: val) { ... }` | Valeur par défaut si non fournie |
| Arguments nommés | `@include nom($arg: val)` | Nomme l'argument pour plus de clarté |
| Arguments variables | `@mixin nom($args...) { ... }` | Nombre variable d'arguments |
| Blocs `@content` | `@content` dans la mixin | Passe un bloc de styles à la mixin |

---

## Prochaine étape

Au chapitre suivant, nous verrons les **fonctions Sass** qui permettent de calculer et retourner des valeurs, une autre brique essentielle de Sass pour rendre votre code plus dynamique et maintenable.
