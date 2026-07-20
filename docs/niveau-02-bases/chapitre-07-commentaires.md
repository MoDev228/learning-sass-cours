# Chapitre 7 : Les commentaires en Sass/SCSS

## Objectifs pédagogiques

À l'issue de ce chapitre, vous serez capable de :
- Utiliser les deux types de commentaires Sass/SCSS
- Comprendre comment la compilation gère les commentaires
- Documenter votre code efficacement
- Appliquer les bonnes pratiques de documentation
- Décider quand utiliser chaque type de commentaire

---

## Introduction

Les commentaires sont essentiels dans tout projet de développement. Ils permettent d'**expliquer le code**, de **documenter les décisions**, et de **faciliter la maintenance**. Sass/SCSS propose deux types de commentaires distincts, chacun avec un comportement différent lors de la compilation.

---

## 1. Les commentaires CSS : `/* */`

### 1.1 Syntaxe

Les commentaires CSS classiques sont délimités par `/*` et `*/`.

```scss
/* Ceci est un commentaire CSS */
.element {
  color: red; /* Ceci est un commentaire inline */
}
```

### 1.2 Caractéristiques principales

- **Conservés** lors de la compilation (par défaut en SCSS)
- **Multiligne** possible
- Peuvent contenir du texte sur plusieurs lignes

### 1.3 Exemples d'utilisation

```scss
/* ============================================
   HEADER COMPONENT
   ============================================
   Description : Composant d'en-tête principal
   Auteur : Jean Dupont
   Date : 2024-01-15
   ============================================ */

.header {
  /* Propriétés de base du header */
  background-color: #333;
  padding: 16px 24px;
  position: sticky;
  top: 0;
  z-index: 1000;

  /*
   * Le logo utilise un SVG inline
   * pour une meilleure résolution
   */
  .logo {
    height: 40px;
    width: auto;
    /* filter: invert(1); pour le mode sombre */
  }

  /* Navigation principale */
  nav {
    display: flex;
    gap: 20px;

    a {
      color: white;
      text-decoration: none;

      /*
       * Changement de couleur au survol
       * Utilise la variable $primary-color
       * définie dans _variables.scss
       */
      &:hover {
        color: #3498db;
      }
    }
  }
}
```

**CSS compilé (les commentaires sont conservés) :**

```css
/* ============================================
   HEADER COMPONENT
   ============================================
   Description : Composant d'en-tête principal
   Auteur : Jean Dupont
   Date : 2024-01-15
   ============================================ */
.header {
  /* Propriétés de base du header */
  background-color: #333;
  padding: 16px 24px;
  position: sticky;
  top: 0;
  z-index: 1000;
}

/*
 * Le logo utilise un SVG inline
 * pour une meilleure résolution
 */
.header .logo {
  height: 40px;
  width: auto;
}

/* Navigation principale */
.header nav {
  display: flex;
  gap: 20px;
}

.header nav a {
  color: white;
  text-decoration: none;
}

/*
 * Changement de couleur au survol
 * Utilise la variable $primary-color
 * définie dans _variables.scss
 */
.header nav a:hover {
  color: #3498db;
}
```

---

## 2. Les commentaires Sass : `//`

### 2.1 Syntaxe

Les commentaires Sass utilisent `//` et s'étendent jusqu'à la fin de la ligne.

```scss
// Ceci est un commentaire Sass
.element {
  color: red; // Ceci est un commentaire en fin de ligne
}
```

### 2.2 Caractéristiques principales

- **Supprimés** lors de la compilation par défaut
- **Uniquement sur une ligne** (pas de multiligne natif)
- Idéaux pour des notes rapides et du code interne

### 2.3 Exemples d'utilisation

```scss
// Variables du thème principal
$primary-color: #3498db;
$secondary-color: #2ecc71;
$danger-color: #e74c3c;

// Configuration de la grille
$grid-columns: 12;
$grid-gutter: 30px;

// Bouton principal
.button {
  padding: 12px 24px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.3s ease; // Transition douce

  // Variante primaire
  &--primary {
    background-color: $primary-color;
    color: white;

    &:hover {
      // Légèrement plus foncé au survol
      background-color: darken($primary-color, 10%);
    }
  }

  // Variante danger
  &--danger {
    background-color: $danger-color;
    color: white;
  }

  // État désactivé
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed; // Curseur interdit
  }
}

// TODO: Ajouter la variante outlined
// FIXME: Le border-radius ne s'applique pas sur Safari
// HACK: Utilisation de -webkit- pour la compatibilité
```

### 2.4 Multiligne avec `//`

Bien que `//` soit nativement uniligne, on peut créer des commentaires multilignes en répétant `//` :

```scss
// Ceci est un commentaire
// qui s'étend sur plusieurs lignes
// en répétant le symbole //

// another single-line comment
// another single-line comment
```

---

## 3. Comment la compilation gère les commentaires

### 3.1 Comportement en mode SCSS

En mode SCSS (fichiers `.scss`), le comportement dépend du type de commentaire :

```scss
// Fichier source : style.scss

/* Commentaire CSS conservé */
$color: red;

// Commentaire Sass supprimé
.element {
  color: $color; // Ceci sera supprimé
  /* Ceci sera conservé */
}
```

```css
/* Fichier compilé : style.css */

/* Commentaire CSS conservé */
.element {
  color: red;
  /* Ceci sera conservé */
}
```

### 3.2 Comportement en mode Sass (indented)

En mode Sass indented (fichiers `.sass`), les deux types de commentaires sont gérés différemment :

```sass
// Fichier source : style.sass

// Commentaire simple (supprimé)
.element
  color: red

/// Commentaire triple-slash (conservé en Sass indented)
.another
  color: blue
```

```css
/* Fichier compilé : style.css */

/// Commentaire triple-slash (conservé)
.another {
  color: blue;
}
```

### 3.3 Flag de compilation `--style`

Le mode de compilation peut affecter la conservation des commentaires :

```bash
// Mode expanded (défaut) - les commentaires /* */ sont conservés
sass --style=expanded input.scss output.css

// Mode compressed - TOUS les commentaires sont supprimés
sass --style=compressed input.scss output.css

// Mode compressed avec commentaire de licence conservé
sass --style=compressed --no-source-map input.scss output.css
```

### 3.4 Commentaire de licence (special comment)

Un commentaire CSS qui commence par `/*!` est **toujours conservé**, même en mode compressed :

```scss
/*!
 * Bootstrap v5.3.0 (https://getbootstrap.com/)
 * Copyright 2011-2024 The Bootstrap Authors
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/main/LICENSE)
 */

.element {
  color: red;
}
```

En mode compressed, seul le commentaire `/*!` sera conservé :

```css
/*!
 * Bootstrap v5.3.0 (https://getbootstrap.com/)
 * Copyright 2011-2024 The Bootstrap Authors
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/main/LICENSE)
 */
.element{color:red}
```

---

## 4. Quand utiliser chaque type

### 4.1 Utiliser `//` pour...

```scss
// 1. Notes de développement internes
// Cette fonctionnalité est en cours de développement
// Elle sera disponible dans la version 2.0

// 2. Rappels de code
// N'oublie pas de mettre à jour le breakpoint dans _responsive.scss

// 3. Explication rapide d'une ligne
background: rgba(0, 0, 0, 0.5); // Overlay semi-transparent

// 4. Désactiver temporairement du code
// .old-component {
//   display: block;
// }

// 5. TODO, FIXME, HACK
// TODO: Ajouter le support RTL
// FIXME: Bug d'overflow sur mobile
// HACK: Workaround pour un bug Chrome 90+

// 6. Séparateurs visuels dans le code
// ============================================
// SECTION : Composants de formulaire
// ============================================
```

### 4.2 Utiliser `/* */` pour...

```scss
/* 1. Documentation officielle du composant
   Ce document explique comment le composant Card
   est conçu et cómo l'utiliser correctement.
*/

/* 2. Informations de copyright/licence */
/*!
 * Mon Framework CSS v1.0.0
 * Copyright 2024 Mon Entreprise
 */

/* 3. Documentation pour les utilisateurs finaux
   Les classes utilitaires suivantes sont disponibles :
   - .mt-1 à .mt-5 (margin-top)
   - .mb-1 à .mb-5 (margin-bottom)
*/

/* 4. Notes de release */
/* Nouveauté en v2.0 : support du dark mode */
/* Supprimé en v3.0 : .legacy-component */

/* 5. Exemples d'utilisation
   <div class="card card--primary">
     <div class="card__header">Titre</div>
     <div class="card__body">Contenu</div>
   </div>
*/
```

### 4.3 Tableau comparatif

| Critère | `//` (Sass) | `/* */` (CSS) |
|---------|-------------|----------------|
| Conservation en compilation | Non (par défaut) | Oui |
| Multiligne | Non natif | Oui |
| Usage principal | Notes internes | Documentation externe |
| Visible dans le CSS final | Non | Oui |
| Compatible avec les outils CSS | Non | Oui |
| Utilisable dans les media queries | Non | Oui |
| License comments | Non | Oui (`/*!`) |

---

## 5. Documentation avec les commentaires

### 5.1 Structure de documentation de fichier

```scss
// ============================================================
// Fichier : _buttons.scss
// Description : Composants de boutons du design system
// Auteur : Équipe Design
// Version : 2.1.0
// Dernière mise à jour : 2024-03-15
// ============================================================
//
// Ce fichier contient tous les styles liés aux boutons.
// Les boutons suivent la méthodologie BEM :
//
// .button           → Élément de base
// .button--primary  → Modificateur de couleur
// .button--small    → Modificateur de taille
// .button__icon     → Élément interne
//
// Dépendances :
//   - _variables.scss (couleurs, espacements)
//   - _mixins.scss (media queries)
//
// ============================================================

// --- Importations ---
@import 'variables';
@import 'mixins';

// --- Variables locales ---
$button-border-radius: 6px;
$button-transition-speed: 0.3s;

// --- Styles de base ---
.button {
  // Base commune à tous les boutons
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 12px 24px;
  border: 2px solid transparent;
  border-radius: $button-border-radius;
  font-family: inherit;
  font-size: 1rem;
  font-weight: 600;
  line-height: 1;
  text-decoration: none;
  cursor: pointer;
  transition: all $button-transition-speed ease;
  user-select: none;

  // Focus visible pour l'accessibilité
  &:focus-visible {
    outline: 3px solid rgba($primary-color, 0.5);
    outline-offset: 2px;
  }

  // --- Modificateurs de couleur ---

  // Bouton primaire (action principale)
  &--primary {
    background-color: $primary-color;
    color: white;

    &:hover {
      background-color: darken($primary-color, 10%);
      transform: translateY(-1px);
      box-shadow: 0 4px 8px rgba($primary-color, 0.3);
    }

    &:active {
      transform: translateY(0);
    }
  }

  // Bouton secondaire (action secondaire)
  &--secondary {
    background-color: transparent;
    color: $primary-color;
    border-color: $primary-color;

    &:hover {
      background-color: $primary-color;
      color: white;
    }
  }

  // Bouton danger (destructif)
  &--danger {
    background-color: $danger-color;
    color: white;

    &:hover {
      background-color: darken($danger-color, 10%);
    }
  }

  // --- Modificateurs de taille ---

  &--small {
    padding: 8px 16px;
    font-size: 0.875rem;
  }

  &--large {
    padding: 16px 32px;
    font-size: 1.125rem;
  }

  // --- États ---

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    pointer-events: none;
  }

  // --- Éléments internes ---

  &__icon {
    margin-right: 8px;
    font-size: 1.2em;
    line-height: 1;
  }

  &__text {
    display: inline-block;
  }

  // --- Modificateurs combinés ---

  // Bouton pleine largeur
  &--block {
    display: flex;
    width: 100%;
  }

  // Bouton sans border-radius
  &--pill {
    border-radius: 100px;
  }
}

// --- Bouton groupe ---
.button-group {
  display: inline-flex;

  // Espacement entre les boutons
  .button + .button {
    margin-left: -2px; // chevauchement des borders
  }

  // Premier bouton
  .button:first-child {
    border-radius: $button-border-radius 0 0 $button-border-radius;
  }

  // Dernier bouton
  .button:last-child {
    border-radius: 0 $button-border-radius $button-border-radius 0;
  }

  // Boutons du milieu
  .button:not(:first-child):not(:last-child) {
    border-radius: 0;
  }
}
```

### 5.2 Documentation de mixin

```scss
/// Méthode pour générer des media queries responsives
///
/// @param {String} $breakpoint - Nom du breakpoint
///   Les breakpoints disponibles sont :
///   - 'sm' : 576px
///   - 'md' : 768px
///   - 'lg' : 992px
///   - 'xl' : 1200px
///   - 'xxl' : 1400px
///
/// @example scss
///   .element {
///     padding: 20px;
///
///     @include respond-to('md') {
///       padding: 10px;
///     }
///   }
///
/// @output
///   .element { padding: 20px; }
///   @media (max-width: 768px) {
///     .element { padding: 10px; }
///   }
///
/// @requires $breakpoints
@mixin respond-to($breakpoint) {
  $breakpoints: (
    'sm': 576px,
    'md': 768px,
    'lg': 992px,
    'xl': 1200px,
    'xxl': 1400px
  );

  @if not map-has-key($breakpoints, $breakpoint) {
    @error "Le breakpoint '#{$breakpoint}' n'existe pas. " +
           "Breakpoints disponibles: #{map-keys($breakpoints)}";
  }

  @media (max-width: map-get($breakpoints, $breakpoint)) {
    @content;
  }
}

/// Génère une grille CSS avec un nombre de colonnes configurables
///
/// @param {Number} $columns - Nombre de colonnes (défaut: 12)
/// @param {Number} $gutter - Espacement entre les colonnes (défaut: 30px)
///
/// @example scss
///   @include generate-grid(12, 20px);
///
/// @output
///   .row { display: flex; flex-wrap: wrap; margin: 0 -10px; }
///   .col { padding: 0 10px; }
///   .col-1 { width: 8.333%; }
///   .col-2 { width: 16.666%; }
///   ...
@mixin generate-grid($columns: 12, $gutter: 30px) {
  $half-gutter: $gutter / 2;

  .row {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -$half-gutter;
  }

  .col {
    padding: 0 $half-gutter;
    box-sizing: border-box;
  }

  @for $i from 1 through $columns {
    .col-#{$i} {
      width: percentage($i / $columns);
    }
  }
}
```

### 5.3 Documentation de fonction

```scss
/// Convertit une valeur en pixels vers des unités rem
///
/// @param {Number} $px - La valeur en pixels à convertir
/// @param {Number} $base - La taille de police de base (défaut: 16px)
/// @return {Number} La valeur en rem
///
/// @throws si $px n'est pas un nombre
///
/// @example scss
///   font-size: px-to-rem(24px);    // Retourne 1.5rem
///   font-size: px-to-rem(14px);    // Retourne 0.875rem
///   font-size: px-to-rem(32px, 20); // Retourne 1.6rem
@function px-to-rem($px, $base: 16) {
  @if type-of($px) != 'number' {
    @error "px-to-rem() attend un nombre, #{type-of($px)} donné.";
  }

  @return $px / $base * 1rem;
}

/// Retourne une couleur assombrie ou éclaircie
///
/// @param {Color} $color - La couleur de base
/// @param {Number} $amount - Le pourcentage d'assombrissement (1-100)
/// @param {String} $direction - 'darken' ou 'lighten' (défaut: 'darken')
/// @return {Color} La couleur modifiée
///
/// @example scss
///   $primary: adjust-color(#3498db, 15, 'darken');  // Plus foncé
///   $primary: adjust-color(#3498db, 15, 'lighten'); // Plus clair
@function adjust-color($color, $amount, $direction: 'darken') {
  @if $direction == 'darken' {
    @return darken($color, $amount);
  } @else if $direction == 'lighten' {
    @return lighten($color, $amount);
  } @else {
    @error "La direction doit être 'darken' ou 'lighten', '#{$direction}' donné.";
  }
}

/// Génère une ombre avec une couleur personnalisée
///
/// @param {Number} $x - Décalage horizontal
/// @param {Number} $y - Décalage vertical
/// @param {Number} $blur - Rayon de flou
/// @param {Color} $color - Couleur de l'ombre (défaut: rgba(0,0,0,0.1))
/// @return {List} La valeur box-shadow complète
///
/// @example scss
///   box-shadow: shadow(0, 2px, 4px);          // Ombre standard
///   box-shadow: shadow(0, 4px, 12px, #000);   // Ombre noire
@function shadow($x: 0, $y: 2px, $blur: 4px, $color: rgba(0, 0, 0, 0.1)) {
  @return $x $y $blur $color;
}
```

### 5.4 Documentation de variables

```scss
// ============================================================
// VARIABLES DU PROJET
// ============================================================
//
// Ce fichier contient toutes les variables du design system.
// Pour personnaliser, redéfinissez les variables AVANT
// l'import de ce fichier.
//
// Utilisation :
//   1. Copiez ce fichier en _custom-variables.scss
//   2. Modifiez les valeurs souhaitées
//   3. Importez _custom-variables.scss avant _variables.scss
//
// ============================================================

// --- COULEURS ---
// Palette de couleurs principales

// Couleur principale du brand
// Utilisée pour : liens, boutons principaux, accents
$color-primary: #3498db !default;

// Couleur secondaire
// Utilisée pour : boutons secondaires, highlights
$color-secondary: #95a5a6 !default;

// Couleur de succès
// Utilisée pour : messages de succès, badges validés
$color-success: #2ecc71 !default;

// Couleur de danger
// Utilisée pour : erreurs, suppressions, alertes critiques
$color-danger: #e74c3c !default;

// Couleur d'avertissement
// Utilisée pour : warnings, notifications importantes
$color-warning: #f39c12 !default;

// Couleur d'information
// Utilisée pour : messages informatifs, tips
$color-info: #17a2b8 !default;

// --- GRIS ---
// Échelle de gris pour les textes et backgrounds

$gray-100: #f8f9fa !default;  // Fond le plus clair
$gray-200: #e9ecef !default;
$gray-300: #dee2e6 !default;
$gray-400: #ced4da !default;
$gray-500: #adb5bd !default;
$gray-600: #6c757d !default;
$gray-700: #495057 !default;
$gray-800: #343a40 !default;
$gray-900: #212529 !default;  // Fond le plus foncé

// --- TYPOGRAPHIE ---

// Familles de polices
$font-family-sans: -apple-system, BlinkMacSystemFont, "Segoe UI",
                   Roboto, "Helvetica Neue", Arial, sans-serif !default;
$font-family-serif: Georgia, "Times New Roman", Times, serif !default;
$font-family-mono: SFMono-Regular, Menlo, Monaco, Consolas,
                   "Liberation Mono", "Courier New", monospace !default;

// Tailles de police
$font-size-base: 1rem !default;      // 16px
$font-size-sm: 0.875rem !default;    // 14px
$font-size-lg: 1.25rem !default;     // 20px
$font-size-xl: 1.5rem !default;      // 24px
$font-size-2xl: 2rem !default;       // 32px

// Poids de police
$font-weight-normal: 400 !default;
$font-weight-medium: 500 !default;
$font-weight-semibold: 600 !default;
$font-weight-bold: 700 !default;

// Hauteur de ligne
$line-height-tight: 1.2 !default;
$line-height-base: 1.5 !default;
$line-height-loose: 1.8 !default;

// --- ESPACEMENT ---
// Échelle d'espacement basée sur 4px

$spacing-1: 0.25rem !default;   // 4px
$spacing-2: 0.5rem !default;    // 8px
$spacing-3: 1rem !default;      // 16px
$spacing-4: 1.5rem !default;    // 24px
$spacing-5: 2rem !default;      // 32px
$spacing-6: 3rem !default;      // 48px
$spacing-8: 4rem !default;      // 64px

// --- BORDURES ---

$border-color: $gray-300 !default;
$border-width: 1px !default;
$border-radius-sm: 0.25rem !default;  // 4px
$border-radius-base: 0.5rem !default; // 8px
$border-radius-lg: 1rem !default;     // 16px
$border-radius-full: 9999px !default; // Cercle

// --- OMBRES ---

$shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05) !default;
$shadow-base: 0 2px 4px rgba(0, 0, 0, 0.1) !default;
$shadow-md: 0 4px 8px rgba(0, 0, 0, 0.12) !default;
$shadow-lg: 0 8px 16px rgba(0, 0, 0, 0.15) !default;
$shadow-xl: 0 16px 32px rgba(0, 0, 0, 0.2) !default;

// --- TRANSITIONS ---

$transition-fast: 0.15s ease !default;
$transition-base: 0.3s ease !default;
$transition-slow: 0.5s ease !default;

// --- Z-INDEX ---

$z-dropdown: 1000 !default;
$z-sticky: 1020 !default;
$z-fixed: 1030 !default;
$z-modal-backdrop: 1040 !default;
$z-modal: 1050 !default;
$z-popover: 1060 !default;
$z-tooltip: 1070 !default;
```

---

## 6. Bonnes pratiques

### 6.1 Règles générales

```scss
// ✅ BON : Commentaires explicites et utiles
// Le padding est de 16px pour respecter la grille 8px
padding: 16px;

// ✅ BON : Commentaires pour les non-évidents
// Utilisation de darken() car la variantheadhover du design
// demande une couleur 10% plus foncée
background-color: darken($primary-color, 10%);

// ❌ MAUVAIS : Commentaire redondant
// Définir la couleur en rouge
color: red;

// ❌ MAUVAIS : Commentaire obsolète ou erroné
//背景色设为蓝色 (Le commentaire dit bleu mais le code est rouge)
background-color: red;

// ✅ BON : Commentaire pour expliquer le POURQUOI, pas le QUOI
// Utilisation de !important car ce style doit surcharger
// les styles inline du composant React
color: red !important;
```

### 6.2 Convention de commentaires par type

```scss
// ═══════════════════════════════════════════════
// SECTION : Séparateur visuel pour les sections
// ═══════════════════════════════════════════════

// TODO : Tâche à faire (à ne pas oublier)
// TODO: Implémenter le dark mode

// FIXME : Bug connu à corriger
// FIXME: Le hover ne fonctionne pas sur Safari 14

// HACK : Solution temporaire / contournement
// HACK: Utilisation de -webkit- en attendant le fix

// NOTE : Information importante
// NOTE: Ce mixin est utilisé par 15+ composants

// WARNING : Avertissement
// WARNING: Ne pas supprimer, casserait la grille

// @todo : Variante de la syntaxe TODO
// @deprecated : Marquer comme obsolète
// @see : Référence à autre chose
```

### 6.3 Commentaires pour les personnes

```scss
// ✅ Bonnes pratiques :
//
// 1. Expliquez le POURQUOI, pas le QUOI
//    Le code explique le QUOI, le commentaire explique le POURQUOI
//
// 2. Soyez concis
//    Un commentaire sur 1 ligne > un paragraphe
//
// 3. Mettez à jour les commentaires
//    Un commentaire obsolète est pire que pas de commentaire
//
// 4. Utilisez les conventions (TODO, FIXME, etc.)
//    Elles facilitent la recherche dans le code
//
// 5. Documentez les API publiques
//    Les fonctions et mixins méritent une documentation complète

// ✅ Exemple de fonction bien documentée
/// Calcule la taille de police en rem à partir de pixels
///
/// Cette fonction est utilisée partout dans le design system
/// pour maintenir une cohérence typographique.
///
/// @param {Number} $size - Taille en pixels
/// @return {Number} Taille en rem
///
/// @example scss
///   h1 { font-size: rem(32px); }  // 2rem
///   p { font-size: rem(14px); }   // 0.875rem
@function rem($size) {
  @return $size / 16px * 1rem;
}
```

---

## 7. Gestion des commentaires en production

### 7.1 Suppression des commentaires de développement

```scss
// Pour la production, on peut vouloir supprimer certains commentaires

// Méthode 1 : Utiliser // pour les notes internes
// (automatiquement supprimé en compilation)

// Méthode 2 : Configurer le compilateur
// sass --style=compressed input.scss output.css

// Méthode 3 : Utiliser un plugin PostCSS
// postcss avec cssnano peut supprimer les commentaires
```

### 7.2 Préserver les commentaires de licence

```scss
// Pour préserver un commentaire en mode compressed
// utilisez le marqueur spécial /*! ... }*/

/*!
 * Mon Framework v2.0.0
 * Copyright 2024 Mon Entreprise
 * License: MIT
 */

// Ce commentaire sera supprimé en mode compressed
// Ceci est un commentaire de développement
```

### 7.3 Script de build avec gestion des commentaires

```json
// package.json
{
  "scripts": {
    "build:dev": "sass --style=expanded --watch src:dist",
    "build:prod": "sass --style=compressed --no-source-map src:dist",
    "build:docs": "sass --style=expanded src:docs"
  }
}
```

---

## 8. Exercices pratiques

### Exercice 1 : Identifier les types

Pour chaque situation, choisissez le bon type de commentaire :

```scss
// Situation 1 : Vous voulez documenter le composant pour d'autres développeurs
// → Réponse : /* */ ou //

// Situation 2 : Vous voulez laisser un rappel rapide sur une ligne de code
// → Réponse : // ou /* */

// Situation 3 : Vous voulez que le commentaire apparaisse dans le CSS final
// → Réponse : /* */ ou //

// Situation 4 : Vous voulez commenter du code temporairement
// → Réponse : // ou /* */

// Situation 5 : Vous voulez ajouter une licence
// → Réponse : /* */ ou //

// Réponses :
// 1. /* */ (documentation officielle)
// 2. // (note rapide)
// 3. /* */ (conservé en compilation)
// 4. // (commenter du code)
// 5. /*! */ (licence préservée)
```

### Exercice 2 : Documenter un mixin

```scss
// Écrivez la documentation complète pour ce mixin :
@mixin flex-center($direction: row) {
  display: flex;
  flex-direction: $direction;
  align-items: center;
  justify-content: center;
}

// Votre documentation doit inclure :
// - Description
// - @param pour chaque paramètre
// - @example avec entrée et sortie
// - @require si applicable
```

### Exercice 3 : Organiser un fichier

```scss
// Réorganisez ce fichier avec des commentaires appropriés :
$primary: #3498db;
$spacing: 16px;
$font: Arial;

@mixin center { display: flex; align-items: center; justify-content: center; }

.button { @include center; padding: $spacing; background: $primary; }

.card { padding: $spacing; border: 1px solid #ccc; }
```

### Exercice 4 : Bonnes pratiques

```scss
// Corrigez les commentaires de ce fichier :
// Rouge
background: red;

// Ceci définit la couleur
color: blue;

// Padding
padding: 10px;

// FONCTION : Cette fonction calcule un truc
@function calculate($a, $b) {
  @return $a * $b;
}
```

### Exercice 5 : Documentation de fonction

```scss
// Documentez complètement cette fonction :
@function spacing($size) {
  $sizes: (
    xs: 4px,
    sm: 8px,
    md: 16px,
    lg: 24px,
    xl: 32px,
    xxl: 48px
  );

  @if not map-has-key($sizes, $size) {
    @error "Taille '#{$size}' non trouvée";
  }

  @return map-get($sizes, $size);
}
```

### Exercice 6 : Système de commentaires

```scss
// Créez un système de commentaires pour un projet complet :
//
// 1. Un header de fichier avec les informations suivantes :
//    - Nom du fichier
//    - Description
//    - Auteur
//    - Version
//    - Dernière mise à jour
//    - Dépendances
//
// 2. Des sections séparées par des séparateurs visuels
//
// 3. Chaque mixin/fonction documentée avec :
//    - Description
//    - @param
//    - @return
//    - @example
//
// 4. Des TODO/FIXME pour les améliorations prévues
//
// 5. Un copyright en haut du fichier
```

---

## Résumé

| Type | Syntaxe | Conservé | Usage |
|------|---------|----------|-------|
| CSS | `/* ... */` | Oui | Documentation externe |
| Sass | `// ...` | Non | Notes internes |
| Licence | `/*! ... */` | Toujours | Copyright, licences |
| Multiligne CSS | `/* ... \n ... */` | Oui | Paragraphes de docs |
| Multiligne Sass | `// ...\n// ...` | Non | Notes longues internes |

---

## Fin du niveau 2

Félicitations ! Vous avez terminé le niveau 2 "Les bases de Sass/SCSS". Vous maîtrisez maintenant :

- **Chapitre 4** : Les variables (types, interpolation, portée, `!default`)
- **Chapitre 5** : L'imbrication (nesting, profondeur, spécificité)
- **Chapitre 6** : Le parent selector (`&`, BEM, pseudo-classes, combinateurs)
- **Chapitre 7** : Les commentaires (types, compilation, documentation)

Dans le prochain niveau, nous explorerons des concepts plus avancés comme les **mixins**, les **fonctions**, les **boucles** et les **opérations** en Sass/SCSS.
