# Chapitre 13 : Les Fonctions

## Introduction

Les **fonctions Sass** sont des blocs de code qui calculent et **retournent une valeur**. Contrairement aux mixins qui génèrent des blocs de propriétés CSS, les fonctions retournent une seule valeur que vous pouvez utiliser n'importe où dans votre code Sass — dans une propriété, comme argument d'une autre fonction ou mixin, ou dans une expression.

Pensez aux fonctions comme à des **calculatrices personnalisées** : elles prennent des entrées, effectuent un calcul ou une transformation, et renvoient un résultat.

Sass dispose de nombreuses **fonctions intégrées** très utiles (comme `darken()`, `lighten()`, `map-get()`, `nth()`, etc.), mais vous pouvez également créer les vôtres.

---

## 1. Les fonctions intégrées de Sass

Avant de créer nos propres fonctions, familiarisons-nous avec les fonctions déjà disponibles.

### Fonctions de couleurs

```scss
$couleur: #3498db;

// Assombrir une couleur
.element1 { color: darken($couleur, 10%); }

// Éclaircir une couleur
.element2 { color: lighten($couleur, 20%); }

// Ajuster la saturation
.element3 { color: saturate($couleur, 20%); }
.element4 { color: desaturate($couleur, 10%); }

// Ajuster la luminosité
.element5 { color: brighten($couleur, 15%); }

// Opacité
.element6 { color: rgba($couleur, 0.5); }

// Mélanger des couleurs
.element7 { color: mix($couleur, #e74c3c, 50%); }

// Complémentaire
.element8 { color: complement($couleur); }

// Inverser
.element9 { color: invert($couleur); }
```

### Fonctions sur les chaînes de caractères

```scss
$texte: "Hello World";

-length($texte);           // 11 (nombre de caractères)
to-upper-case($texte);     // "HELLO WORLD"
to-lower-case($texte);     // "hello world"
str-slash($texte);         // "\"Hello World\""
str-insert($texte, " Beautiful", 6); // "Hello Beautiful World"
```

### Fonctions mathématiques

```scss
math.round(3.7);       // 4
math.round(3.2);       // 3
math.ceil(3.1);        // 4
math.floor(3.9);       // 3
math.abs(-5);          // 5
math.min(1, 2, 3);     // 1
math.max(1, 2, 3);     // 3
math.log(100, 10);     // 2
math.pow(2, 3);        // 8
math.sqrt(16);         // 4
```

### Fonctions sur les listes

```scss
$liste: 10px 20px 30px;

length($liste);          // 3
nth($liste, 1);          // 10px
nth($liste, 3);          // 30px
index($liste, 20px);     // 2
append($liste, 40px);    // 10px 20px 30px 40px
join($liste, (40px 50px)); // 10px 20px 30px 40px 50px
```

### Fonctions sur les maps

```scss
$map: (primary: blue, secondary: red);

map-get($map, primary);      // blue
map-has-key($map, primary);  // true
map-keys($map);               // (primary, secondary)
map-values($map);             // (blue, red)
map-merge($map, (tertiary: green)); // (primary: blue, secondary: red, tertiary: green)
```

---

## 2. Créer une fonction avec `@function`

La syntaxe pour définir une fonction Sass est :

```scss
@function nom-de-la-fonction($argument1, $argument2) {
  // logique de calcul
  @return $valeur;
}
```

La directive `@return` est **obligatoire** — elle spécifie la valeur que la fonction renvoie.

### Exemple simple : doubler une valeur

```scss
@function doubler($valeur) {
  @return $valeur * 2;
}
```

**Utilisation :**

```scss
.bandeau {
  height: doubler(50px);    // height: 100px;
  padding: doubler(1rem);   // padding: 2rem;
}
```

### Exemple : convertir en rem

```scss
@function en-rem($pixels, $base: 16px) {
  @return math.round($pixels / $base * 100) / 100 * 1rem;
}
```

**Utilisation :**

```scss
.texte {
  font-size: en-rem(14px);      // font-size: 0.875rem;
  margin: en-rem(24px);         // margin: 1.5rem;
  line-height: en-rem(32px);    // line-height: 2rem;
}
```

### Exemple : calculer une couleur plus sombre

```scss
@function couleur-sombre($couleur, $niveau: 10%) {
  @return darken($couleur, $niveau);
}

@function couleur-claire($couleur, $niveau: 10%) {
  @return lighten($couleur, $niveau);
}
```

---

## 3. Valeurs par défaut dans les fonctions

Comme pour les mixins, les fonctions supportent les **valeurs par défaut** pour leurs arguments.

### Exemple

```scss
@function calculer-marge($base: 1rem, $facteur: 1.5, $max: 5rem) {
  $resultat: $base * $facteur;

  @if $resultat > $max {
    @return $max;
  }

  @return $resultat;
}
```

**Utilisation :**

```scss
.section {
  margin-bottom: calculer-marge();           // 1.5rem (valeurs par défaut)
  padding: calculer-marge(2rem);             // 3rem (base modifiée)
  padding-bottom: calculer-marge(1rem, 2);   // 2rem (base et facteur modifiés)
}
```

---

## 4. Fonctions avec arguments nommés

Les fonctions supportent les arguments nommés, comme les mixins.

### Exemple

```scss
@function generer-ombre($x: 0, $y: 2px, $blur: 4px, $spread: 0, $couleur: rgba(0, 0, 0, 0.25)) {
  @return $x $y $blur $spread $couleur;
}
```

**Utilisation :**

```scss
.carte {
  box-shadow: generer-ombre($blur: 10px, $couleur: rgba(0, 0, 0, 0.1));
}
```

---

## 5. Conditions dans les fonctions (`@if` / `@else`)

Les fonctions peuvent contenir de la logique conditionnelle.

### Exemple : déterminer la taille de police responsive

```scss
@function taille-responsive($min: 1rem, $max: 2.5rem, $vmin: 320px, $vmax: 1200px) {
  @if $min == $max {
    @return $min;
  }

  @if $vmin >= $vmax {
    @return $min;
  }

  // Formule de clamp simplifiée
  @return clamp($min, #{$min} + #{($max - $min)} * ((100vw - $vmin) / #{$vmax - $vmin}), $max);
}
```

### Exemple : type de média

```scss
@function media-query($breakpoint) {
  @if $breakpoint == "mobile" {
    @return "(max-width: 576px)";
  } @else if $breakpoint == "tablette" {
    @return "(max-width: 768px)";
  } @else if $breakpoint == "desktop" {
    @return "(max-width: 992px)";
  } @else if $breakpoint == "large" {
    @return "(max-width: 1200px)";
  } @else if $breakpoint == "print" {
    @return "print";
  } @else {
    @return $breakpoint;
  }
}
```

**Utilisation :**

```scss
// Utilisation dans une mixin (pas directement dans @media)
@mixin responsive($breakpoint) {
  @media #{media-query($breakpoint)} {
    @content;
  }
}
```

---

## 6. Itération dans les fonctions (`@for`, `@each`)

Les fonctions peuvent contenir des boucles pour effectuer des calculs itératifs.

### Exemple : générer une liste de valeurs

```scss
@function generer-gammes($couleur, $pas: 10%, $total: 9) {
  $gammes: ();
  $luminosite: 10%;

  @for $i from 1 through $total {
    $gammes: append($gammes, lighten($couleur, $luminosite));
    $luminosite: $luminosite + $pas;
  }

  @return $gammes;
}
```

### Exemple : calcul de Fibonacci

```scss
@function fibonacci($n) {
  @if $n <= 1 {
    @return $n;
  }

  $a: 0;
  $b: 1;

  @for $i from 2 through $n {
    $temp: $a + $b;
    $a: $b;
    $b: $temp;
  }

  @return $b;
}
```

**Utilisation :**

```scss
.grille-1 { gap: fibonacci(1) * 1px; }  // 1px
.grille-2 { gap: fibonacci(2) * 1px; }  // 1px
.grille-3 { gap: fibonacci(3) * 1px; }  // 2px
.grille-4 { gap: fibonacci(4) * 1px; }  // 3px
.grille-5 { gap: fibonacci(5) * 1px; }  // 5px
.grille-6 { gap: fibonacci(6) * 1px; }  // 8px
.grille-7 { gap: fibonacci(7) * 1px; }  // 13px
.grille-8 { gap: fibonacci(8) * 1px; }  // 21px
```

---

## 7. Différence entre fonctions et mixins

C'est un point crucial à comprendre.

### Tableau comparatif

| Critère | `@function` | `@mixin` |
|---|---|---|
| **Retourne** | Une seule valeur | Un bloc de propriétés CSS |
| **Utilisation** | Dans une expression/propriété | Avec `@include` |
| **`@return`** | Obligatoire | Interdit |
| **`@content`** | Non supporté | Supporté |
| **Itération** | Oui | Oui |
| **Conditions** | Oui | Oui |
| **Cas d'usage** | Calculs, transformations | Styles réutilisables |

### Exemple comparatif

```scss
// FONCTION : retourne une valeur
@function doubler($valeur) {
  @return $valeur * 2;
}

// MIXIN : génère des propriétés
@mixin doubler-styles($propriete, $valeur) {
  #{$propriete}: $valeur * 2;
  #{$propriete}-hover: $valeur * 2.5;
}

// Utilisation de la fonction (dans une expression)
.bloc {
  width: doubler(100px);          // width: 200px;
  height: doubler(50px);          // height: 100px;
  padding: doubler(1rem);         // padding: 2rem;
}

// Utilisation de la mixin (génère des propriétés)
.bloc2 {
  @include doubler-styles(margin, 20px);
  // margin: 40px;
  // margin-hover: 50px;
}
```

### Quand utiliser quoi ?

- **Fonction** : quand vous avez besoin de **calculer** ou **transformer** une valeur
- **Mixin** : quand vous avez besoin de **générer des propriétés** CSS

---

## 8. Exemples pratiques de fonctions

### 8.1 Fonction de conversion d'unités

```scss
@function px-vers-rem($pixels, $base: 16px) {
  @if $pixels == 0 {
    @return 0;
  }
  @return math.round($pixels / $base * 1000) / 1000 * 1rem;
}

@function rem-vers-px($rem, $base: 16px) {
  @if $rem == 0 {
    @return 0;
  }
  @return math.round($rem * $base * 100) / 100 * 1px;
}
```

**Utilisation :**

```scss
.texte {
  font-size: px-vers-rem(16px);     // font-size: 1rem;
  font-size: px-vers-rem(14px);     // font-size: 0.875rem;
  font-size: px-vers-rem(12px);     // font-size: 0.75rem;
  font-size: px-vers-rem(20px);     // font-size: 1.25rem;
  font-size: px-vers-rem(24px);     // font-size: 1.5rem;
}

.bandeau {
  height: rem-vers-px(2rem);        // height: 32px;
  padding: rem-vers-px(1.5rem);     // padding: 24px;
}
```

### 8.2 Fonction de gamme de couleurs

```scss
@function gamme($couleur, $nb: 10) {
  $resultat: ();
  $pas: math.round(100 / ($nb + 1));

  @for $i from 1 through $nb {
    $luminosite: $pas * $i;
    $resultat: append($resultat, adjust-hue($couleur, $luminosite));
  }

  @return $resultat;
}
```

### 8.3 Fonction de calcul de taille responsive

```scss
@function taille-vw($min, $max, $vmin-viewport: 320px, $vmax-viewport: 1200px) {
  $diff-pixels: $max - $min;
  $diff-viewport: $vmax-viewport - $vmin-viewport;

  @return math.round($diff-pixels / $diff-viewport * 10000) / 100 * 1vw + $min * 1px;
}
```

**Utilisation :**

```scss
.titre-principal {
  // Taille qui varie de 24px (à 320px) à 48px (à 1200px)
  font-size: taille-vw(24px, 48px);
  font-weight: 700;
}

.titre-secondaire {
  font-size: taille-vw(18px, 32px);
  font-weight: 600;
}
```

### 8.4 Fonction de mapping d'espacement

```scss
@function espace($niveau) {
  $espace: (
    0: 0,
    1: 0.25rem,
    2: 0.5rem,
    3: 1rem,
    4: 1.5rem,
    5: 2rem,
    6: 3rem,
    8: 4rem,
    10: 5rem,
    12: 6rem,
    16: 8rem,
    20: 10rem,
  );

  @if not map-has-key($espace, $niveau) {
    @warn "Le niveau d'espacement '#{$niveau}' n'existe pas. Valeurs disponibles : #{map-keys($espace)}";
    @return null;
  }

  @return map-get($espace, $niveau);
}
```

**Utilisation :**

```scss
.card {
  padding: espace(4);        // padding: 1.5rem;
  margin-bottom: espace(3);  // margin-bottom: 1rem;
  gap: espace(2);            // gap: 0.5rem;
}

.section {
  padding-top: espace(6);    // padding-top: 3rem;
  padding-bottom: espace(6); // padding-bottom: 3rem;
}
```

### 8.5 Fonction de calcul de contraste

```scss
@function luminance($couleur) {
  $rgb: red($couleur) * 0.2126 + green($couleur) * 0.7152 + blue($couleur) * 0.0722;
  @return $rgb / 255;
}

@function est-sombre($couleur) {
  @return luminance($couleur) < 0.5;
}

@function couleur-texte-pour($couleur-fond) {
  @if est-sombre($couleur-fond) {
    @return #ffffff;
  } @else {
    @return #1a1a2e;
  }
}
```

**Utilisation :**

```scss
.bouton-primair {
  background-color: #3498db;
  color: couleur-texte-pour(#3498db); // #ffffff (la couleur est "claire")

  &:hover {
    background-color: darken(#3498db, 10%);
  }
}

.bouton-sombre {
  background-color: #2c3e50;
  color: couleur-texte-pour(#2c3e50); // #1a1a2e... non, #ffffff car sombre

  &:hover {
    background-color: darken(#2c3e50, 10%);
  }
}
```

### 8.6 Fonction de gestion de z-index

```scss
$z-index-layers: (
  "base": 0,
  "dropdown": 100,
  "sticky": 200,
  "overlay": 300,
  "modal": 400,
  "popover": 500,
  "toast": 600,
  "tooltip": 700,
);

@function z($couche) {
  @if not map-has-key($z-index-layers, $couche) {
    @warn "La couche z-index '#{$couche}' n'existe pas.";
    @return 0;
  }

  @return map-get($z-index-layers, $couche);
}

// Incrémenter pour des variantes
@function z-increment($couche, $increment: 1) {
  @return z($couche) + $increment;
}
```

**Utilisation :**

```scss
.dropdown { z-index: z("dropdown"); }
.modal { z-index: z("modal"); }
.tooltip { z-index: z("tooltip"); }
.modal-backdrop { z-index: z-increment("modal", -1); }  // 399
.modal-content { z-index: z-increment("modal", 1); }    // 401
```

### 8.7 Fonction de validation de breakpoint

```scss
@function valider-breakpoint($valeur) {
  $breakpoints: (
    "xs": 0,
    "sm": 576px,
    "md": 768px,
    "lg": 992px,
    "xl": 1200px,
    "xxl": 1400px,
  );

  @if map-has-key($breakpoints, $valeur) {
    @return map-get($breakpoints, $valeur);
  }

  // Si ce n'est pas un nom de breakpoint, retourner la valeur telle quelle
  @return $valeur;
}
```

---

## 9. Bonnes pratiques pour les fonctions

### 1. Toujours retourner une valeur

```scss
// MAUVAIS : pas de @return
@function calculer($a, $b) {
  $resultat: $a + $b;
}

// BON : toujours retourner
@function calculer($a, $b) {
  @return $a + $b;
}
```

### 2. Documenter vos fonctions avec des commentaires

```scss
/// Convertit des pixels en unités rem
/// @param {Number} $pixels - La valeur en pixels à convertir
/// @param {Number} $base [$base-font-size] - La taille de police de base
/// @returns {Number} La valeur en rem
/// @example scss - "Convertit 16px en 1rem"
///   en-rem(16px) => 1rem
@function en-rem($pixels, $base: 16px) {
  @return math.round($pixels / $base * 1000) / 1000 * 1rem;
}
```

### 3. Gérer les cas limites

```scss
@function diviser($a, $b) {
  @if $b == 0 {
    @warn "Division par zéro ! Retour de 0.";
    @return 0;
  }
  @return $a / $b;
}
```

### 4. Garder les fonctions simples et modulaires

```scss
// MAUVAIS : une fonction qui fait tout
@function complexe($a, $b, $c, $d, $e, $f) {
  // ... 50 lignes de logique
}

// BON : fonctions simples et combinables
@function multiplier($a, $b) {
  @return $a * $b;
}

@function arrondir($valeur, $precision: 2) {
  @return math.round($valeur * math.pow(10, $precision)) / math.pow(10, $precision);
}

@function multiplier-et-arrondir($a, $b, $precision: 2) {
  @return arrondir(multiplier($a, $b), $precision);
}
```

---

## 10. Exercices

### Exercice 1 : Fonction de mélange de couleurs

Créez une fonction `$mix($couleur1, $couleur2, $ratio: 0.5)` qui mélange deux couleurs avec le ratio donné.

```scss
// Votre réponse ici

.bloc1 {
  background: mix-couleurs(#3498db, #e74c3c, 0.25);  // Plus proche du bleu
  color: text-color-for-bg(mix-couleurs(#3498db, #e74c3c, 0.25));
}
```

### Exercice 2 : Fonction de conversion d'unités

Créez les fonctions :
- `$px-vers-em($valeur, $base: 16px)`
- `$px-vers-vw($valeur, $viewport-width: 1920px)`

```scss
// Votre réponse ici

.titre {
  font-size: px-vers-vw(48px);      // Conversion en vw
  line-height: px-vers-em(24px);     // Conversion en em
}
```

### Exercice 3 : Fonction de génération de tailles de polices

Créez une fonction `$font-scale($niveau, $base-size: 16px, $ratio: 1.25)` qui génère une échelle de polices basée sur une ratio modulaire.

- Niveau 0 : base-size (corps de texte)
- Niveau 1 : base-size * ratio (titres de niveau 3)
- Niveau 2 : base-size * ratio² (titres de niveau 2)
- Niveau 3 : base-size * ratio³ (titres de niveau 1)
- Niveau -1 : base-size / ratio (petit texte)

```scss
// Votre réponse ici

body {
  font-size: font-scale(0);    // 16px
}
h3 {
  font-size: font-scale(1);    // 20px
}
h2 {
  font-size: font-scale(2);    // 25px
}
h1 {
  font-size: font-scale(3);    // 31.25px
}
```

### Exercice 4 : Fonction de mapping de breakpoints

Créez une fonction `$bp($nom)` qui retourne la bonne media query pour chaque breakpoint de votre design system.

```scss
// Votre réponse ici

.element {
  @media (max-width: bp("desktop")) {
    display: none;
  }
}
```

### Exercice 5 : Fonction de calcul de grilles

Créez une fonction `$col-width($colonnes, $total: 12, $gap: 0)` qui calcule la largeur d'une colonne dans une grille.

```scss
// Votre réponse HERE

.col-6 {
  width: col-width(6, 12, 2rem);  // Calcule la largeur de 6 colonnes sur 12 avec un gap
}
.col-4 {
  width: col-width(4, 12, 2rem);
}
.col-3 {
  width: col-width(3, 12, 2rem);
}
```

---

## Résumé

| Concept | Syntaxe | Description |
|---|---|---|
| Définir une fonction | `@function nom($arg) { @return val; }` | Crée une fonction qui retourne une valeur |
| Valeurs par défaut | `@function nom($arg: val) { ... }` | Argument optionnel |
| Arguments nommés | `nom($arg: valeur)` | Nomme l'argument pour la clarté |
| `@return` | `@return expression;` | Retourne la valeur calculée |
| Différence avec mixin | Fonction = valeur unique, Mixin = bloc de propriétés | Choix selon le cas d'usage |

---

## Prochaine étape

Au chapitre suivant, nous verrons les **placeholders** (`%nom`), un mécanisme de réutilisation de styles qui complète les mixins et les fonctions, avec l'avantage de ne pas dupliquer le CSS généré.
