# Chapitre 10 : La directive @forward

## Introduction

Dans ce chapitre, nous allons découvrir la directive **`@forward`**, un outil puissant pour créer des **API de style** propres et bien organisées. `@forward` permet de **ré-exporter** des modules Sass, offrant un contrôle précis sur ce qui est visible à l'extérieur d'un dossier. C'est la clé pour créer des bibliothèques de styles maintenables et modulaires.

---

## 10.1 Créer une API de style

### Le concept

Une **API de style** est un ensemble de fichiers organisés qui exposent une interface claire et contrôlée. L'utilisateur de l'API n'a pas besoin de connaître l'organisation interne — il importe un seul fichier et accède à tout ce dont il a besoin.

### Sans @forward

```scss
// Pour utiliser les styles d'un projet bien organisé, il faudrait écrire :
@use 'abstracts/colors' as c;
@use 'abstracts/variables' as v;
@use 'abstracts/mixins' as *;
@use 'abstracts/functions' as *;
@use 'components/buttons' as btn;
@use 'components/cards' as card;
@use 'components/forms' as form;

// → 7 lignes d'import, l'utilisateur doit connaître l'organisation interne
```

### Avec @forward

```scss
// abstracts/_index.scss — Le point d'entrée de l'API
@forward 'colors';
@forward 'variables';
@forward 'mixins';
@forward 'functions';

// main.scss — L'utilisateur importe UN SEUL fichier
@use 'abstracts';

// → Accès à colors.$primary, variables.$spacing-md, etc.
// → Un seul import, propre et simple
```

---

## 10.2 Ré-exporter des modules

### Syntaxe de base

```scss
// _index.scss (ou tout autre fichier "barrel")
@forward 'colors';
@forward 'variables';
@forward 'mixins';
@forward 'functions';
```

### Fonctionnement interne

Lorsque vous utilisez `@forward`, le fichier qui contient la directive **ré-exporte** les membres des modules转发és. Cela signifie que les modules转发és sont accessibles via le fichier转发é.

```scss
// _colors.scss
$primary: #3498db;
$secondary: #2ecc71;

// _index.scss
@forward 'colors';

// main.scss
@use 'index';
// → index.$primary n'est PAS accessible directement
// → Les membres de colors sont accessibles via le namespace du module转发é
```

### Différence entre @forward et @use

```scss
// @use : IMPORTE un module (le consomme)
@use 'colors';
// → colors.$primary accessible

// @forward : RÉ-EXPORTE un module (le rend disponible pour les importateurs)
@forward 'colors';
// → Les membres de colors sont disponibles via le fichier转发é
// → Mais le fichier转发é ne les consomme pas lui-même
```

---

## 10.3 @forward avec @use

### Le pattern d'utilisation typique

```scss
// abstracts/_index.scss
// Ce fichier forward tous les modules de abstracts

@forward 'colors';
@forward 'variables';
@forward 'mixins';
@forward 'functions';
```

```scss
// main.scss
@use 'abstracts' as a;

.button {
  background-color: a.colors.$primary;    // Accès via le namespace du forward
  padding: a.variables.$spacing-md;
  border-radius: a.variables.$border-radius;
  
  @include a.mixins.flex-center();
  width: a.functions.to-rem(200px);
}
```

### Avec un alias sur le forward

```scss
// abstracts/_index.scss
@forward 'colors';
@forward 'variables';
@forward 'mixins' as *;  // Forward le mixin sans namespace
@forward 'functions' as *; // Forward les fonctions sans namespace
```

```scss
// main.scss
@use 'abstracts' as a;
@use 'abstracts/mixins' as *;  // Double import possible

.button {
  background-color: a.colors.$primary;
  padding: a.variables.$spacing-md;
  @include flex-center();     // Accès direct via as *
}
```

---

## 10.4 Cacher et montrer avec @forward

### Le problème

Parfois, un module contient des éléments qui ne doivent **pas** être exposés à l'extérieur. Par exemple, des variables internes ou des mixins auxiliaires.

### La solution : hide

La syntaxe `@forward 'module' hide (membres)` permet de **cacher** certains membres du module转发é :

```scss
// _colors.scss
$primary: #3498db;          // Public
$secondary: #2ecc71;        // Public
$danger: #e74c3c;           // Public
$internal-gray: #666;       // Interne (ne doit pas être exposé)
$debug-color: red;          // Debug (ne doit pas être exposé)

@mixin public-mixin() { ... }   // Public
@mixin internal-helper() { ... } // Interne
```

```scss
// _index.scss
@forward 'colors' hide ($internal-gray, $debug-color, internal-helper);
// Seuls $primary, $secondary, $danger et public-mixin sont exposés
```

```scss
// main.scss
@use 'abstracts' as a;

.element {
  color: a.colors.$primary;         // ✅ OK
  color: a.colors.$internal-gray;   // ❌ Erreur ! Caché
}
```

### Cacher avec un pattern

```scss
// Cacher toutes les variables sauf celles listées
@forward 'colors' hide ($primary);  // Cache uniquement $primary

// Cacher un seul mixin
@forward 'mixins' hide (internal-helper);
```

### La solution : show

La syntaxe `@forward 'module' show (membres)` permet de **montrer uniquement** certains membres :

```scss
// _colors.scss
$primary: #3498db;
$secondary: #2ecc71;
$danger: #e74c3c;
$internal-gray: #666;
$debug-color: red;
```

```scss
// _index.scss
@forward 'colors' show ($primary, $secondary, $danger);
// Seules ces 3 variables sont exposées, tout le reste est caché
```

### show vs hide

```scss
// Utilisez SHOW quand vous voulez exposer PEU de choses
@forward 'colors' show ($primary, $secondary, $danger);
// → 3 variables exposées, le reste caché

// Utilisez HIDE quand vous voulez cacher PEU de choses
@forward 'colors' hide ($debug-color, $internal-gray);
// → Tout exposé sauf 2 variables
```

---

## 10.5 Configurer les modules转发és

### La configuration via @forward

```scss
// _theme.scss
$primary: blue !default;
$secondary: green !default;
$spacing: 16px !default;
```

```scss
// _index.scss
// Permettre la configuration des modules转发és
@forward 'theme';

// OU avec une configuration par défaut
@forward 'theme' with (
  $primary: #3498db
);
```

```scss
// main.scss
// L'utilisateur peut maintenant configurer les modules转发és
@use 'abstracts' with (
  $primary: #e74c3c
);
```

### Configuration avancée

```scss
// _index.scss
@forward 'colors' with (
  $primary: #3498db
);

@forward 'variables' with (
  $spacing-unit: 8px,
  $border-radius: 4px
);

@forward 'theme' with (
  $primary: #3498db,
  $secondary: #2ecc71
);
```

---

## 10.6 Patterns d'organisation modernes

### Pattern 1 : Barrel files (fichiers barrel)

```scss
// Chaque dossier a un _index.scss qui forward ses modules
// Ce fichier est le "barrel" du dossier

// abstracts/_index.scss
@forward 'colors';
@forward 'variables';
@forward 'mixins';
@forward 'functions';

// components/_index.scss
@forward 'buttons';
@forward 'cards';
@forward 'forms';
@forward 'modals';

// layout/_index.scss
@forward 'grid';
@forward 'header';
@forward 'footer';

// pages/_index.scss
@forward 'home';
@forward 'about';
@forward 'contact';
```

```scss
// main.scss — Importe les barrels
@use 'abstracts';
@use 'components';
@use 'layout';
@use 'pages';
```

### Pattern 2 : API publique vs interne

```scss
// abstracts/_index.scss
// API publique — ce que les autres dossiers peuvent utiliser
@forward 'colors';
@forward 'variables';
@forward 'mixins' as *;
@forward 'functions' as *;

// Cacher les détails d'implémentation
@forward 'internal-helpers' hide (debug, test);
@forward 'deprecated-utils' hide *;  // Cache tout
```

### Pattern 3 : Thèmes configurables

```scss
// themes/_index.scss
@forward 'light-theme' with (
  $bg-color: white,
  $text-color: #333,
  $border-color: #ddd
);

@forward 'dark-theme' with (
  $bg-color: #1a1a1a,
  $text-color: #f0f0f0,
  $border-color: #444
);

@forward 'high-contrast-theme' with (
  $bg-color: black,
  $text-color: yellow,
  $border-color: yellow
);
```

```scss
// main.scss
@use 'themes' with (
  $bg-color: #f5f5f5,  // Thème personnalisé
  $text-color: #222
);
```

### Pattern 4 : Bibliothèque de composants

```scss
// components/_index.scss
// Composants principaux (publics)
@forward 'buttons';
@forward 'cards';
@forward 'forms';
@forward 'modals';
@forward 'navigation';

// Composants internes (non exposés)
@forward 'internal/card-variants' hide *;
@forward 'internal/form-helpers' hide *;
```

### Pattern 5 : Organisation par responsabilité

```scss
// _index.scss — Point d'entrée principal
// 1. Abstracts (pas de CSS généré)
@forward 'abstracts/colors';
@forward 'abstracts/variables';
@forward 'abstracts/mixins' as *;
@forward 'abstracts/functions' as *;

// 2. Base (CSS minimal)
@forward 'base/reset';
@forward 'base/typography';

// 3. Layout (structures)
@forward 'layout/grid';
@forward 'layout/header';
@forward 'layout/footer';

// 4. Composants (éléments UI)
@forward 'components/buttons';
@forward 'components/cards';

// 5. Utilitaires (classes helper)
@forward 'utilities/spacing';
@forward 'utilities/text';
```

---

## 10.7 Cas d'utilisation pratiques

### Cas 1 : Créer une bibliothèque de design tokens

```scss
// tokens/_index.scss
@forward 'colors' show ($primary, $secondary, $success, $warning, $danger, $info);
@forward 'spacing' show ($xs, $sm, $md, $lg, $xl, $xxl);
@forward 'typography' show ($font-family, $font-size, $font-weight, $line-height);
@forward 'shadows' show ($shadow-sm, $shadow-md, $shadow-lg);
@forward 'border-radius' show ($radius-sm, $radius-md, $radius-lg, $radius-full);
```

```scss
// Utilisation de la bibliothèque
@use 'tokens' as t;

.button {
  background-color: t.colors.$primary;
  padding: t.spacing.$md t.spacing.$lg;
  font-family: t.typography.$font-family;
  border-radius: t.border-radius.$radius-md;
  box-shadow: t.shadows.$shadow-sm;
}
```

### Cas 2 : Système de thèmes dark mode

```scss
// themes/_index.scss
@forward 'light';
@forward 'dark';

// light.scss
$bg-primary: #ffffff;
$bg-secondary: #f8f9fa;
$text-primary: #212529;
$text-secondary: #6c757d;
$border-color: #dee2e6;
$shadow-color: rgba(0, 0, 0, 0.1);

// dark.scss
$bg-primary: #1a1a2e;
$bg-secondary: #16213e;
$text-primary: #e0e0e0;
$text-secondary: #a0a0a0;
$border-color: #333355;
$shadow-color: rgba(0, 0, 0, 0.3);
```

```scss
// main.scss
@use 'abstracts/colors' as c;

// Application du thème clair par défaut
@use 'themes/light' as theme;

// Mode sombre via une classe CSS
.dark-mode {
  @use 'themes/dark' as theme;
}

.card {
  background-color: theme.$bg-primary;
  color: theme.$text-primary;
  border: 1px solid theme.$border-color;
  box-shadow: 0 2px 8px theme.$shadow-color;
}
```

### Cas 3 : Bundle de composants

```scss
// components/_index.scss
// Forward tous les composants
@forward 'buttons';
@forward 'cards';
@forward 'forms';
@forward 'modals';
@forward 'alerts';
@forward 'badges';
@forward 'breadcrumbs';
@forward 'pagination';
@forward 'tabs';
@forward 'tooltips';
```

```scss
// Utilisateur importe uniquement ce dont il a besoin
@use 'components/buttons' as btn;
@use 'components/cards';

// OU importe tout
@use 'components';
```

### Cas 4 : Overrides et personnalisations

```scss
// _defaults.scss
// Valeurs par défaut configurables
$font-size-base: 16px !default;
$line-height-base: 1.5 !default;
$color-primary: #3498db !default;
$spacing-unit: 8px !default;

// _index.scss
@forward 'defaults';
```

```scss
// main.scss — Personnalisation
@use 'abstracts' with (
  $font-size-base: 18px,
  $color-primary: #e74c3c,
  $spacing-unit: 10px
);
```

---

## 10.8 Avantages de @forward

### 1. Encapsulation

```scss
// Les modules转发és sont encapsulés
// L'utilisateur ne voit que ce qui est exposé

// _internal.scss
$secret-variable: 42;        // Interne
$public-variable: 100;       // Public

@mixin secret-mixin() { ... } // Interne
@mixin public-mixin() { ... } // Public

// _index.scss
@forward 'internal' show ($public-variable, public-mixin);

// main.scss
@use 'index';
// → $public-variable accessible
// → $secret-variable inaccessible
```

### 2. Organisation claire

```scss
// L'utilisateur sait exactement où trouver quoi
@use 'abstracts';      // Variables, mixins, fonctions
@use 'components';     // Composants UI
@use 'layout';         // Mise en page

// Pas besoin de connaître les détails internes
```

### 3. Réutilisabilité

```scss
// La bibliothèque peut être partagée entre projets
// Le fichier _index.scss définit l'API publique
// Les détails d'implémentation sont cachés
```

### 4. Maintenance

```scss
// Modifier l'implémentation interne sans casser l'API publique
// Réorganiser les fichiers internes sans affecter les importateurs
```

---

## 10.9 Erreurs courantes

### Erreur 1 : @forward après @use

```scss
// ❌ ERREUR — @forward doit être avant @use
@use 'colors';
@forward 'mixins';  // ⚠️ @forward doit être avant tout @use

// ✅ CORRECT
@forward 'colors';
@forward 'mixins';
@use 'abstracts';
```

### Erreur 2 : Confondre @forward et @use

```scss
// @forward RÉ-EXPORTE (rend disponible)
// @use CONSOMME (importe pour usage propre)

// _index.scss
@forward 'colors';  // Rend colors disponible pour les importateurs

// main.scss
@use 'abstracts';    // Consomme l'API de abstracts
@use 'colors';       // ⚠️ Les deux importent colors, mais @use a priorité
```

### Erreur 3 : Utiliser des membres cachés

```scss
// _index.scss
@forward 'colors' show ($primary, $secondary);

// main.scss
@use 'abstracts' as a;
a.colors.$danger;  // ❌ Erreur ! $danger n'est pas dans show
```

### Erreur 4 : Oublier que @forward ne consomme pas

```scss
// _index.scss
@forward 'colors';
@forward 'mixins';
// Le fichier _index.scss ne peut PAS utiliser les membres de colors
// car il ne fait que les ré-exporter

// Pour utiliser les membres DANS _index.scss, il faut aussi @use
@forward 'colors';
@use 'colors' as c;  // Maintenant _index.scss peut utiliser c.$primary
```

---

## 10.10 Exercices

### Exercice 1 : Créer un barrel file

Créez la structure suivante et les fichiers barrel :

```
abstracts/
├── _colors.scss        (5+ couleurs)
├── _variables.scss     (5+ variables)
├── _mixins.scss        (3+ mixins)
├── _functions.scss     (2+ fonctions)
└── _index.scss         (barrel file qui forward tout)
```

### Exercice 2 : API publique vs interne

Créez un fichier `_utils.scss` contenant :

1. 3 fonctions publiques
2. 2 fonctions internes (à cacher)
3. 5 variables (3 publiques, 2 internes)
4. 2 mixins (1 public, 1 interne)

Puis créez un `_index.scss` qui forward uniquement les éléments publics.

```scss
// Votre _utils.scss
@function public-fn-1() { ... }
@function internal-fn-1() { ... }
$public-var-1: 10;
$internal-var: 20;
@mixin public-mixin() { ... }
@mixin internal-mixin() { ... }

// Votre _index.scss
@forward 'utils' show (???);
```

### Exercice 3 : Thème configurable

Créez un système de thèmes :

1. `_light.scss` et `_dark.scss` avec des variables !default
2. `_themes-index.scss` qui forward les deux thèmes
3. `main.scss` qui utilise le thème avec configuration

```scss
// light.scss
$bg: white !default;
$text: #333 !default;

// dark.scss
$bg: #1a1a1a !default;
$text: #f0f0f0 !default;

// themes-index.scss
@forward 'light';
@forward 'dark';

// main.scss
@use 'themes' with ($bg: #f5f5f5, $text: #222);
```

### Exercice 4 : Bibliothèque de composants

Créez une bibliothèque de composants avec `@forward` :

```
components/
├── _buttons.scss
├── _cards.scss
├── _forms.scss
├── _index.scss    (barrel)
```

Chaque composant doit :
- Importer les abstracts nécessaires avec `@use`
- Exposer des mixins pour générer les styles
- Être forwardé via `_index.scss`

### Exercice 5 : Migration vers @forward

Convertissez ce projet `@import` en projet `@use` + `@forward` :

```scss
// AVANT — main.scss
@import 'abstracts/colors';
@import 'abstracts/variables';
@import 'abstracts/mixins';
@import 'abstracts/functions';
@import 'base/reset';
@import 'base/typography';
@import 'layout/grid';
@import 'components/buttons';
@import 'components/cards';

// Créez les barrel files et le nouveau main.scss
```

---

## Résumé

| Concept | Syntaxe | Description |
|---------|---------|-------------|
| Ré-exporter | `@forward 'module'` | Rend un module disponible pour les importateurs |
| Cacher | `@forward 'module' hide (...)` | Cache certains membres |
| Montrer | `@forward 'module' show (...)` | Expose uniquement certains membres |
| Configurer | `@forward 'module' with (...)` | Configure les variables !default |
| Barrel file | `_index.scss` | Fichier qui forward tous les modules d'un dossier |
| Ordre | Avant @use | @forward doit apparaître avant @use |

### Quand utiliser @forward ?

```
@forward  →  Créer une API publique (barrel files)
@forward  →  Cacher des détails d'implémentation
@forward  →  Ré-exporter des modules dans un seul fichier
@forward  →  Créer des bibliothèques de composants
@use      →  Consommer un module dans votre code
```

---

**Prochain chapitre** : [Chapitre 11 : Structure professionnelle](chapitre-11-structure-pro.md) — Organisation complète d'un projet Sass professionnel.
