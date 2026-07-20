# Chapitre 3 — Premier fichier SCSS

> **Niveau** : Débutant | **Durée estimée** : 45 minutes

---

## Objectifs du chapitre

À l'issue de ce chapitre, vous serez capable de :

- Créer un fichier SCSS structuré
- Compiler le fichier en CSS
- Analyser et comprendre le processus de compilation
- Identifier ce qui est préservé et ce qui est transformé
- Interpréter les différences entre le SCSS écrit et le CSS généré

---

## 1. Créer votre premier fichier SCSS

Dans ce chapitre, nous allons créer un fichier SCSS complet pour un composant de page web. Nous analyserons ensuite en détail le CSS généré pour comprendre exactement ce que fait le compilateur.

### Structure du projet

Créez cette structure de dossiers :

```
mon-projet/
├── scss/
│   └── style.scss
├── css/
│   └── (sera généré par la compilation)
└── index.html
```

```bash
mkdir -p mon-projet/scss mon-projet/css
```

### Le fichier style.scss

Créez le fichier `scss/style.scss` avec le contenu suivant :

```scss
// ============================================
// VARIABLES
// ============================================

// Couleurs
$color-primary: #3498db;
$color-primary-dark: darken($color-primary, 15%);
$color-primary-light: lighten($color-primary, 20%);

$color-secondary: #2ecc71;
$color-danger: #e74c3c;
$color-warning: #f39c12;

$color-text: #333333;
$color-text-light: #666666;
$color-bg: #ffffff;
$color-bg-alt: #f8f9fa;

// Typographie
$font-primary: "Helvetica Neue", Arial, sans-serif;
$font-secondary: "Georgia", serif;
$font-size-base: 16px;
$font-size-sm: 14px;
$font-size-lg: 18px;
$font-size-xl: 24px;
$line-height-base: 1.6;

// Espacements
$spacing-xs: 4px;
$spacing-sm: 8px;
$spacing-md: 16px;
$spacing-lg: 24px;
$spacing-xl: 32px;
$spacing-xxl: 48px;

// Bordures
$border-radius: 4px;
$border-radius-lg: 8px;
$border-color: #dee2e6;

// Ombres
$shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.12);
$shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
$shadow-lg: 0 10px 20px rgba(0, 0, 0, 0.15);

// ============================================
// MIXINS
// ============================================

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

@mixin transition($property: all, $duration: 0.3s, $easing: ease) {
  transition: $property $duration $easing;
}

@mixin responsive($breakpoint) {
  @if $breakpoint == mobile {
    @media (max-width: 576px) {
      @content;
    }
  } @else if $breakpoint == tablet {
    @media (max-width: 768px) {
      @content;
    }
  } @else if $breakpoint == desktop {
    @media (max-width: 992px) {
      @content;
    }
  }
}

// ============================================
// RESET & BASE
// ============================================

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: $font-primary;
  font-size: $font-size-base;
  line-height: $line-height-base;
  color: $color-text;
  background-color: $color-bg;
}

// ============================================
// TYPOGRAPHY
// ============================================

h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: $font-secondary;
  color: $color-text;
  line-height: 1.2;
  margin-bottom: $spacing-md;
}

h1 {
  font-size: $font-size-xl + 12px;
}

h2 {
  font-size: $font-size-xl;
}

h3 {
  font-size: $font-size-lg + 4px;
}

p {
  margin-bottom: $spacing-md;
}

a {
  color: $color-primary;
  text-decoration: none;

  &:hover {
    color: $color-primary-dark;
    text-decoration: underline;
  }
}

// ============================================
// LAYOUT
// ============================================

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 $spacing-md;

  @include responsive(mobile) {
    padding: 0 $spacing-sm;
  }
}

.section {
  padding: $spacing-xxl 0;

  &--alt {
    background-color: $color-bg-alt;
  }
}

// ============================================
// HEADER
// ============================================

.header {
  @include flex-between;
  padding: $spacing-md 0;
  border-bottom: 1px solid $border-color;

  &__logo {
    font-size: $font-size-xl;
    font-weight: bold;
    color: $color-primary;
  }

  &__nav {
    ul {
      @include flex-center;
      list-style: none;
      gap: $spacing-lg;
    }

    a {
      color: $color-text;
      font-weight: 500;
      @include transition;

      &:hover {
        color: $color-primary;
      }
    }
  }
}

// ============================================
// BUTTONS
// ============================================

.btn {
  display: inline-block;
  padding: $spacing-sm $spacing-md;
  font-size: $font-size-base;
  font-weight: 600;
  text-align: center;
  border: none;
  border-radius: $border-radius;
  cursor: pointer;
  @include transition;

  &--primary {
    background-color: $color-primary;
    color: $color-bg;

    &:hover {
      background-color: $color-primary-dark;
    }
  }

  &--secondary {
    background-color: $color-secondary;
    color: $color-bg;

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
      color: $color-bg;
    }
  }

  &--danger {
    background-color: $color-danger;
    color: $color-bg;

    &:hover {
      background-color: darken($color-danger, 10%);
    }
  }
}

// ============================================
// CARDS
// ============================================

.card {
  background-color: $color-bg;
  border: 1px solid $border-color;
  border-radius: $border-radius-lg;
  box-shadow: $shadow-sm;
  overflow: hidden;
  @include transition;

  &:hover {
    box-shadow: $shadow-md;
    transform: translateY(-2px);
  }

  &__image {
    width: 100%;
    height: 200px;
    object-fit: cover;
  }

  &__content {
    padding: $spacing-lg;
  }

  &__title {
    font-size: $font-size-lg;
    margin-bottom: $spacing-sm;
  }

  &__text {
    color: $color-text-light;
    margin-bottom: $spacing-md;
  }

  &__footer {
    padding: $spacing-md $spacing-lg;
    border-top: 1px solid $border-color;
    @include flex-between;
  }
}

// ============================================
// GRID SYSTEM
// ============================================

.grid {
  display: grid;
  gap: $spacing-lg;

  &--2 {
    grid-template-columns: repeat(2, 1fr);
  }

  &--3 {
    grid-template-columns: repeat(3, 1fr);
  }

  &--4 {
    grid-template-columns: repeat(4, 1fr);
  }

  @include responsive(tablet) {
    grid-template-columns: repeat(2, 1fr) !important;
  }

  @include responsive(mobile) {
    grid-template-columns: 1fr !important;
  }
}

// ============================================
// UTILITIES
// ============================================

.text-center {
  text-align: center;
}

.text-primary {
  color: $color-primary;
}

.mb-0 {
  margin-bottom: 0;
}
.mb-sm {
  margin-bottom: $spacing-sm;
}
.mb-md {
  margin-bottom: $spacing-md;
}
.mb-lg {
  margin-bottom: $spacing-lg;
}
.mb-xl {
  margin-bottom: $spacing-xl;
}

.hidden {
  display: none;
}

.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
```

Ce fichier est **conséquent** — il contient des variables, des mixins, de l'imbrication, des opérations, des media queries et des utilitaires. C'est un excellent exercice d'apprentissage.

---

## 2. Compiler le fichier

Lancez la compilation :

```bash
sass scss/style.scss css/style.css
```

Ou avec le mode surveillance :

```bash
sass --watch scss/style.scss:css/style.css
```

---

## 3. Analyser le processus de compilation

Le processus de compilation suit plusieurs étapes. Examinons-les une par une.

### Étape 1 : Résolution des variables

Le compilateur parcourt le fichier SCSS et **remplace chaque variable** par sa valeur.

**SCSS écrit :**

```scss
$color-primary: #3498db;

.btn {
  background-color: $color-primary;
}
```

**CSS généré :**

```css
.btn {
  background-color: #3498db;
}
```

La variable `$color-primary` n'existe plus dans le CSS final. Elle a été résolue pendant la compilation.

### Étape 2 : Résolution des opérations

Les opérations arithmétiques sont calculées à l'avance.

**SCSS écrit :**

```scss
$font-size-base: 16px;

h1 {
  font-size: $font-size-xl + 12px; // 24px + 12px
}
```

**CSS généré :**

```css
h1 {
  font-size: 36px;
}
```

Le calcul `24px + 12px = 36px` est effectué par le compilateur.

### Étape 3 : Application des fonctions intégrées

Les fonctions comme `darken()`, `lighten()`, `percentage()` sont résolues.

**SCSS écrit :**

```scss
$color-primary: #3498db;

.btn--primary {
  background-color: $color-primary;

  &:hover {
    background-color: darken($color-primary, 10%);
  }
}
```

**CSS généré :**

```css
.btn--primary {
  background-color: #3498db;
}

.btn--primary:hover {
  background-color: #2980b9;
}
```

La fonction `darken(#3498db, 10%)` a calculé la nouvelle couleur `#2980b9`.

### Étape 4 : Expansion des mixins

Chaque `@include` est remplacé par le contenu du mixin correspondant.

**SCSS écrit :**

```scss
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.header {
  @include flex-center;
}
```

**CSS généré :**

```css
.header {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

Le mixin a été **copié-collé** à la place du `@include`.

### Étape 5 : Résolution de l'imbrication (nesting)

Le `&` est remplacé par le sélecteur parent.

**SCSS écrit :**

```scss
.card {
  background-color: white;

  &__title {
    font-size: 18px;
  }

  &:hover {
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
}
```

**CSS généré :**

```css
.card {
  background-color: white;
}

.card__title {
  font-size: 18px;
}

.card:hover {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
```

### Étape 6 : Résolution des media queries

Les media queries imbriquées sont remontées au niveau supérieur.

**SCSS écrit :**

```scss
.container {
  max-width: 1200px;

  @media (max-width: 768px) {
    padding: 0 8px;
  }
}
```

**CSS généré :**

```css
.container {
  max-width: 1200px;
}

@media (max-width: 768px) {
  .container {
    padding: 0 8px;
  }
}
```

> **Règle importante** : Les media queries ne peuvent pas être imbriquées dans le CSS final. SASS les remonte automatiquement.

---

## 4. Ce qui est préservé vs ce qui change

### ✅ Ce qui est PRESERVÉ dans le CSS généré

| Élément           | Exemple                                       |
| ----------------- | --------------------------------------------- |
| Propriétés CSS    | `color`, `padding`, `margin`, `display`, etc. |
| Valeurs valides   | `#3498db`, `16px`, `100%`, `solid`, etc.      |
| Sélecteurs        | `.class`, `#id`, `element`, `:hover`, etc.    |
| Commentaires CSS  | `/* commentaires CSS */`                      |
| Règles @media     | `@media (max-width: 768px) { ... }`           |
| Règles @keyframes | `@keyframes animation { ... }`                |
| Règles @font-face | `@font-face { ... }`                          |

### ❌ Ce qui est TRANSFORMÉ ou SUPPRIMÉ

| Élément SCSS                  | Ce qui arrive                      |
| ----------------------------- | ---------------------------------- |
| `$variables`                  | Remplacées par leurs valeurs       |
| `@mixin` + `@include`         | Le mixin est copié-collé           |
| Imbrication `{}`              | Les sélecteurs sont reconstruits   |
| `&` (parent selector)         | Remplacé par le sélecteur parent   |
| Opérations `+`, `-`, `*`, `/` | Calculées à l'avance               |
| `darken()`, `lighten()`, etc. | Couleurs calculées                 |
| `@for`, `@each`, `@while`     | Boucles déroulées en code statique |
| `@if`, `@else`                | Branches résolues                  |
| `@import` / `@use`            | Fichiers assemblés en un seul      |
| Commentaires `//`             | Supprimés (non inclus dans le CSS) |
| `@extend`                     | Les sélecteurs sont fusionnés      |

---

## 5. Exercices pratiques

### Exercice 1 — Observer la compilation

1. Utilisez le fichier `style.scss` créé dans ce chapitre
2. Compilez-le en mode **expanded** (défaut) :
   ```bash
   sass scss/style.scss css/style-expanded.css
   ```
3. Compilez-le en mode **compressed** :
   ```bash
   sass --style=compressed scss/style.scss css/style-compressed.css
   ```
4. Comparez les tailles des deux fichiers avec :
   ```bash
   ls -la css/
   ```
5. Ouvrez chaque fichier et observez les différences

**Questions :**

- Quelle est la différence de taille entre les deux fichiers ?
- Combien de lignes contient chaque fichier ?
- Pouvez-vous retrouver les variables du SCSS dans le CSS généré ?

---

### Exercice 2 — Identifier les transformations

Créez le fichier `test.scss` avec ce code :

```scss
$primary: #3498db;
$spacing: 20px;

@mixin box-shadow($color) {
  box-shadow: 0 2px 4px $color;
}

@mixin centered {
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: $spacing;

  .header {
    @include centered;
    padding-bottom: $spacing / 2;

    h1 {
      color: $primary;
      font-size: 24px + 8px;
    }
  }

  .card {
    @include box-shadow(rgba(0, 0, 0, 0.1));
    background: $primary;
    border-radius: 4px;
    margin-bottom: $spacing * 1.5;

    &--highlighted {
      border: 2px solid $primary;
      background: lighten($primary, 45%);
    }

    &:hover {
      transform: translateY(-4px);
      @include box-shadow(rgba(0, 0, 0, 0.2));
    }
  }

  @media (max-width: 768px) {
    padding: $spacing / 2;

    .card {
      margin-bottom: $spacing;
    }
  }
}
```

Compilez le fichier, puis répondez aux questions :

1. **Variables** : Quelles lignes du CSS correspondent à des variables SCSS résolues ?
2. **Mixins** : Où voyez-vous le contenu des mixins copié-collé ?
3. **Imbrication** : Comment les sélecteurs `.container .header h1` sont-ils construits ?
4. **Opérations** : Quelles valeurs ont été calculées ?
5. **Media queries** : Comment la media query a-t-elle été remontée ?

---

### Exercice 3 — Créer votre propre fichier

Créez un fichier SCSS complet pour un composant **formulaire de contact** avec :

1. **Variables** pour :
   - La couleur principale
   - La police de caractères
   - Les espacements (small, medium, large)
   - Les couleurs d'état (succès, erreur, avertissement)

2. **Un mixin** pour les transitions

3. **Imbrication** pour :
   - Le conteneur du formulaire
   - Les champs de saisie (input, textarea)
   - Les labels
   - Les messages d'erreur et de succès
   - Le bouton de soumission

4. **Un état hover** pour le bouton

5. **Un media query** pour le responsive (mobile)

Compilez votre fichier et vérifiez que le CSS généré est correct.

---

## 6. Bonnes pratiques à retenir

### Toujours compiler avant d'utiliser

```bash
# ❌ Mauvais — le navigateur ne comprend pas le SCSS
<link rel="stylesheet" href="style.scss">

# ✅ Bon — le navigateur lit le CSS
<link rel="stylesheet" href="style.css">
```

### Utiliser le mode watch en développement

```bash
# Économise du temps de compilation manuelle
sass --watch scss/style.scss:css/style.css
```

### Minifier pour la production

```bash
# CSS compact pour un chargement rapide
sass --style=compressed scss/style.scss css/style.min.css
```

### Structurer son SCSS

```scss
// 1. Variables (en haut)
$primary: #3498db;

// 2. Mixins
@mixin flex-center { ... }

// 3. Reset / Base
* { box-sizing: border-box; }

// 4. Composants
.header { ... }
.button { ... }

// 5. Utilités (en bas)
.hidden { display: none; }
```

---

## 7. Résumé

| Concept                     | Description                                      |
| --------------------------- | ------------------------------------------------ |
| Compilation                 | Transformation du SCSS en CSS par le compilateur |
| Variables résolues          | `$var` → valeur finale                           |
| Mixins expandus             | `@include` → code copié-collé                    |
| Imbrication résolue         | Sélecteurs imbriqués → sélecteurs plats          |
| Opérations calculées        | `$a + $b` → résultat numérique                   |
| Fonctions résolues          | `darken()` → couleur calculée                    |
| Media queries remontées     | Déplacées au niveau racine du CSS                |
| Commentaires `//` supprimés | Seuls les commentaires `/* */` sont conservés    |

---

## Pour aller plus loin

- [Documentation des types de données SASS](https://sass-lang.com/documentation/values/)
- [Guide des opérations](https://sass-lang.com/documentation/operators/)
- [Sass Playground](https://sass-lang.com/playground/)

---

**Prochaine étape** : [Niveau 2 — Les bases](../niveau-02-bases/chapitre-04-variables.md)
