# Chapitre 35 — Sécurité et Qualité du Code SCSS

## Introduction

La sécurité et la qualité du code SCSS sont souvent négligées, mais elles ont un impact direct sur la maintenabilité, les performances et la sécurité de l'application. Ce chapitre couvre les pièges de sécurité, les bonnes pratiques de qualité, le linting, et les revues de code.

---

## Sécurité en SCSS

### Règle n°1 : Ne jamais injecter de données utilisateur

```scss
// DANGER : injection de données utilisateur
// Si $user-input vient d'un champ de formulaire ou d'un paramètre URL

$user-input: $_GET['color']; // Ex: "../../etc/passwd" ou "red; } body{display:none"

// MAUVAIS : interpolation directe de données non contrôlées
.#{$user-input} {
  color: red;
}
// Résultat possible : .../../etc/passwd { color: red; }
// Ou pire : .red; } body{display:none { color: red; }

// MAUVAIS : construction de propriété avec interpolation
element {
  color: #{$user-input};
}
// Résultat : color: red; } body{display:none;
```

### Sélectionneurs générés de manière non contrôlée

```scss
// DANGER : construire des sélecteurs avec des valeurs dynamiques
$roles: $user-roles; // Valeur non contrôlée

@each $role in $roles {
  .user-role-#{$role} {
    // ...
  }
}

// Si $roles contient "admin; .hidden { display: none; } .evil"
// Résultat : .user-role-admin; .hidden { display: none; } .evil { ... }
```

### Solution : Validation et whitelist

```scss
// SÉCURISÉ : utilisation d'une whitelist
$allowed-colors: (
  primary: #3498db,
  secondary: #2ecc71,
  danger: #e74c3c,
  warning: #f39c12,
);

// Fonction sécurisée
@function safe-color($name) {
  @if map-has-key($allowed-colors, $name) {
    @return map-get($allowed-colors, $name);
  }

  @warn "Couleur `#{$name}` non autorisée. Couleur par défaut utilisée.";
  @return map-get($allowed-colors, 'primary');
}

// Utilisation
.element {
  color: safe-color($user-choice); // Toujours sécurisé
}

// SÉCURISÉ : validation des sélecteurs
$allowed-roles: 'admin', 'editor', 'viewer';

@each $role in $allowed-roles {
  @if index($allowed-roles, $role) {
    .user-role-#{$role} {
      // ...
    }
  }
}
```

### Interpolation et escaping

```scss
// L'interpolation #{ } peut causer des problèmes de sécurité
$unsafe: '../../etc/passwd';

// DANGER
.#{$unsafe} { }  // Génère .../../etc/passwd { }

// SÉCURISÉ : toujours valider ou utiliser des noms prédéfinis
$safe-slugs: ('admin', 'editor', 'viewer');

@each $slug in $safe-slugs {
  .role-#{$slug} { }
  // Génère : .role-admin, .role-editor, .role-viewer
}
```

---

## Qualité du code

### Limiter la profondeur de nesting

```scss
// RÈGLE : maximum 3 niveaux de nesting

// MAUVAIS : 5 niveaux
.page {
  .content {
    .section {
      .article {
        .title {
          font-size: 24px;
        }
      }
    }
  }
}

// BON : 2-3 niveaux
.article-title {
  font-size: 24px;
}

// OU avec BEM
.page__content-section-title {
  font-size: 24px;
}

// OU nesting limité
.content {
  .section-title {
    font-size: 24px;
  }
}
```

### Préférer @use/@forward à @import

```scss
// ANCIEN : @import (pollution du namespace global)
@import 'variables';
@import 'mixins';

// Les deux fichiers exportent $color-primary
// Conflit silencieux !

// MODERNE : @use (namespaces isolés)
@use 'variables' as vars;
@use 'mixins' as m;

.element {
  color: vars.$color-primary;  // Clair et sans conflit
  @include m.flex-center;
}
```

### Organiser les variables, mixins, fonctions

```scss
// ABSTRACTS : tout ce qui ne génère pas de CSS
// abstracts/_variables.scss
$color-primary: #3498db;
$spacing-md: 16px;
$font-size-base: 16px;

// abstracts/_mixins.scss
@mixin flex-center { }
@mixin respond-to($bp) { }

// abstracts/_functions.scss
@function rem($px) { }
@function spacing($n) { }

// abstracts/_placeholders.scss
%truncate { }
%list-reset { }
```

### CSS simple, lisible, performant

```scss
// MAUVAIS : CSS complexe et lent
.container {
  :not(:first-child):not(:last-child):nth-child(odd) {
    > *:not(:empty):not([hidden]) {
      display: block !important;
    }
  }
}

// BON : CSS simple et lisible
.container__item {
  display: block;
}

.container__item--hidden {
  display: none;
}
```

---

## Linting SCSS

### Stylelint

```bash
# Installation
npm install --save-dev stylelint stylelint-config-standard-scss

# Configuration
```

```javascript
// .stylelintrc.json
{
  "extends": [
    "stylelint-config-standard-scss",
    "stylelint-config-standard"
  ],
  "rules": {
    "scss/at-rule-no-unknown": true,
    "scss/no-unused-mixins": true,
    "scss/no-unused-variables": true,
    "selector-class-pattern": "^[a-z][a-z0-9]*(__[a-z][a-z0-9]*)?(--[a-z][a-z0-9]*)?$",
    "max-nesting-depth": 3,
    "no-descending-specificity": true,
    "declaration-block-no-redundant-longhand-properties": true,
    "length-zero-no-unit": true,
    "color-named": "never",
    "shorthand-property-no-redundant-values": true,
    "declaration-no-important": true,
    "selector-pseudo-class-no-unknown": true,
    "selector-pseudo-element-no-unknown": true,
    "property-no-unknown": true,
    "no-duplicate-selectors": true,
    "no-empty-source": true,
    "no-invalid-double-slash-comments": true,
    "scss/dollar-variable-no-unused": true,
    "scss/at-import-no-partial-extension": "always"
  },
  "ignoreFiles": [
    "node_modules/**",
    "dist/**"
  ]
}
```

### Règles importantes de Stylelint

```javascript
// .stylelintrc.json - configuration recommandée

// 1. Pattern BEM pour les classes
"selector-class-pattern": "^[a-z][a-z0-9]*(__[a-z][a-z0-9]*)?(--[a-z][a-z0-9]*)?$"

// 2. Pas de nesting au-delà de 3 niveaux
"max-nesting-depth": 3

// 3. Pas de !important
"declaration-no-important": true

// 4. Pas de couleurs nommées
"color-named": "never"

// 5. Valeurs raccourcies
"shorthand-property-no-redundant-values": true

// 6. Zéro sans unité
"length-zero-no-unit": true

// 7. Pas de sélecteurs descendants après des sélecteurs
"no-descending-specificity": true

// 8. Variables SCSS non utilisées
"scss/dollar-variable-no-unused": true

// 9. Mixins SCSS non utilisés
"scss/no-unused-mixins": true

// 10. Extension de fichier pour @import
"scss/at-import-no-partial-extension": "always"
```

### Intégration dans le projet

```json
// package.json
{
  "scripts": {
    "lint:scss": "stylelint 'src/scss/**/*.scss'",
    "lint:scss:fix": "stylelint 'src/scss/**/*.scss' --fix",
    "lint:scss:watch": "onchange 'src/scss/**/*.scss' -- npm run lint:scss"
  },
  "devDependencies": {
    "stylelint": "^15.0.0",
    "stylelint-config-standard-scss": "^9.0.0",
    "stylelint-config-standard": "^34.0.0"
  }
}
```

### Résultat du linting

```bash
$ npm run lint:scss

src/scss/components/_buttons.scss
  12:5  ✖  Unexpected !important  declaration-no-important
  15:3  ✖  Expected color to be named  color-named
  23:7  ✖  Unexpected nesting depth 3  max-nesting-depth

src/scss/abstracts/_variables.scss
  5:1   ✖  Unused variable  scss/dollar-variable-no-unused

✖ 4 problems (4 errors, 0 warnings)
```

---

## Revue de code (Code Review)

### Checklist de revue SCSS

```markdown
## Checklist de revue de code SCSS

### Sécurité
- [ ] Pas d'interpolation de données utilisateur
- [ ] Pas de construction de sélecteurs dynamiques non contrôlés
- [ ] Variables validées avant utilisation
- [ ] Aucune injection possible

### Nommage
- [ ] Variables descriptives (pas $bg, $bd, $fs)
- [ ] Mixins avec noms clairs
- [ ] Classes en BEM
- [ ] Fichiers en kebab-case

### Structure
- [ ] Un composant = un fichier
- [ ] Architecture respectée (7-1 ou ITCSS)
- [ ] Imports avec @use/@forward
- [ ] Variables partagées dans abstracts/

### Qualité
- [ ] Nesting ≤ 3 niveaux
- [ ] Pas de !important inutile
- [ ] Pas de sélecteurs trop spécifiques
- [ ] Code mort supprimé

### Performance
- [ ] Pas de boucles lourdes
- [ ] Opérations optimisées
- [ ] CSS final raisonnable en taille
- [ ] Pas de récursion profonde

### Maintenabilité
- [ ] Code documenté si complexe
- [ ] Variables configurables (!default)
- [ ] Compatible avec le système de build
- [ ] Pas de dépendances cachées
```

### Commentaires de revue

```scss
// === EXEMPLES DE COMMENTAIRE DE REVUE ===

// 1. Question de sécurité
// ⚠️ Cette variable est-elle toujours validée avant utilisation ?
// $user-choice est utilisé dans .#{$user-choice} sans validation

// 2. Suggestion d'amélioration
// 💡 Ce nesting peut être réduit en utilisant BEM
// Avant : .card { .title { ... } }
// Après : .card__title { ... }

// 3. Problème de performance
// ⚠️ Cette boucle génère 1000+ règles, envisagez une approche différente
@for $i from 1 through 1000 {
  .item-#{$i} { }
}

// 4. Convention
// 📝 Ce fichier devrait être dans abstracts/ car il ne génère pas de CSS
```

---

## Patterns de qualité

### Variables avec !default

```scss
// Toujours utiliser !default pour les variables configurables
$color-primary: #3498db !default;
$spacing-md: 16px !default;
$font-size-base: 16px !default;

// Permet à l'utilisateur de surcharger AVANT l'import
// main.scss
$color-primary: #e74c3c;  // Surcharge AVANT
@import 'abstracts/variables';  // !default ne s'applique pas
```

### Modules isolés

```scss
// Chaque module ne doit pas dépendre de l'état global

// MAUVAIS : dépendance à un ordre d'import global
@import 'variables';  // Doit être importé en premier
@import 'mixins';     // Doit être importé après variables

// BON : dépendances explicites via @use
// _buttons.scss
@use '../abstracts/variables' as *;
@use '../abstracts/mixins' as *;

.btn {
  color: $color-primary;  // Dépendance explicite
  @include flex-center;   // Dépendance explicite
}
```

### Pas de code mort

```scss
// MAUVAIS : code commenté ou mort
// .old-button {
//   background: red;
// }

// .deprecated-component {
//   display: none;
// }

$unused-variable: #333;  // Jamais utilisé

// BON : supprimer le code mort
// Si c'est dans Git, on peut toujours le retrouver
```

### Règles d'accessibilité

```scss
// Accessibilité dans le CSS
.element {
  // Pas de information uniquement visuelle
  // Utiliser des classes sémantiques

  // Focus visible obligatoire
  &:focus-visible {
    outline: 2px solid $color-primary;
    outline-offset: 2px;
  }

  // Respect de prefers-reduced-motion
  @media (prefers-reduced-motion: reduce) {
    * {
      animation-duration: 0.01ms !important;
      transition-duration: 0.01ms !important;
    }
  }
}
```

---

## Testing SCSS

### Tests visuels

```scss
// test/visual/buttons.test.scss
@use '../../src/abstracts' as *;
@use '../../src/components/buttons';

// Ce fichier peut être utilisé avec un outil de visual testing
// comme Percy, BackstopJS ou Chromatic
```

### Tests de compilation

```javascript
// test/scss.test.js
const sass = require('sass');
const path = require('path');

describe('SCSS Compilation', () => {
  test('main.scss compile sans erreur', () => {
    const result = sass.compile(
      path.join(__dirname, '../src/main.scss')
    );
    expect(result.css).toBeTruthy();
  });

  test('Pas de warnings de dépréciation', () => {
    const result = sass.compile(
      path.join(__dirname, '../src/main.scss'),
      { silenceDeprecations: [] }
    );
    expect(result.loadedUrls.length).toBe(0);
  });
});
```

---

## Métriques de qualité

```markdown
## Métriques à surveiller

### Taille
- CSS total : < 100KB gzippé
- Nombre de sélecteurs : < 2000
- Profondeur max de nesting : ≤ 3

### Performance
- Compilation : < 5 secondes
- Nombre de fichiers : raisonnable (pas > 100)

### Maintenance
- Code mort : 0%
- Variables non utilisées : 0
- Mixins non utilisés : 0
- Nesting > 3 niveaux : 0
```

---

## Résumé

La sécurité SCSS consiste à ne jamais interpoler de données non contrôlées. La qualité passe par un linting strict (Stylelint), un nesting limité, l'utilisation de @use/@forward, et des revues de code structurées. En combinant ces pratiques, on obtient un code SCSS sûr, maintenable et performant.
