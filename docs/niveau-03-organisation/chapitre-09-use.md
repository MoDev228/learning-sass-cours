# Chapitre 9 : La directive @use

## Introduction

Dans ce chapitre, nous allons explorer la directive **`@use`**, la manière **moderne et recommandée** d'importer des modules en Sass. `@use` remplace l'ancienne directive `@import` en résolvant de nombreux problèmes liés à la portée des variables, aux conflits de noms et à la performance de compilation.

---

## 9.1 Pourquoi @use remplace @import ?

### Les problèmes de @import

La directive `@import` existait depuis les débuts de Sass, mais elle présentait plusieurs problèmes fondamentaux :

#### Problème 1 : Portée globale des variables

```scss
// _colors.scss
$primary-color: #3498db;

// main.scss
@import 'colors';
// $primary-color est maintenant dispo PARTOUT (portée globale)
// Cela peut créer des conflits avec d'autres variables
```

Avec `@import`, **toutes** les variables, mixins et fonctions des fichiers importés deviennent globales. Cela signifie que deux fichiers pourraient accidentellement écraser la même variable.

#### Problème 2 : Conflits de noms

```scss
// _module-a.scss
$spacing: 10px;  // Espacement petit

// _module-b.scss
$spacing: 20px;  // Espacement moyen

// main.scss
@import 'module-a';
@import 'module-b';
// ⚠️ CONFLIT ! $spacing vaut maintenant 20px (écrasé par module-b)
// La valeur de module-a est PERDUE
```

#### Problème 3 : Import en cascade

```scss
// _base.scss
@import 'variables';
@import 'mixins';
@import 'components';
// Tout est importé dans un seul scope global
// Impossible de contrôler ce qui est visible à l'extérieur
```

#### Problème 4 : Pas de contrôle sur l'exposition

```scss
// _helpers.scss contient des mixins ET des styles générés
@import 'helpers';
// → TOUT est importé, y compris les mixins internes
// → Impossible de n'importer QUE certaines choses
```

### La solution : @use

`@use` résout **tous** ces problèmes :

```scss
// _colors.scss
$primary-color: #3498db;

// main.scss
@use 'colors';
// $primary-color n'est accessible QUE via colors.$primary-color
// → Pas de portée globale !
// → Pas de conflits de noms !
```

---

## 9.2 Importer un module : @use "colors"

### Syntaxe de base

```scss
// _colors.scss
$primary: #3498db;
$secondary: #2ecc71;
$danger: #e74c3c;

@mixin color-scheme($bg, $text) {
  background-color: $bg;
  color: $text;
}
```

```scss
// main.scss
@use 'colors';

.button {
  // Pour accéder aux variables, on utilise le NOM DU FICHIER comme namespace
  background-color: colors.$primary;
  color: white;
}

.alert {
  background-color: colors.$danger;
  color: white;
}

.hero {
  @include colors.color-scheme(colors.$secondary, white);
}
```

### Le namespace

Le **namespace** est automatiquement dérivé du nom du fichier (sans l'underscore et sans l'extension) :

```scss
// Fichier          → Namespace
_colors.scss    →   colors
_variables.scss →   variables
_mixins.scss    →   mixins
_functions.scss →   functions
_buttons.scss   →   buttons
```

### Accéder aux membres

```scss
// Pour accéder à une VARIABLE : namespace.$variable
@use 'colors';
.element { color: colors.$primary; }

// Pour appeler un MIXIN : namespace.mixin-name()
@use 'mixins';
.element { @include mixins.flex-center(); }

// Pour appeler une FONCTION : namespace.function-name()
@use 'functions';
.element { width: functions.to-rem(16px); }
```

---

## 9.3 Alias : @use "colors" as c

### Pourquoi un alias ?

Parfois, le nom du fichier est long ou le namespace par défaut n'est pas pratique. Vous pouvez créer un **alias** :

```scss
// Au lieu de :
@use 'abstracts/colors';
.element { color: abstracts.colors.$primary; }  // Long !

// Utilisez un alias :
@use 'abstracts/colors' as c;
.element { color: c.$primary; }  // Court et pratique !
```

### Exemples d'alias

```scss
// Alias court pour un fichier long
@use 'abstracts/variables' as v;
@use 'abstracts/mixins' as m;
@use 'abstracts/functions' as fn;

.button {
  background-color: v.$color-primary;
  padding: v.$spacing-md;
  border-radius: v.$border-radius;
  
  @include m.transition(background-color);
  
  &:hover {
    background-color: m.darken-color(v.$color-primary, 10%);
  }
}
```

### Alias pour plusieurs fichiers

```scss
@use 'colors' as c;           // c.$primary, c.$secondary, etc.
@use 'variables' as v;        // v.$spacing-md, v.$border-radius, etc.
@use 'mixins' as m;           // m.flex-center(), m.respond-to(), etc.
@use 'functions' as fn;       // fn.to-rem(), fn.color(), etc.

.card {
  background-color: c.$white;
  padding: v.$spacing-lg;
  border-radius: v.$border-radius-lg;
  box-shadow: v.$shadow;
  
  @include m.respond-to('md') {
    padding: v.$spacing-xl;
  }
}
```

---

## 9.4 Le namespace expliqué

### Comment le namespace est déterminé

```scss
// Le namespace = nom du fichier SANS underscore SANS extension
@use 'colors';           // namespace = "colors"
@use '_colors';          // namespace = "colors" (underscore ignoré)
@use 'colors.scss';      // namespace = "colors" (extension ignorée)
@use 'abstracts/colors'; // namespace = "colors" (chemin ignoré)
```

### Namespace avec alias

```scss
@use 'colors' as c;      // namespace = "c"
@use 'colors' as palette; // namespace = "palette"
```

### Espaces de noms imbriqués

```scss
// Si vous importez plusieurs fichiers avec des noms similaires :
@use 'abstracts/colors' as ac;      // ac.$primary
@use 'components/colors' as cc;     // cc.$primary (différent !)

// Pas de conflit car les namespaces sont différents
```

### Membres accessibles via le namespace

```scss
// Tous les membres d'un module sont accessibles via le namespace :
@use 'colors';

colors.$primary        // Variable
colors.$secondary      // Variable
colors.$color-map      // Map
colors.primary-name()  // Fonction
colors.set-color()     // Mixin
```

---

## 9.5 Tout importer : @use "colors" as *

### Le problème du namespace

Parfois, écrire le namespace à chaque fois est pénible :

```scss
@use 'colors';

.button-primary { background-color: colors.$primary; }
.button-secondary { background-color: colors.$secondary; }
.button-danger { background-color: colors.$danger; }
.alert-info { background-color: colors.$info; }
.text-primary { color: colors.$primary; }
```

### La solution : as *

La syntaxe `as *` importe **tous les membres** dans l'espace de noms **global** (sans namespace) :

```scss
@use 'colors' as *;

.button-primary { background-color: $primary; }
.button-secondary { background-color: $secondary; }
.button-danger { background-color: $danger; }
.alert-info { background-color: $info; }
.text-primary { color: $primary; }
```

### Attention aux conflits

```scss
// _colors.scss
$primary: #3498db;

// _buttons.scss
$primary: #e74c3c;  // ⚠️ Même nom !

// main.scss
@use 'colors' as *;
@use 'buttons' as *;
// ⚠️ CONFLIT ! $primary est ambiguous
```

### Bonnes pratiques pour `as *`

```scss
// ✅ BON — Utiliser as * pour les mixins et fonctions (pas de conflits probables)
@use 'mixins' as *;
@use 'functions' as *;

.element {
  @include flex-center();    // Direct, sans namespace
  width: to-rem(16px);       // Direct, sans namespace
}

// ✅ BON — Garder le namespace pour les variables (éviter les conflits)
@use 'colors';
@use 'variables';

.element {
  color: colors.$primary;          // Namespace explicite
  padding: variables.$spacing-md;  // Namespace explicite
}

// ❌ ÉVITER — Utiliser as * pour les variables quand il y a un risque de conflit
@use 'colors' as *;
@use 'variables' as *;
// Si colors.$spacing et variables.$spacing existent → conflit
```

---

## 9.6 Configurer des modules : @use "colors" with (...)

### Le concept de configuration

Certains modules Sass acceptent des **paramètres de configuration**. La syntaxe `with (...)` permet de personnaliser un module lors de son importation.

### Exemple : un module configurable

```scss
// _theme.scss
// Ce module définit un thème par défaut qui peut être configuré

$primary-color: #3498db !default;
$secondary-color: #2ecc71 !default;
$border-radius: 4px !default;
$font-family-base: 'Arial', sans-serif !default;
$font-size-base: 16px !default;

@mixin apply-theme() {
  color: $primary-color;
  border-radius: $border-radius;
  font-family: $font-family-base;
  font-size: $font-size-base;
}
```

### Utiliser la configuration

```scss
// Import avec la configuration par défaut
@use 'theme';

.element {
  @include theme.apply-theme();
}

// Import avec une configuration personnalisée
@use 'theme' with (
  $primary-color: #e74c3c,
  $font-family-base: 'Georgia', serif,
  $font-size-base: 18px
);

.element {
  @include theme.apply-theme();
  // → Utilise le rouge, Georgia, et 18px
}
```

### La directive !default

Le flag `!default` est **essentiel** pour que la configuration fonctionne :

```scss
// _config.scss
$color: blue !default;   // ← Le !default permet la surcharge
$spacing: 16px !default;

// main.scss
@use 'config' with ($color: red);
// $color = red (la valeur fournie écrase le défaut)

@use 'config2';  // Sans with(...)
// $color = blue (la valeur par défaut est utilisée)
```

**Sans `!default`**, la valeur `with (...)` ne peut PAS écraser la variable.

### Exemple complet : un module de thème configuré

```scss
// _alert.scss
$alert-padding: 12px !default;
$alert-border-radius: 4px !default;
$alert-font-size: 14px !default;

$alert-colors: (
  "success": #27ae60,
  "warning": #f39c12,
  "danger": #e74c3c,
  "info": #3498db
) !default;

@mixin alert($type) {
  padding: $alert-padding;
  border-radius: $alert-border-radius;
  font-size: $alert-font-size;
  background-color: map-get($alert-colors, $type);
  color: white;
  border: none;
}

@mixin alert-outline($type) {
  @include alert($type);
  background-color: transparent;
  color: map-get($alert-colors, $type);
  border: 2px solid map-get($alert-colors, $type);
}
```

```scss
// Configuration personnalisée dans main.scss
@use 'alert' with (
  $alert-padding: 16px,
  $alert-border-radius: 8px,
  $alert-colors: (
    "success": #00c853,
    "warning": #ff9100,
    "danger": #ff1744,
    "info": #2979ff
  )
);

.alert-success { @include alert('success'); }
.alert-warning { @include alert('warning'); }
.alert-danger { @include alert('danger'); }
.alert-info { @include alert('info'); }
```

---

## 9.7 Plusieurs règles @use

### Ordre des @use

Les règles `@use` doivent **toutes** apparaître **avant** toute autre règle Sass (hors `@forward`) :

```scss
// ✅ BON — Tous les @use d'abord
@use 'abstracts/colors';
@use 'abstracts/variables';
@use 'abstracts/mixins' as *;
@use 'abstracts/functions' as *;

// Ensuite, le reste du code
.button {
  background-color: colors.$primary;
  padding: $spacing-md;
}

// ❌ MAUVAIS — @use après du code
.button {
  background-color: colors.$primary;
}

@use 'abstracts/colors';  // ⚠️ ERREUR ! @use doit être en premier
```

### Nombre de @use

```scss
// Il n'y a PAS de limite au nombre de @use
// Mais gardez-les organisés :

// Abstracts
@use 'abstracts/colors' as c;
@use 'abstracts/variables' as v;
@use 'abstracts/mixins' as *;
@use 'abstracts/functions' as *;

// Base
@use 'base/reset';
@use 'base/typography';

// Layout
@use 'layout/grid' as g;
@use 'layout/header';
@use 'layout/footer';

// Components
@use 'components/buttons' as btn;
@use 'components/cards';
@use 'components/forms' as form;

// Le reste du code suit...
```

### Utilisation multiple d'un même module

```scss
// Vous pouvez @use un module plusieurs fois
// Mais cela ne charge le module qu'UNE SEULE FOIS

@use 'colors';
@use 'colors';  // Pas de problème, Sass optimise

// Équivalent à :
@use 'colors';
```

---

## 9.8 Exemples pratiques complets

### Exemple 1 : Système de boutons

```scss
// _buttons.scss
@use 'abstracts/colors' as c;
@use 'abstracts/variables' as v;
@use 'abstracts/mixins' as *;

// Mixin pour générer un bouton
@mixin button-variant($bg-color, $hover-color: null) {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: v.$spacing-sm v.$spacing-lg;
  border: none;
  border-radius: v.$border-radius;
  background-color: $bg-color;
  color: c.$color-text-inverse;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  @include transition(all);
  
  @if $hover-color {
    &:hover {
      background-color: $hover-color;
    }
  } @else {
    &:hover {
      background-color: darken($bg-color, 10%);
    }
  }
  
  &:active {
    transform: scale(0.98);
  }
  
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}

// Mixin pour bouton outline
@mixin button-outline($border-color) {
  @include button-variant(transparent);
  background-color: transparent;
  color: $border-color;
  border: 2px solid $border-color;
  
  &:hover {
    background-color: $border-color;
    color: c.$color-text-inverse;
  }
}

// Mixin pour taille
@mixin button-size($padding-y, $padding-x, $font-size) {
  padding: $padding-y $padding-x;
  font-size: $font-size;
}
```

```scss
// main.scss
@use 'abstracts/colors' as c;
@use 'abstracts/variables' as v;
@use 'components/buttons' as btn;

.btn {
  @include btn.button-variant(c.$color-primary);
  
  &--secondary {
    @include btn.button-variant(c.$color-secondary);
  }
  
  &--danger {
    @include btn.button-variant(c.$color-danger);
  }
  
  &--outline {
    @include btn.button-outline(c.$color-primary);
  }
  
  &--sm {
    @include btn.button-size(v.$spacing-xs, v.$spacing-sm, 0.875rem);
  }
  
  &--lg {
    @include btn.button-size(v.$spacing-md, v.$spacing-xl, 1.25rem);
  }
}
```

### Exemple 2 : Système de grilles

```scss
// _grid.scss
@use 'abstracts/variables' as v;

$grid-columns: 12 !default;
$grid-gutter: v.$spacing-md !default;
$grid-breakpoints: (
  "sm": v.$breakpoint-sm,
  "md": v.$breakpoint-md,
  "lg": v.$breakpoint-lg,
  "xl": v.$breakpoint-xl
) !default;

@mixin container {
  width: 100%;
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
  padding-left: $grid-gutter / 2;
  padding-right: $grid-gutter / 2;
}

@mixin row {
  display: flex;
  flex-wrap: wrap;
  margin-left: -$grid-gutter / 2;
  margin-right: -$grid-gutter / 2;
}

@mixin col($columns) {
  flex: 0 0 percentage($columns / $grid-columns);
  max-width: percentage($columns / $grid-columns);
  padding-left: $grid-gutter / 2;
  padding-right: $grid-gutter / 2;
}

@mixin col-auto {
  flex: 0 0 auto;
  width: auto;
}
```

```scss
// Utilisation
@use 'layout/grid' as g;

.container {
  @include g.container;
}

.row {
  @include g.row;
}

.col-6 {
  @include g.col(6);
}

.col-4 {
  @include g.col(4);
}

.col-md-8 {
  @include g.respond-to('md') {
    @include g.col(8);
  }
}
```

### Exemple 3 : Système de tokens de design

```scss
// _tokens.scss
$tokens: (
  "colors": (
    "primary": #3498db,
    "secondary": #2ecc71,
    "danger": #e74c3c
  ),
  "spacing": (
    "xs": 4px,
    "sm": 8px,
    "md": 16px,
    "lg": 24px,
    "xl": 32px
  ),
  "font-sizes": (
    "small": 12px,
    "base": 16px,
    "large": 20px,
    "xlarge": 24px
  ),
  "border-radius": (
    "small": 4px,
    "medium": 8px,
    "large": 16px,
    "full": 9999px
  )
) !default;

@function token($category, $name) {
  @return map-get(map-get($tokens, $category), $name);
}

@mixin token-style($category, $name, $property) {
  #{$property}: token($category, $name);
}
```

```scss
// Utilisation
@use 'abstracts/tokens' as t;

.button {
  background-color: t.token("colors", "primary");
  padding: t.token("spacing", "sm") t.token("spacing", "md");
  border-radius: t.token("border-radius", "medium");
  font-size: t.token("font-sizes", "base");
}

.card {
  padding: t.token("spacing", "lg");
  border-radius: t.token("border-radius", "large");
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
```

---

## 9.9 Migration de @import vers @use

### Guide de migration

```scss
// ❌ AVANT (avec @import)
@import 'abstracts/colors';
@import 'abstracts/variables';
@import 'abstracts/mixins';

.button {
  background-color: $primary-color;    // Portée globale
  padding: $spacing-md;                // Portée globale
  @include flex-center();              // Portée globale
}

// ✅ APRÈS (avec @use)
@use 'abstracts/colors' as c;
@use 'abstracts/variables' as v;
@use 'abstracts/mixins' as *;

.button {
  background-color: c.$primary-color;   // Namespace explicite
  padding: v.$spacing-md;                // Namespace explicite
  @include flex-center();                // as * → direct
}
```

### Règles de migration

```scss
// 1. Remplacer @import par @use
@import 'colors';      →  @use 'colors';
@import 'mixins';      →  @use 'mixins' as *;

// 2. Ajouter le namespace aux variables
$primary-color;        →  colors.$primary-color;

// 3. Les mixins/fonctions avec as * restent identiques
@mixin flex-center();  →  @include flex-center();  // Pas de changement

// 4. Les configurations
@import 'theme';       →  @use 'theme' with ($primary: red);
```

---

## 9.10 Erreurs courantes

### Erreur 1 : @use après du code

```scss
// ❌ ERREUR
.button { color: red; }
@use 'colors';  // ⚠️ @use doit être en premier !

// ✅ CORRECT
@use 'colors';
.button { color: colors.$primary; }
```

### Erreur 2 : Confondre namespace et fichier

```scss
// ❌ ERREUR
@use 'abstracts/colors';
.element { color: colors.$primary; }
// ⚠️ Le namespace est "colors", pas "abstracts" !

// ✅ CORRECT
@use 'abstracts/colors';
.element { color: colors.$primary; }  // ✅
```

### Erreur 3 : Oublier le !default pour la configuration

```scss
// _theme.scss
$primary: blue;  // ❌ Pas de !default
// La valeur ne pourra JAMAIS être configurée via with (...)

// _theme.scss
$primary: blue !default;  // ✅ Avec !default
// Maintenant configurable via with (...)
```

### Erreur 4 : Utiliser un namespace après as *

```scss
@use 'colors' as *;

.element {
  color: colors.$primary;  // ❌ ERREUR ! Pas de namespace avec as *
  color: $primary;          // ✅ CORRECT
}
```

---

## 9.11 Exercices

### Exercice 1 : Migration @import → @use

Convertissez le code suivant de `@import` vers `@use` :

```scss
// AVANT — À convertir
@import 'abstracts/colors';
@import 'abstracts/variables';
@import 'abstracts/mixins';
@import 'abstracts/functions';

.card {
  background-color: $white;
  padding: $spacing-lg;
  border-radius: $border-radius-lg;
  box-shadow: $shadow;
  
  &__title {
    color: $primary-color;
    font-size: to-rem(24px);
  }
  
  &__content {
    @include truncate(3);
  }
}
```

```scss
// APRÈS — Votre code ici
@use '???';
@use '???';

.card {
  background-color: ???;
  // ...
}
```

### Exercice 2 : Configuration de module

Créez un module `_card-configurable.scss` qui accepte une configuration :

```scss
// _card-configurable.scss
// Définissez les variables avec !default
// Créez un mixin @include card-style() qui utilise ces variables
```

Puis utilisez-le avec une configuration personnalisée dans `main.scss`.

### Exercice 3 : Namespace et alias

```scss
// Étant donné ces fichiers :
// abstracts/colors.scss     → $primary, $secondary, $danger
// abstracts/spacing.scss    → $sm, $md, $lg, $xl
// abstracts/mixins.scss     → flex-center(), respond-to()
// abstracts/functions.scss  → to-rem(), em()

// Écrivez les directives @use avec des alias appropriés
// puis créez un composant .navigation qui utilise tous ces modules
```

### Exercice 4 : Système de tokens

Créez un fichier `_design-tokens.scss` contenant :

1. Une map de tokens organisée par catégorie (colors, spacing, typography, shadows)
2. Une fonction `token($category, $name)` pour accéder aux tokens
3. Un mixin `apply-token($category, $name, $property)` pour appliquer un token
4. Utilisez-le pour styliser un composant `.user-profile`

### Exercice 5 : Comparaison @import vs @use

Écrivez un tableau comparatif montrant les différences entre `@import` et `@use` sur ces aspects :

| Aspect | @import | @use |
|--------|---------|------|
| Portée des variables | ? | ? |
| Conflits de noms | ? | ? |
| Configuration | ? | ? |
| Performance | ? | ? |
| Contrôle de l'exposition | ? | ? |

---

## Résumé

| Concept | Syntaxe | Description |
|---------|---------|-------------|
| Import basique | `@use 'module'` | Importe un module avec un namespace |
| Alias | `@use 'module' as alias` | Crée un raccourci pour le namespace |
| Tout importer | `@use 'module' as *` | Importe sans namespace (global) |
| Configuration | `@use 'module' with (...)` | Configure les variables !default du module |
| Namespace | `module.$variable` | Accède aux membres du module |
| Obligation | En tête de fichier | Tous les @use avant tout autre code |

### Quand utiliser chaque syntaxe ?

```
@use 'module'         → Variables (pour éviter les conflits)
@use 'module' as *    → Mixins et fonctions (pour la simplicité)
@use 'module' as x    → Alias court pour des noms de fichiers longs
@use 'module' with    → Configurer un module à l'importation
```

---

**Prochain chapitre** : [Chapitre 10 : @forward](chapitre-10-forward.md) — Créer des API de style et ré-exporter des modules.
