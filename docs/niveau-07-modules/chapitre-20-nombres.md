# Chapitre 20 : Les Nombres — Module `sass:math`

## Introduction

Le module `sass:math` est l'un des modules les plus fondamentaux de Sass. Il fournit des fonctions pour effectuer des opérations mathématiques, gérer les unités, arrondir des nombres et effectuer des calculs complexes. Que vous créiez des grilles fluides, des typographies proportionnelles ou des calculs de mise en page, ce module est indispensable. Ce chapitre explore en détail chaque fonction de `sass:math` avec de nombreux exemples pratiques.

---

## 20.1 — Introduction au module `sass:math`

### Importation du module

```scss
@use 'sass:math';

.resultat {
  width: math.percentage(1 / 3);
}
```

### Types de nombres en Sass

Sass reconnaît plusieurs types de nombres :

```scss
$entier: 42;
$decimal: 3.14159;
$negatif: -100;
$positif: +50;
$scientifique: 1.5e2; // 150
$hex: 0xFF; // 255
$avec-unite: 10px;
$sans-unite: 10;
```

### Opérations arithmétiques de base

Avant de plonger dans les fonctions, rappelons les opérations de base :

```scss
$addition: 10 + 5;          // 15
$soustraction: 10 - 5;      // 5
$multiplication: 10 * 5;    // 50
$division: 10 / 5;           // 2 (avec math.div ou parenthèses)
$modulo: 10 % 3;             // 1

// Attention avec la division
$division-1: math.div(10, 5);   // 2
$division-2: (10 / 5);          // 2 (parenthèses nécessaires)
$division-3: 10px / 5px;        // 2 (unités identiques)
```

---

## 20.2 — `math.div()` : Division

La fonction `math.div()` est la manière recommandée d'effectuer des divisions en Sass moderne. Elle remplace l'opérateur `/` qui est maintenant utilisé principalement pour les raccourcis CSS.

### Syntaxe

```scss
math.div($dividende, $diviseur)
```

### Exemples de base

```scss
$division-1: math.div(10, 2);       // 5
$division-2: math.div(10, 3);       // 3.33333
$division-3: math.div(100, 7);      // 14.28571
$division-4: math.div(-10, 2);      // -5
$division-5: math.div(0, 5);        // 0
```

### Division avec unités

```scss
$largeur-colonne: math.div(100%, 12);  // 8.33333%
$hauteur: math.div(100vh, 3);          // 33.33333vh
$padding: math.div(20px, 4);           // 5px
$margin: math.div(60px, 3);            // 20px
```

### Utilisation pratique : grilles

```scss
$nbre-colonnes: 12;
$gouttiere: 20px;

.colonne {
  $largeur-colonne: math.div(100%, $nbre-colonnes);
  width: $largeur-colonne;
  padding: 0 math.div($gouttiere, 2);
}

.colonne-6 {
  width: math.div(100% * 6, $nbre-colonnes);
}

.colonne-4 {
  width: math.div(100% * 4, $nbre-colonnes);
}

.colonne-3 {
  width: math.div(100% * 3, $nbre-colonnes);
}
```

### Erreurs de division

```scss
// ❌ Division par zéro — erreur Sass
// math.div(10, 0);

// ✅ Vérification préalable
$diviseur: 0;

@if $diviseur != 0 {
  $resultat: math.div(10, $diviseur);
}
```

### Différence entre `/` et `math.div()`

```scss
// L'opérateur / est maintenant principalement pour le CSS
.font-size {
  font: 16px/1.5 Arial;  // Raccourci CSS — pas une division
}

// math.div() est pour les calculs Sass
.calcule {
  width: math.div(100%, 3);  // Division Sass
}
```

---

## 20.3 — `math.round()` : Arrondir

La fonction `math.round()` arrondit un nombre au nombre entier le plus proche.

### Syntaxe

```scss
math.round($nombre)
```

### Exemples

```scss
$exact: math.round(4.5);     // 5
$sans-decimal: math.round(4); // 4
$bas: math.round(4.4);       // 4
$haut: math.round(4.6);      // 5
$negatif: math.round(-4.5);  // -4 ou -5 selon l'implémentation
$negatif-bas: math.round(-4.3); // -4
$negatif-haut: math.round(-4.7); // -5
```

### Utilisation pratique

```scss
$nbre-colonnes: 7;
$largeur-totale: 1100px;

$largeur-colonne: math.round(math.div($largeur-totale, $nbre-colonnes));

.grille {
  .colonne {
    width: $largeur-colonne;
  }
}

// Arrondir les valeurs de typographie
$taille-base: 16px;
$ratio: 1.25;

h1 {
  font-size: math.round($taille-base * math.pow($ratio, 4));
}

h2 {
  font-size: math.round($taille-base * math.pow($ratio, 3));
}

h3 {
  font-size: math.round($taille-base * math.pow($ratio, 2));
}
```

---

## 20.4 — `math.ceil()` : Arrondir au supérieur

La fonction `math.ceil()` arrondit un nombre vers l'entier supérieur (plafond).

### Syntaxe

```scss
math.ceil($nombre)
```

### Exemples

```scss
$resultat-1: math.ceil(4.1);   // 5
$resultat-2: math.ceil(4.5);   // 5
$resultat-3: math.ceil(4.9);   // 5
$resultat-4: math.ceil(4.0);   // 4
$resultat-5: math.ceil(-4.1);  // -4
$resultat-6: math.ceil(-4.5);  // -4
$resultat-7: math.ceil(-4.9);  // -4
```

### Utilisation pratique : pagination

```scss
$total-elements: 23;
$elements-par-page: 5;

$nbre-pages: math.ceil(math.div($total-elements, $elements-par-page));
// Résultat : 5 pages

.pagination {
  display: flex;
  gap: 5px;

  @for $i from 1 through $nbre-pages {
    .page-#{$i} {
      width: 30px;
      height: 30px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
}
```

### Utilisation pratique : grilles CSS

```scss
$total-elements: 7;
$colonnes: 3;

$nbre-lignes: math.ceil(math.div($total-elements, $colonnes));

.grille {
  display: grid;
  grid-template-columns: repeat($colonnes, 1fr);
  grid-template-rows: repeat($nbre-lignes, 1fr);
}
```

---

## 20.5 — `math.floor()` : Arrondir au inférieur

La fonction `math.floor()` arrondit un nombre vers l'entier inférieur (plancher).

### Syntaxe

```scss
math.floor($nombre)
```

### Exemples

```scss
$resultat-1: math.floor(4.1);   // 4
$resultat-2: math.floor(4.5);   // 4
$resultat-3: math.floor(4.9);   // 4
$resultat-4: math.floor(4.0);   // 4
$resultat-5: math.floor(-4.1);  // -5
$resultat-6: math.floor(-4.5);  // -5
$resultat-7: math.floor(-4.9);  // -5
```

### Utilisation pratique : colonnes complètes

```scss
$total-elements: 7;
$colonnes: 3;

$colonnes-completes: math.floor(math.div($total-elements, $colonnes));
$elements-restants: $total-elements - ($colonnes-completes * $colonnes);

.grille {
  display: grid;
  grid-template-columns: repeat($colonnes, 1fr);

  .element {
    border: 1px solid #ddd;
    padding: 10px;
  }
}
```

### Différence entre ceil et floor

```scss
$nombre: 4.7;

.ceil {
  // 5 — toujours vers le haut
  value: math.ceil($nombre);
}

.floor {
  // 4 — toujours vers le bas
  value: math.floor($nombre);
}

.round {
  // 5 — au plus proche
  value: math.round($nombre);
}

.trunc {
  // 4 — tronque les décimales
  value: math.trunc($nombre);
}
```

---

## 20.6 — `math.trunc()` : Tronquer

La fonction `math.trunc()` supprime les décimales sans arrondir.

### Syntaxe

```scss
math.trunc($nombre)
```

### Exemples

```scss
$resultat-1: math.trunc(4.7);    // 4
$resultat-2: math.trunc(4.3);    // 4
$resultat-3: math.trunc(-4.7);   // -4
$resultat-4: math.trunc(4.0);    // 4
$resultat-5: math.trunc(100.999); // 100
```

### Comparaison avec les autres fonctions d'arrondi

```scss
$nombre: 4.7;

.arrondi {
  round: math.round($nombre);   // 5
  ceil: math.ceil($nombre);     // 5
  floor: math.floor($nombre);   // 4
  trunc: math.trunc($nombre);   // 4
}
```

### Utilisation pratique

```scss
$temps-total: 15.7; // secondes
$temps-arrondi: math.trunc($temps-total); // 15 secondes

.timer {
  &::after {
    content: "#{$temps-arrondi}s";
  }
}
```

---

## 20.7 — `math.percentage()` : Conversion en pourcentage

La fonction `math.percentage()` convertit un nombre sans unité en pourcentage.

### Syntaxe

```scss
math.percentage($nombre)
```

### Exemples

```scss
$pourcentage-1: math.percentage(0.5);    // 50%
$pourcentage-2: math.percentage(0.25);   // 25%
$pourcentage-3: math.percentage(0.1);    // 10%
$pourcentage-4: math.percentage(1);      // 100%
$pourcentage-5: math.percentage(1.5);    // 150%
$pourcentage-6: math.percentage(0.3333); // 33.33%
```

### Utilisation pratique : grilles fluides

```scss
$nbre-colonnes: 12;

@mixin colonne($nbre) {
  width: math.percentage(math.div($nbre, $nbre-colonnes));
}

.col-1 { @include colonne(1); }   // 8.33%
.col-2 { @include colonne(2); }   // 16.67%
.col-3 { @include colonne(3); }   // 25%
.col-4 { @include colonne(4); }   // 33.33%
.col-5 { @include colonne(5); }   // 41.67%
.col-6 { @include colonne(6); }   // 50%
.col-7 { @include colonne(7); }   // 58.33%
.col-8 { @include colonne(8); }   // 66.67%
.col-9 { @include colonne(9); }   // 75%
.col-10 { @include colonne(10); } // 83.33%
.col-11 { @include colonne(11); } // 91.67%
.col-12 { @include colonne(12); } // 100%
```

### Calcul de proportions

```scss
$total: 500;
$partie: 175;

$proportion: math.percentage(math.div($partie, $total)); // 35%

.barre-progression {
  width: $proportion;
  background-color: #3498db;
  height: 20px;
  border-radius: 10px;
}
```

---

## 20.8 — `math.min()` et `math.max()`

Ces fonctions retournent respectivement la valeur minimale et maximale parmi une liste de valeurs.

### Syntaxe

```scss
math.min($valeur1, $valeur2, ..., $valeurN)
math.max($valeur1, $valeur2, ..., $valeurN)
```

### Exemples de base

```scss
$min-1: math.min(10, 20, 30, 5);      // 5
$min-2: math.min(100px, 50px, 200px);  // 50px
$max-1: math.max(10, 20, 30, 5);       // 30
$max-2: math.max(100px, 50px, 200px);  // 200px
```

### Limitation de valeurs (clamping)

```scss
$largeur: 900px;
$min-largeur: 320px;
$max-largeur: 1200px;

.container {
  width: math.min(math.max($largeur, $min-largeur), $max-largeur);
  // Ou plus simplement :
  width: math.max($min-largeur, math.min($largeur, $max-largeur));
}
```

### Responsive avec min/max

```scss
$taille-texte: 2vw;
$min-taille: 14px;
$max-taille: 24px;

body {
  font-size: math.max($min-taille, math.min($taille-texte, $max-taille));
}
```

### Calcul de marges

```scss
$marge-haute: 100px;
$marge-basse: 50px;
$marge-gauche: 30px;
$marge-droite: 40px;

.section {
  margin: math.max($marge-haute, $marge-basse) math.max($marge-gauche, $marge-droite);
}
```

### Grille avec min/max

```scss
$nbre-colonnes: 4;
$gouttiere: 20px;
$largeur-max: 1200px;

@mixin grille-adaptative {
  $largeur-colonne: math.div(100%, $nbre-colonnes);
  $largeur-avec-gouttiere: $largeur-colonne - $gouttiere;

  display: grid;
  grid-template-columns: repeat($nbre-colonnes, 1fr);
  gap: $gouttiere;
  max-width: math.min($largeur-max, 100%);
  margin: 0 auto;
  padding: 0 math.max($gouttiere, 2%);
}
```

---

## 20.9 — `math.abs()` : Valeur absolue

La fonction `math.abs()` retourne la valeur absolue d'un nombre.

### Syntaxe

```scss
math.abs($nombre)
```

### Exemples

```scss
$abs-1: math.abs(5);    // 5
$abs-2: math.abs(-5);   // 5
$abs-3: math.abs(-3.14); // 3.14
$abs-4: math.abs(0);    // 0
```

### Utilisation pratique

```scss
$position-relative: -20px;
$decalage: math.abs($position-relative); // 20px

.element {
  left: $decalage;
}

// Calcul de distance entre deux points
$x1: 10;
$x2: 50;
$distance: math.abs($x2 - $x1); // 40
```

---

## 20.10 — `math.pow()` : Puissance

La fonction `math.pow()` élève un nombre à une puissance donnée.

### Syntaxe

```scss
math.pow($base, $exposant)
```

### Exemples

```scss
$puissance-1: math.pow(2, 3);   // 8
$puissance-2: math.pow(10, 2);  // 100
$puissance-3: math.pow(5, 0);   // 1
$puissance-4: math.pow(2, -1);  // 0.5
$puissance-5: math.pow(9, 0.5); // 3
```

### Utilisation pratique : échelle modulaire

```scss
$taille-base: 16px;
$ratio: 1.25;

$taille-xs: $taille-base * math.pow($ratio, -1); // 12.8px
$taille-sm: $taille-base;                          // 16px
$taille-md: $taille-base * math.pow($ratio, 1);   // 20px
$taille-lg: $taille-base * math.pow($ratio, 2);   // 25px
$taille-xl: $taille-base * math.pow($ratio, 3);   // 31.25px
$taille-xxl: $taille-base * math.pow($ratio, 4);  // 39.06px
```

### Système de grille exponentiel

```scss
$base: 8;
$niveaux: 6;

@for $i from 0 through $niveaux {
  .espace-#{$i} {
    padding: math.pow(2, $i) * 4px;
    // 4px, 8px, 16px, 32px, 64px, 128px, 256px
  }
}
```

---

## 20.11 — `math.sqrt()` : Racine carrée

La fonction `math.sqrt()` calcule la racine carrée d'un nombre.

### Syntaxe

```scss
math.sqrt($nombre)
```

### Exemples

```scss
$sqrt-1: math.sqrt(4);    // 2
$sqrt-2: math.sqrt(9);    // 3
$sqrt-3: math.sqrt(2);    // 1.41421
$sqrt-4: math.sqrt(100);  // 10
$sqrt-5: math.sqrt(0.25); // 0.5
```

### Utilisation pratique : ratio d'aspect

```scss
// Ratio黄金比 (golden ratio)
$phi: math.div(math.add(1, math.sqrt(5)), 2); // 1.618

.bloc-dore {
  width: 300px;
  height: math.div(300px, $phi);
}

// Ratio √2 (format papier A4)
$ratio-a4: math.sqrt(2);

.page-a4 {
  width: 210mm;
  height: math.div(210mm, $ratio-a4);
}
```

---

## 20.12 — `math.log()` et `math.pow()` combinés

### Calcul de logarithmes

```scss
// Logarithme naturel (base e)
$log-e: math.log(math.e);     // 1
$log-10: math.log(100, 10);   // 2
$log-2: math.log(8, 2);       // 3
```

### Échelle logarithmique

```scss
$echelle-log {
  @for $i from 1 through 5 {
    --niveau-#{$i}: math.pow(10, $i);
    // 10, 100, 1000, 10000, 100000
  }
}
```

---

## 20.13 — Gestion des unités

### Unités en Sass

```scss
$pixel: 10px;
$rem: 1.5rem;
$em: 2em;
$pourcentage: 50%;
$point: 16pt;

// Les unités peuvent être combinées dans les calculs
$ resultat: 10px * 2;          // 20px
$resultat: 10px + 5px;         // 15px
$resultat: math.div(100px, 5); // 20px
```

### Conversion d'unités

```scss
$pixels: 16px;
$points: $pixels * 75 / 96; // Conversion px vers pt

$centimetres: 2.54cm;
$millimetres: $centimetres * 10;
$pouces: math.div($centimetres, 2.54);
```

### Fonctions utilitaires pour les unités

```scss
@mixin supprimer-unite($valeur) {
  // Utile pour les calculs
}

// Utilisation de math.div pour supprimer les unités
$largeur: 100px;
$nbre-colonnes: 4;
$resultat: math.div($largeur, $nbre-colonnes); // 25px

// Pour un nombre sans unité
$pure-number: math.div(100px, 100px); // 1
```

---

## 20.14 — Exemples pratiques avancés

### Système de design complet

```scss
@use 'sass:math';

// Configuration
$base-font-size: 16px;
$scale-ratio: 1.25;
$baseline: 8px;

// Échelle typographique
$font-xs: math.round($base-font-size * math.pow($scale-ratio, -2));   // 10px
$font-sm: math.round($base-font-size * math.pow($scale-ratio, -1));   // 13px
$font-base: $base-font-size;                                           // 16px
$font-md: math.round($base-font-size * math.pow($scale-ratio, 1));    // 20px
$font-lg: math.round($base-font-size * math.pow($scale-ratio, 2));    // 25px
$font-xl: math.round($base-font-size * math.pow($scale-ratio, 3));    // 31px
$font-xxl: math.round($base-font-size * math.pow($scale-ratio, 4));   // 39px

// Échelle d'espacement
$space-1: $baseline * 0.5;  // 4px
$space-2: $baseline;        // 8px
$space-3: $baseline * 1.5;  // 12px
$space-4: $baseline * 2;    // 16px
$space-5: $baseline * 3;    // 24px
$space-6: $baseline * 4;    // 32px
$space-7: $baseline * 6;    // 48px
$space-8: $baseline * 8;    // 64px

// Application
body {
  font-size: $font-base;
  line-height: 1.5;
}

h1 { font-size: $font-xxl; }
h2 { font-size: $font-xl; }
h3 { font-size: $font-lg; }
h4 { font-size: $font-md; }
h5 { font-size: $font-base; }
h6 { font-size: $font-sm; }

.card {
  padding: $space-5;
  margin-bottom: $space-4;
}
```

### Grille responsive avancée

```scss
@use 'sass:math';

$breakpoints: (
  "sm": 576px,
  "md": 768px,
  "lg": 992px,
  "xl": 1200px,
  "xxl": 1400px
);

$nbre-colonnes: 12;
$gouttiere: 30px;

// Génération des classes de grille
@each $nom, $largeur in $breakpoints {
  @media (min-width: $largeur) {
    @for $i from 1 through $nbre-colonnes {
      .col-#{$nom}-#{$i} {
        width: math.percentage(math.div($i, $nbre-colonnes));
        padding: 0 math.div($gouttiere, 2);
      }
    }

    .offset-#{$nom}-0 { margin-left: 0; }

    @for $i from 1 through ($nbre-colonnes - 1) {
      .offset-#{$nom}-#{$i} {
        margin-left: math.percentage(math.div($i, $nbre-colonnes));
      }
    }
  }
}
```

### Calculateur de taille responsive

```scss
@use 'sass:math';

$min-width: 320px;
$max-width: 1200px;
$min-font: 14px;
$max-font: 20px;

@function taille-responsive($min-size, $max-size) {
  $diff-font: $max-font - $min-font;
  $diff-screen: $max-width - $min-width;

  $slope: math.div($diff-font, $diff-screen);
  $intercept: $min-font - ($slope * $min-width);

  @return calc(#{$intercept} + #{math.percentage($slope)});
}

body {
  font-size: taille-responsive(14px, 20px);
}

h1 {
  font-size: taille-responsive(24px, 48px);
}
```

### Système de padding proportionnel

```scss
@use 'sass:math';

$ratios: (
  "golden": 1.618,
  "major-third": 1.25,
  "perfect-fourth": 1.333,
  "augmented-fourth": 1.414,
  "double-octave": 2
);

@each $nom, $ratio in $ratios {
  .padding-#{$nom} {
    padding: math.round(16px * $ratio);
  }
}
```

### Animation avec timing mathématique

```scss
@use 'sass:math';

$duration-base: 0.3s;
$easing: cubic-bezier(0.25, 0.1, 0.25, 1);

// Durées basées sur la racine carrée
$dur-1: math.round(math.sqrt(1) * $duration-base * 100) / 100 * 1s; // 0.3s
$dur-2: math.round(math.sqrt(2) * $duration-base * 100) / 100 * 1s; // 0.42s
$dur-3: math.round(math.sqrt(3) * $duration-base * 100) / 100 * 1s; // 0.52s
$dur-4: math.round(math.sqrt(4) * $duration-base * 100) / 100 * 1s; // 0.6s

.bouton {
  transition: all $dur-1 $easing;

  &:hover {
    transition-duration: $dur-2;
  }

  &:active {
    transition-duration: $dur-3;
  }
}
```

---

## 20.15 — Exercices

### Exercice 1 : Calculateur de grille

Créez un mixin qui génère une grille responsive avec un nombre configurable de colonnes et de gouttières. Utilisez `math.percentage()` et `math.div()`.

### Exercice 2 : Échelle typographique

Créez une échelle typographique basée sur le ratio d'or (1.618) en utilisant `math.pow()`. Générez les tailles de h1 à h6.

### Exercice 3 : Pagination

Créez un système de pagination qui calcule le nombre de pages nécessaires en utilisant `math.ceil()`.

### Exercice 4 : Limitation de valeurs

Créez un mixin qui limite une valeur entre un minimum et un maximum en utilisant `math.min()` et `math.max()`.

### Exercice 5 : Convertisseur d'unités

Créez des fonctions pour convertir entre différentes unités (px, em, rem, pt) en utilisant les fonctions mathématiques de Sass.

---

## 20.16 — Résumé

| Fonction | Description | Exemple | Résultat |
|----------|-------------|---------|----------|
| `math.div()` | Division | `math.div(10, 2)` | 5 |
| `math.round()` | Arrondir au plus proche | `math.round(4.5)` | 5 |
| `math.ceil()` | Arrondir au supérieur | `math.ceil(4.1)` | 5 |
| `math.floor()` | Arrondir à l'inférieur | `math.floor(4.9)` | 4 |
| `math.trunc()` | Tronquer les décimales | `math.trunc(4.7)` | 4 |
| `math.percentage()` | Convertir en pourcentage | `math.percentage(0.5)` | 50% |
| `math.min()` | Valeur minimale | `math.min(10, 5, 20)` | 5 |
| `math.max()` | Valeur maximale | `math.max(10, 5, 20)` | 20 |
| `math.abs()` | Valeur absolue | `math.abs(-5)` | 5 |
| `math.pow()` | Puissance | `math.pow(2, 3)` | 8 |
| `math.sqrt()` | Racine carrée | `math.sqrt(9)` | 3 |
| `math.log()` | Logarithme | `math.log(100, 10)` | 2 |

---

**Prochain chapitre :** Nous explorerons le module `sass:string` pour la manipulation de chaînes de caractères.
