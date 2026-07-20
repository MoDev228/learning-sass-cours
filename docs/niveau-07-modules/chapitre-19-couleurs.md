# Chapitre 19 : Les Couleurs — Module `sass:color`

## Introduction

La gestion des couleurs est un aspect fondamental du design web. Sass fournit un module puissant `sass:color` qui permet de manipuler, ajuster et transformer les couleurs de manière programmatique. Ce module offre bien plus de possibilités que les simples valeurs hexadécimales ou RGB. Vous pouvez éclaircir, assombrir, saturer, désaturer, mixer et ajuster les teintes de vos couleurs directement dans votre code Sass. Ce chapitre explore en profondeur chaque fonction du module `sass:color` avec de nombreux exemples pratiques.

---

## 19.1 — Introduction au module `sass:color`

### Importation du module

En Sass moderne, le module `sass:color` s'importe avec la syntaxe `@use` :

```scss
@use 'sass:color';

.mon-bouton {
  background-color: color.adjust(#3498db, $lightness: -10%);
}
```

### Couleurs de base en Sass

Sass reconnaît plusieurs formats de couleurs :

```scss
// Hexadécimal
$couleur-hex: #3498db;
$couleur-hex-court: #fff;

// RGB
$couleur-rgb: rgb(52, 152, 219);
$couleur-rgba: rgba(52, 152, 219, 0.8);

// HSL
$couleur-hsl: hsl(204, 70%, 53%);
$couleur-hsla: hsl(204, 70%, 53%, 0.8);

// Noms de couleurs
$couleur-nom: blue;
$couleur-nom-fr: bleu; // Les noms français ne fonctionnent pas
```

---

## 19.2 — `color.adjust()` : Ajuster une couleur

La fonction `color.adjust()` est la fonction la plus polyvalente du module. Elle remplace les anciennes fonctions `lighten()`, `darken()`, `saturate()`, `desaturate()` et `adjust-hue()`.

### Syntaxe

```scss
color.adjust($couleur, $red: valeur, $green: valeur, $blue: valeur, $alpha: valeur, $hue: valeur, $saturation: valeur, $lightness: valeur, $whiteness: valeur, $blackness: valeur, $red-space: valeur, $green-space: valeur, $blue-space: valeur)
```

### Éclaircir avec `lightness`

```scss
$couleur-base: #3498db;

.bouton {
  background-color: $couleur-base;

  &:hover {
    background-color: color.adjust($couleur-base, $lightness: 10%);
  }

  &:active {
    background-color: color.adjust($couleur-base, $lightness: -5%);
  }

  &:disabled {
    background-color: color.adjust($couleur-base, $lightness: 20%);
    opacity: 0.6;
  }
}
```

### Assombrir avec `lightness` négatif

```scss
$couleur-base: #2ecc71;

.bouton-vert {
  background-color: $couleur-base;
  border: 2px solid color.adjust($couleur-base, $lightness: -20%);

  &:hover {
    background-color: color.adjust($couleur-base, $lightness: -10%);
  }
}
```

### Ajuster la teinte (hue)

```scss
$teinte-base: 200; // Bleu

.element-a {
  background-color: hsl($teinte-base, 70%, 50%);
}

.element-b {
  background-color: hsl($teinte-base + 30, 70%, 50%);
}

.element-c {
  background-color: hsl($teinte-base + 60, 70%, 50%);
}

.element-d {
  background-color: hsl($teinte-base - 30, 70%, 50%);
}
```

### Ajuster la saturation

```scss
$couleur-base: hsl(200, 70%, 50%);

.vivid {
  background-color: color.adjust($couleur-base, $saturation: 20%);
}

.muted {
  background-color: color.adjust($couleur-base, $saturation: -30%);
}

.grayscale {
  background-color: color.adjust($couleur-base, $saturation: -70%);
}
```

### Ajuster les composantes RGB

```scss
$couleur: rgb(100, 150, 200);

.ajuste-rouge {
  background-color: color.adjust($couleur, $red: 50);
}

.ajuste-vert {
  background-color: color.adjust($couleur, $green: -30);
}

.ajuste-bleu {
  background-color: color.adjust($couleur, $blue: 40);
}
```

### Ajuster l'alpha (transparence)

```scss
$couleur-base: #3498db;

.opaque {
  background-color: color.adjust($couleur-base, $alpha: 0.3);
}

.transparent {
  background-color: color.adjust($couleur-base, $alpha: -0.5);
}
```

### Ajuster whiteness et blackness

```scss
$couleur: #3498db;

.plus-claire {
  background-color: color.adjust($couleur, $whiteness: 10%);
}

.plus-sombre {
  background-color: color.adjust($couleur, $blackness: 10%);
}
```

---

## 19.3 — `color.scale()` : Scale une couleur

La fonction `color.scale()` permet d'ajuster une couleur de manière proportionnelle, en restant dans les limites valides (0% à 100%).

### Différence entre `adjust` et `scale`

```scss
$couleur: #3498db;

// adjust peut dépasser les limites
.exces-adjust {
  color: color.adjust($couleur, $lightness: 80%);
  // Peut devenir blanc si trop ajusté
}

// scale reste dans les limites
.securise-scale {
  color: color.scale($couleur, $lightness: 80%);
  // Ne dépassera jamais les limites valides
}
```

### Exemples de `color.scale()`

```scss
$couleur-primaire: #3498db;

.bouton-primaire {
  background-color: $couleur-primaire;
  color: white;

  &:hover {
    background-color: color.scale($couleur-primaire, $lightness: -15%);
  }

  &:focus {
    background-color: color.scale($couleur-primaire, $lightness: -20%);
  }

  &:disabled {
    background-color: color.scale($couleur-primaire, $lightness: 30%);
  }
}

.card {
  border-left: 4px solid $couleur-primaire;

  &:hover {
    border-left-color: color.scale($couleur-primaire, $saturation: 20%);
  }
}
```

### Système de couleurs avec scale

```scss
$couleur-base: hsl(210, 80%, 50%);

.palette {
  --50: color.scale($couleur-base, $lightness: 45%, $saturation: -20%);
  --100: color.scale($couleur-base, $lightness: 35%, $saturation: -15%);
  --200: color.scale($couleur-base, $lightness: 25%, $saturation: -10%);
  --300: color.scale($couleur-base, $lightness: 15%, $saturation: -5%);
  --400: color.scale($couleur-base, $lightness: 5%);
  --500: $couleur-base;
  --600: color.scale($couleur-base, $lightness: -5%);
  --700: color.scale($couleur-base, $lightness: -15%);
  --800: color.scale($couleur-base, $lightness: -25%);
  --900: color.scale($couleur-base, $lightness: -35%);
}
```

---

## 19.4 — `color.change()` : Changer une composante

La fonction `color.change()` remplace une composante entière d'une couleur sans en affecter les autres.

### Syntaxe

```scss
color.change($couleur, $red: valeur, $green: valeur, $blue: valeur, $alpha: valeur, $hue: valeur, $saturation: valeur, $lightness: valeur)
```

### Changer l'alpha

```scss
$couleur-base: #3498db;

.opaque {
  background-color: color.change($couleur-base, $alpha: 1);
}

.semi-transparent {
  background-color: color.change($couleur-base, $alpha: 0.5);
}

.transparent {
  background-color: color.change($couleur-base, $alpha: 0);
}
```

### Changer la teinte (hue)

```scss
$couleur-bleue: hsl(210, 70%, 50%);

.rouge {
  background-color: color.change($couleur-bleue, $hue: 0);
}

.vert {
  background-color: color.change($couleur-bleue, $hue: 120);
}

.jaune {
  background-color: color.change($couleur-bleue, $hue: 60);
}

.violet {
  background-color: color.change($couleur-bleue, $hue: 270);
}
```

### Changer la saturation

```scss
$couleur: hsl(200, 50%, 50%);

.vivid {
  background-color: color.change($couleur, $saturation: 100%);
}

.muted {
  background-color: color.change($couleur, $saturation: 20%);
}

.grayscale {
  background-color: color.change($couleur, $saturation: 0%);
}
```

### Changer la luminosité

```scss
$couleur: hsl(200, 70%, 50%);

.tres-claire {
  background-color: color.change($couleur, $lightness: 90%);
}

.claire {
  background-color: color.change($couleur, $lightness: 70%);
}

.moyenne {
  background-color: color.change($couleur, $lightness: 50%);
}

.foncee {
  background-color: color.change($couleur, $lightness: 30%);
}

.tres-foncee {
  background-color: color.change($couleur, $lightness: 10%);
}
```

### Changer les composantes RGB

```scss
$couleur: rgb(100, 150, 200);

.pure-rouge {
  background-color: color.change($couleur, $red: 255, $green: 0, $blue: 0);
}

.pure-vert {
  background-color: color.change($couleur, $red: 0, $green: 255, $blue: 0);
}

.pure-bleu {
  background-color: color.change($couleur, $red: 0, $green: 0, $blue: 255);
}
```

---

## 19.5 — `color.mix()` : Mélanger deux couleurs

La fonction `color.mix()` combine deux couleurs en proportion configurable.

### Syntaxe

```scss
color.mix($couleur1, $couleur2, $poids: 50%)
```

### Exemples de base

```scss
$rouge: #ff0000;
$bleu: #0000ff;

.melange-50-50 {
  background-color: color.mix($rouge, $bleu);
  // Résultat : violet
}

.melange-70-30 {
  background-color: color.mix($rouge, $bleu, 70%);
  // Plus rouge que violet
}

.melange-30-70 {
  background-color: color.mix($rouge, $bleu, 30%);
  // Plus bleu que violet
}
```

### Créer une palette avec mix

```scss
$primaire: #3498db;
$secondaire: #e74c3c;

.palette-mix {
  --primaire: $primaire;
  --secondaire: $secondaire;
  --melange-1: color.mix($primaire, $secondaire, 80%);
  --melange-2: color.mix($primaire, $secondaire, 60%);
  --melange-3: color.mix($primaire, $secondaire, 50%);
  --melange-4: color.mix($primaire, $secondaire, 40%);
  --melange-5: color.mix($primaire, $secondaire, 20%);
}
```

### Mélanger avec du blanc ou du noir

```scss
$couleur: #3498db;
$blanc: #ffffff;
$noir: #000000;

.eclaircie-25 {
  background-color: color.mix($blanc, $couleur, 25%);
}

.eclaircie-50 {
  background-color: color.mix($blanc, $couleur, 50%);
}

.foncee-25 {
  background-color: color.mix($noir, $couleur, 25%);
}

.foncee-50 {
  background-color: color.mix($noir, $couleur, 50%);
}
```

### Exemple pratique : dégradé de couleurs

```scss
$base: #6c5ce7;
$accent: #fd79a8;

.degrade {
  background: linear-gradient(
    135deg,
    color.mix($base, $accent, 100%),
    color.mix($base, $accent, 75%),
    color.mix($base, $accent, 50%),
    color.mix($base, $accent, 25%),
    color.mix($base, $accent, 0%)
  );
}
```

---

## 19.6 — `color.adjust-hue()` : Ajuster la teinte

La fonction `color.adjust-hue()` permet de déplacer une couleur sur le cercle chromatique.

### Syntaxe

```scss
color.adjust-hue($couleur, $degre)
```

### Exemples

```scss
$bleu: hsl(210, 70%, 50%);

.bleu-original { background-color: $bleu; }
.bleu-plus-teinte-30 {
  background-color: color.adjust-hue($bleu, 30deg);
}
.bleu-moins-teinte-30 {
  background-color: color.adjust-hue($bleu, -30deg);
}
```

### Créer des couleurs complémentaires

```scss
$primaire: #3498db;

.complementaire {
  background-color: color.adjust-hue($primaire, 180deg);
}

.triadique-1 {
  background-color: color.adjust-hue($primaire, 120deg);
}

.triadique-2 {
  background-color: color.adjust-hue($primaire, 240deg);
}
```

### Rotation chromatique complète

```scss
$base: hsl(0, 70%, 50%);

.cercle-chromatique {
  --teinte-0: hsl(0, 70%, 50%);
  --teinte-30: hsl(30, 70%, 50%);
  --teinte-60: hsl(60, 70%, 50%);
  --teinte-90: hsl(90, 70%, 50%);
  --teinte-120: hsl(120, 70%, 50%);
  --teinte-150: hsl(150, 70%, 50%);
  --teinte-180: hsl(180, 70%, 50%);
  --teinte-210: hsl(210, 70%, 50%);
  --teinte-240: hsl(240, 70%, 50%);
  --teinte-270: hsl(270, 70%, 50%);
  --teinte-300: hsl(300, 70%, 50%);
  --teinte-330: hsl(330, 70%, 50%);
}
```

---

## 19.7 — Fonctions d'accès aux propriétés de couleur

### `color.channel()`

Retourne la valeur d'un canal spécifique d'une couleur.

```scss
$couleur: rgba(52, 152, 219, 0.8);

.roUGE: color.channel($couleur, $red);
.vert: color.channel($couleur, $green);
.bleu: color.channel($couleur, $blue);
.alpha: color.channel($couleur, $alpha);
```

### `color.is-same()`

Vérifie si deux couleurs sont identiques.

```scss
$couleur1: #3498db;
$couleur2: #3498db;
$couleur3: #2ecc71;

@mixin check-identique($c1, $c2) {
  @if color.is-same($c1, $c2) {
    &::after { content: "Les couleurs sont identiques"; }
  } @else {
    &::after { content: "Les couleurs sont différentes"; }
  }
}
```

### `color.is-in-gamut()`

Vérifie si une couleur est dans l'espace colorimétrique sRGB.

```scss
$couleur: #3498db;

.test {
  @if color.is-in-gamut($couleur) {
    color: $couleur;
  } @else {
    color: color.adjust($couleur, $saturation: -10%);
  }
}
```

### `color.to-gamut()`

Convertit une couleur dans l'espace sRGB.

```scss
$couleur-originale: oklch(0.7, 0.15, 210);

.convertee {
  color: color.to-gamut($couleur-originale);
}
```

---

## 19.8 — Création d'un système de couleurs complet

### Système de design tokens

```scss
@use 'sass:color';

// Couleurs de base
$bleu-500: #3b82f6;
$vert-500: #22c55e;
$rouge-500: #ef4444;
$jaune-500: #eab308;
$violet-500: #8b5cf6;
$rose-500: #ec4899;

// Génération automatique des échelles
$bleu: (
  "50": color.scale($bleu-500, $lightness: 45%, $saturation: -20%),
  "100": color.scale($bleu-500, $lightness: 35%, $saturation: -15%),
  "200": color.scale($bleu-500, $lightness: 25%, $saturation: -10%),
  "300": color.scale($bleu-500, $lightness: 15%, $saturation: -5%),
  "400": color.scale($bleu-500, $lightness: 5%),
  "500": $bleu-500,
  "600": color.scale($bleu-500, $lightness: -5%),
  "700": color.scale($bleu-500, $lightness: -15%),
  "800": color.scale($bleu-500, $lightness: -25%),
  "900": color.scale($bleu-500, $lightness: -35%),
  "950": color.scale($bleu-500, $lightness: -45%),
);

// Utilisation des tokens
.bouton-primaire {
  background-color: map-get($bleu, "500");
  border-color: map-get($bleu, "600");
  color: white;

  &:hover {
    background-color: map-get($bleu, "600");
  }
}

.texte-primaire {
  color: map-get($bleu, "700");
}
```

### Système de thème

```scss
@use 'sass:color';

// Thème clair
$theme-clair: (
  "bg-primary": #ffffff,
  "bg-secondary": #f8f9fa,
  "bg-tertiary": #e9ecef,
  "text-primary": #212529,
  "text-secondary": #6c757d,
  "accent": #007bff,
);

// Thème sombre
$theme-sombre: (
  "bg-primary": #1a1a2e,
  "bg-secondary": #16213e,
  "bg-tertiary": #0f3460,
  "text-primary": #e0e0e0,
  "text-secondary": #b0b0b0,
  "accent": color.adjust(#007bff, $lightness: 10%),
);

@mixin appliquer-theme($theme) {
  background-color: map-get($theme, "bg-primary");
  color: map-get($theme, "text-primary");

  .secondaire {
    background-color: map-get($theme, "bg-secondary");
    color: map-get($theme, "text-secondary");
  }

  .accent {
    color: map-get($theme, "accent");
  }
}

body {
  @include appliquer-theme($theme-clair);
}

@media (prefers-color-scheme: dark) {
  body {
    @include appliquer-theme($theme-sombre);
  }
}
```

### Système de couleurs sémantiques

```scss
@use 'sass:color';

$succes: #22c55e;
$avertissement: #eab308;
$erreur: #ef4444;
$information: #3b82f6;

.alerte {
  padding: 15px 20px;
  border-radius: 8px;
  margin-bottom: 15px;

  &.succes {
    background-color: color.adjust($succes, $lightness: 40%);
    border: 1px solid color.adjust($succes, $lightness: -20%);
    color: color.adjust($succes, $lightness: -40%);
  }

  &.avertissement {
    background-color: color.adjust($avertissement, $lightness: 40%);
    border: 1px solid color.adjust($avertissement, $lightness: -20%);
    color: color.adjust($avertissement, $lightness: -40%);
  }

  &.erreur {
    background-color: color.adjust($erreur, $lightness: 40%);
    border: 1px solid color.adjust($erreur, $lightness: -20%);
    color: color.adjust($erreur, $lightness: -40%);
  }

  &.information {
    background-color: color.adjust($information, $lightness: 40%);
    border: 1px solid color.adjust($information, $lightness: -20%);
    color: color.adjust($information, $lightness: -40%);
  }
}
```

---

## 19.9 — Exemples pratiques avancés

### Bouton avec états de survol

```scss
@use 'sass:color';

$couleur-base: #6c5ce7;

.bouton {
  display: inline-block;
  padding: 12px 24px;
  background-color: $couleur-base;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;

  &:hover {
    background-color: color.adjust($couleur-base, $lightness: -10%);
    transform: translateY(-2px);
    box-shadow: 0 4px 15px color.change($couleur-base, $alpha: 0.4);
  }

  &:active {
    background-color: color.adjust($couleur-base, $lightness: -15%);
    transform: translateY(0);
  }

  &:focus {
    outline: 3px solid color.change($couleur-base, $alpha: 0.5);
    outline-offset: 2px;
  }

  &:disabled {
    background-color: color.adjust($couleur-base, $lightness: 30%);
    cursor: not-allowed;
    transform: none;
    box-shadow: none;
  }

  &.outline {
    background-color: transparent;
    border: 2px solid $couleur-base;
    color: $couleur-base;

    &:hover {
      background-color: $couleur-base;
      color: white;
    }
  }

  &.ghost {
    background-color: transparent;
    color: $couleur-base;

    &:hover {
      background-color: color.change($couleur-base, $alpha: 0.1);
    }
  }
}
```

### Système de badges

```scss
@use 'sass:color';

.badge {
  display: inline-block;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: bold;

  @each $nom, $couleur in (
    "info": #3498db,
    "succes": #22c55e,
    "avertissement": #eab308,
    "erreur": #ef4444,
    "neutre": #6b7280
  ) {
    &.#{$nom} {
      background-color: color.adjust($couleur, $lightness: 40%);
      color: color.adjust($couleur, $lightness: -40%);
    }
  }
}
```

### Carte avec ombres colorées

```scss
@use 'sass:color';

$couleurs-carte: (
  "bleu": #3b82f6,
  "vert": #22c55e,
  "violet": #8b5cf6,
  "rose": #ec4899
);

@each $nom, $couleur in $couleurs-carte {
  .carte-#{$nom} {
    background-color: white;
    border-radius: 12px;
    padding: 24px;
    box-shadow:
      0 4px 6px color.change($couleur, $alpha: 0.1),
      0 10px 15px color.change($couleur, $alpha: 0.1);

    &:hover {
      transform: translateY(-5px);
      box-shadow:
        0 10px 25px color.change($couleur, $alpha: 0.2),
        0 20px 40px color.change($couleur, $alpha: 0.15);
    }

    .titre {
      color: $couleur;
    }
  }
}
```

### Gradients animés

```scss
@use 'sass:color';

$couleur-1: #667eea;
$couleur-2: #764ba2;
$couleur-3: #f093fb;

.gradient-anime {
  background: linear-gradient(
    -45deg,
    color.mix($couleur-1, $couleur-2, 50%),
    $couleur-2,
    $couleur-3,
    color.mix($couleur-3, $couleur-1, 50%)
  );
  background-size: 400% 400%;
  animation: gradient-shift 15s ease infinite;
}

@keyframes gradient-shift {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
```

---

## 19.10 — Exercices

### Exercice 1 : Palette de couleurs

Créez une palette de 5 couleurs basées sur la couleur `#e74c3c` en utilisant `color.scale()` et `color.adjust()`. Chaque couleur doit avoir un nom (50, 100, 200, 300, 400).

### Exercice 2 : Thème dark mode

Créez un thème dark mode complet en utilisant `color.adjust()` pour assombrir les couleurs de votre thème clair existant.

### Exercice 3 : Boutons avec états

Créez un système de boutons complet avec les états : normal, hover, active, focus, disabled, outline et ghost. Utilisez `color.mix()` pour les mélanges.

### Exercice 4 : Système d'alertes colorées

Créez un système d'alertes qui génère automatiquement les couleurs de fond, de bordure et de texte pour chaque type d'alerte (succès, erreur, avertissement, info).

### Exercice 5 : Gradients

Créez 5 dégradés différents en utilisant `color.mix()` pour mélanger les couleurs de manière progressive.

---

## 19.11 — Résumé

| Fonction | Description | Exemple |
|----------|-------------|---------|
| `color.adjust()` | Ajuste une ou plusieurs composantes | `color.adjust($c, $lightness: 10%)` |
| `color.scale()` | Scale proportionnellement une composante | `color.scale($c, $lightness: 50%)` |
| `color.change()` | Remplace une composante entièrement | `color.change($c, $alpha: 0.5)` |
| `color.mix()` | Mélange deux couleurs | `color.mix($c1, $c2, 70%)` |
| `color.adjust-hue()` | Ajuste la teinte | `color.adjust-hue($c, 30deg)` |
| `color.channel()` | Retourne la valeur d'un canal | `color.channel($c, $red)` |
| `color.is-same()` | Compare deux couleurs | `color.is-same($c1, $c2)` |
| `color.is-in-gamut()` | Vérifie si dans sRGB | `color.is-in-gamut($c)` |
| `color.to-gamut()` | Convertit vers sRGB | `color.to-gamut($c)` |

---

**Prochain chapitre :** Nous explorerons le module `sass:math` pour les opérations mathématiques avancées.
