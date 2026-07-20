# Chapitre 8 : Les fichiers partiels

## Introduction

Dans ce chapitre, nous allons découvrir les **fichiers partiels** en Sass, un concept fondamental pour organiser votre code CSS de manière professionnelle. Les fichiers partiels vous permettent de découper vos feuilles de style en petits modules réutilisables, facilitant ainsi la maintenance, la collaboration et l'évolutivité de vos projets.

---

## 8.1 Pourquoi utiliser les fichiers partiels ?

### Le problème du CSS classique

Dans un projet CSS traditionnel, tout le code se trouve dans un seul fichier (ou quelques gros fichiers). Cela pose plusieurs problèmes :

- **Difficulté de navigation** : Trouver un style spécifique dans un fichier de 5000 lignes est fastidieux.
- **Risque de conflits** : Modifier un style peut avoir des effets de bord imprévus.
- **Difficulté de réutilisation** : Extraire un composant pour un autre projet nécessite du copier-coller.
- **Maintenance lourde** : Plusieurs développeurs travaillant sur le même fichier créent des conflits de fusion.

### La solution : la modularisation

Les fichiers partiels permettent de **découper** votre code en modules logiques :

```
scss/
├── _colors.scss        → Variables de couleurs
├── _typography.scss    → Styles de typographie
├── _buttons.scss       → Composant boutons
├── _forms.scss         → Composant formulaires
├── _layout.scss        → Mise en page
└── main.scss           → Point d'entrée (compile uniquement celui-ci)
```

---

## 8.2 La convention du tiret basse (underscore)

### La règle fondamentale

En Sass, un fichier **partiel** est un fichier dont le nom commence par un **underscore** `_`. Cet underscore indique au compilateur Sass que ce fichier **ne doit pas être compilé directement** en CSS.

```scss
// Fichier : _colors.scss
// Ce fichier ne sera JAMAIS compilé seul en colors.css

$primary-color: #3498db;
$secondary-color: #2ecc71;
$danger-color: #e74c3c;
```

### Différence avec un fichier normal

| Fichier | Compilation | Utilisation |
|---------|-------------|-------------|
| `colors.scss` | Génère `colors.css` | Peut être compilé directement |
| `_colors.scss` | **Aucune compilation** | Importé uniquement par d'autres fichiers |

### Ce qui se passe lors de la compilation

Quand vous compilez votre projet, Sass ignore automatiquement les fichiers commençant par `_` :

```bash
# Si vous avez ces fichiers :
# _colors.scss
# _buttons.scss
# main.scss

sass src/scss/main.scss dist/css/main.css

# Résultat : SEUL main.css est généré
# _colors.scss et _buttons.scss ne génèrent RIEN
```

---

## 8.3 Créer un fichier `_colors.scss`

### Exemple complet

Créez le fichier `src/scss/abstracts/_colors.scss` :

```scss
// ============================================
// Fichier : _colors.scss
// Description : Palette de couleurs du projet
// ============================================

// --- Couleurs primaires ---
$color-primary: #3498db;
$color-primary-light: #5dade2;
$color-primary-dark: #2980b9;
$color-primary-ultra-dark: #1a5276;

// --- Couleurs secondaires ---
$color-secondary: #2ecc71;
$color-secondary-light: #58d68d;
$color-secondary-dark: #27ae60;

// --- Couleurs d'accentuation ---
$color-accent: #f39c12;
$color-accent-light: #f5b041;
$color-accent-dark: #d68910;

// --- Couleurs neutres ---
$color-white: #ffffff;
$color-black: #000000;
$color-gray-100: #f8f9fa;
$color-gray-200: #e9ecef;
$color-gray-300: #dee2e6;
$color-gray-400: #ced4da;
$color-gray-500: #adb5bd;
$color-gray-600: #6c757d;
$color-gray-700: #495057;
$color-gray-800: #343a40;
$color-gray-900: #212529;

// --- Couleurs sémantiques ---
$color-success: #27ae60;
$color-warning: #f39c12;
$color-danger: #e74c3c;
$color-info: #3498db;

// --- Couleurs de fond ---
$color-bg-primary: $color-white;
$color-bg-secondary: $color-gray-100;
$color-bg-dark: $color-gray-900;

// --- Couleurs de texte ---
$color-text-primary: $color-gray-900;
$color-text-secondary: $color-gray-600;
$color-text-muted: $color-gray-500;
$color-text-inverse: $color-white;

// --- Maps de couleurs (pour les boucles Sass) ---
$colors: (
  "primary": $color-primary,
  "secondary": $color-secondary,
  "success": $color-success,
  "warning": $color-warning,
  "danger": $color-danger,
  "info": $color-info
);

// --- Échelle de gris (map) ---
$grays: (
  "100": $color-gray-100,
  "200": $color-gray-200,
  "300": $color-gray-300,
  "400": $color-gray-400,
  "500": $color-gray-500,
  "600": $color-gray-600,
  "700": $color-gray-700,
  "800": $color-gray-800,
  "900": $color-gray-900
);
```

---

## 8.4 Créer d'autres fichiers partiels

### `_variables.scss` – Variables globales

```scss
// ============================================
// Fichier : _variables.scss
// Description : Variables globales du projet
// ============================================

// --- Breakpoints ---
$breakpoint-sm: 576px;
$breakpoint-md: 768px;
$breakpoint-lg: 992px;
$breakpoint-xl: 1200px;
$breakpoint-xxl: 1400px;

// --- Z-index ---
$z-dropdown: 1000;
$z-sticky: 1020;
$z-fixed: 1030;
$z-modal-backdrop: 1040;
$z-modal: 1050;
$z-popover: 1060;
$z-tooltip: 1070;
$z-toast: 1080;

// --- Transitions ---
$transition-speed: 0.3s;
$transition-easing: cubic-bezier(0.4, 0, 0.2, 1);

// --- Espacement ---
$spacing-unit: 8px;
$spacing-xs: $spacing-unit * 0.5;   // 4px
$spacing-sm: $spacing-unit;          // 8px
$spacing-md: $spacing-unit * 2;      // 16px
$spacing-lg: $spacing-unit * 3;      // 24px
$spacing-xl: $spacing-unit * 4;      // 32px
$spacing-xxl: $spacing-unit * 6;     // 48px

// --- Bordures ---
$border-radius-sm: 3px;
$border-radius: 5px;
$border-radius-lg: 10px;
$border-radius-pill: 50px;
$border-radius-circle: 50%;

// --- Ombres ---
$shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.12);
$shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
$shadow-lg: 0 4px 16px rgba(0, 0, 0, 0.2);
$shadow-xl: 0 8px 32px rgba(0, 0, 0, 0.25);
```

### `_mixins.scss` – Mixins réutilisables

```scss
// ============================================
// Fichier : _mixins.scss
// Description : Mixins utilitaires réutilisables
// ============================================

// --- Responsive breakpoints ---
@mixin respond-to($breakpoint) {
  @if $breakpoint == "sm" {
    @media (min-width: $breakpoint-sm) { @content; }
  } @else if $breakpoint == "md" {
    @media (min-width: $breakpoint-md) { @content; }
  } @else if $breakpoint == "lg" {
    @media (min-width: $breakpoint-lg) { @content; }
  } @else if $breakpoint == "xl" {
    @media (min-width: $breakpoint-xl) { @content; }
  }
}

// --- Flexbox center ---
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

// --- Flexbox between ---
@mixin flex-between {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

// --- Grid automatique ---
@mixin auto-grid($min-width: 250px, $gap: $spacing-md) {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax($min-width, 1fr));
  gap: $gap;
}

// --- Truncate text ---
@mixin truncate($lines: 1) {
  @if $lines == 1 {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  } @else {
    display: -webkit-box;
    -webkit-line-clamp: $lines;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
}

// --- Position absolute centré ---
@mixin absolute-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

// --- Clearfix ---
@mixin clearfix {
  &::after {
    content: "";
    display: table;
    clear: both;
  }
}

// --- Visually hidden (accessibilité) ---
@mixin visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

// --- Transition personnalisée ---
@mixin transition($properties...) {
  $result: ();
  @each $prop in $properties {
    $result: append($result, $prop $transition-speed $transition-easing, comma);
  }
  transition: $result;
}
```

### `_functions.scss` – Fonctions utilitaires

```scss
// ============================================
// Fichier : _functions.scss
// Description : Fonctions utilitaires Sass
// ============================================

// --- Convertir px en rem ---
@function to-rem($px) {
  @return ($px / 16px) * 1rem;
}

// --- Convertir px en em ---
@function to-em($px, $context: 16px) {
  @return ($px / $context) * 1em;
}

// --- Obtenir une couleur de la map ---
@function color($key) {
  @if not map-has-key($colors, $key) {
    @warn "La couleur '#{$key}' n'existe pas dans la map $colors.";
    @return null;
  }
  @return map-get($colors, $key);
}

// --- Obtenir un gris ---
@function gray($level) {
  @if not map-has-key($grays, $level) {
    @warn "Le gris '#{$level}' n'existe pas dans la map $grays.";
    @return null;
  }
  @return map-get($grays, $level);
}

// --- Éclaircir une couleur ---
@function lighten-color($color, $amount: 10%) {
  @return lighten($color, $amount);
}

// --- Assombrir une couleur ---
@function darken-color($color, $amount: 10%) {
  @return darken($color, $amount);
}

// --- Calculer un pourcentage de marge/padding ---
@function spacing($multiplier) {
  @return $spacing-unit * $multiplier;
}

// --- Générer une ombre avec une couleur ---
@function colored-shadow($color, $blur: 10px, $spread: 0px, $opacity: 0.25) {
  @return 0 $spread $blur rgba($color, $opacity);
}
```

---

## 8.5 Importer les fichiers partiels

### La syntaxe d'importation

Pour utiliser le contenu d'un fichier partial, on utilise la directive `@import` (ancienne méthode) ou `@use` (méthode moderne — voir chapitre 9) :

```scss
// Fichier : main.scss

// Méthode ancienne (@import) — encore largement utilisée
@import 'abstracts/colors';
@import 'abstracts/variables';
@import 'abstracts/mixins';
@import 'abstracts/functions';

// Méthode moderne (@use) — recommandée
@use 'abstracts/colors';
@use 'abstracts/variables';
@use 'abstracts/mixins' as *;
@use 'abstracts/functions' as *;
```

### Utilisation après import

```scss
// Après avoir importé _colors.scss et _mixins.scss :

.button--primary {
  background-color: $color-primary;      // Depuis _colors.scss
  color: $color-text-inverse;            // Depuis _colors.scss
  padding: $spacing-sm $spacing-md;      // Depuis _variables.scss
  border-radius: $border-radius;         // Depuis _variables.scss
  @include transition(background-color); // Depuis _mixins.scss
  
  &:hover {
    background-color: $color-primary-dark;
  }
}
```

---

## 8.6 Les conventions de nommage

### Règles de nommage des fichiers

```scss
// ✅ BON — Noms descriptifs et clairs
_colors.scss
_typography.scss
_mixins.scss
_buttons.scss
_cards.scss
_navigation.scss

// ✅ BON — Utilisation du tiret basse (kebab-case pour les fichiers)
_global-variables.scss
_base-reset.scss

// ❌ MAUVAIS — Noms vagues ou ambigus
vars.scss          // Trop court, pas descriptif
stuff.scss         // Pas de sens
CSSHelpers.scss    // Mauvaise convention de casse
```

### Structure recommandée des dossiers

```
src/scss/
├── abstracts/          → Variables, mixins, fonctions, outils
│   ├── _colors.scss
│   ├── _variables.scss
│   ├── _mixins.scss
│   └── _functions.scss
├── base/               → Styles de base (reset, typographie)
│   ├── _reset.scss
│   ├── _typography.scss
│   └── _animations.scss
├── layout/             → Mise en page (header, footer, grid)
│   ├── _header.scss
│   ├── _footer.scss
│   ├── _sidebar.scss
│   └── _grid.scss
├── components/         → Composants réutilisables
│   ├── _buttons.scss
│   ├── _cards.scss
│   ├── _forms.scss
│   ├── _modals.scss
│   └── _navigation.scss
├── pages/              → Styles spécifiques à des pages
│   ├── _home.scss
│   ├── _about.scss
│   └── _contact.scss
├── themes/             → Thèmes et modes (dark mode, etc.)
│   ├── _light.scss
│   └── _dark.scss
├── vendors/            → Styles de bibliothèques externes
│   └── _bootstrap-overrides.scss
├── utilities/          → Classes utilitaires
│   ├── _spacing.scss
│   ├── _visibility.scss
│   └── _text.scss
└── main.scss           → Point d'entrée unique
```

### Convention de nommage des variables

```scss
// ✅ BON — Préfixe du projet ou du type
$color-primary: #3498db;
$font-size-base: 16px;
$spacing-md: 16px;
$z-index-modal: 1050;

// ✅ BON — Noms sémantiques
$color-success: #27ae60;
$color-danger: #e74c3c;
$border-color-light: $color-gray-300;

// ❌ MAUVAIS — Noms peu descriptifs
$blue: #3498db;        // Quel bleu ? Pour quoi ?
$big: 24px;            // Grand pour quoi ?
$pad: 16px;            // Pas clair
$z: 1050;              // Incompréhensible
```

### Convention de nommage des mixins

```scss
// ✅ BON — Verbe ou description de l'action
@mixin flex-center { ... }
@mixin respond-to($breakpoint) { ... }
@mixin truncate-text($lines) { ... }
@mixin button-variant($bg-color) { ... }

// ✅ BON — Préfixe pour éviter les conflits
@mixin my-flex-center { ... }
@mixin project-transition($props) { ... }

// ❌ MAUVAIS — Trop générique ou ambigu
@mixin do-something { ... }
@mixin helper { ... }
@mixin style { ... }
```

---

## 8.7 Les avantages des fichiers partiels

### 1. Organisation modulaire

```scss
// Chaque fichier a UN SEUL rôle
_buttons.scss      → Uniquement les styles de boutons
_navigation.scss   → Uniquement la navigation
_cards.scss        → Uniquement les cartes
_modals.scss       → Uniquement les modales
```

### 2. Réutilisation

```scss
// Le fichier _buttons.scss peut être utilisé dans N'IMPORTE quel projet
// Il suffit de le copier et de l'importer

@import 'components/buttons';
// Tous les styles de boutons sont maintenant disponibles
```

### 3. Maintenabilité

```scss
// Pour modifier un bouton, vous savez exactement où aller :
// → components/_buttons.scss

// Pas besoin de chercher dans 5000 lignes de CSS
```

### 4. Collaboration

```scss
// Développeur A travaille sur : _navigation.scss
// Développeur B travaille sur : _forms.scss
// Développeur C travaille sur : _cards.scss

// → Aucun conflit de fusion possible !
```

### 5. Performance de compilation

```scss
// Sass ne recompile que les fichiers modifiés
// Si vous changez _buttons.scss, seuls les fichiers dépendants
// de _buttons.scss sont recompilés
```

---

## 8.8 Exemple de fichier `main.scss` avec importations

```scss
// ============================================
// Fichier : main.scss
// Description : Point d'entrée du projet
// ============================================

// --- 1. Abstracts (variables, mixins, fonctions) ---
@import 'abstracts/colors';
@import 'abstracts/variables';
@import 'abstracts/mixins';
@import 'abstracts/functions';

// --- 2. Base (reset, typographie, animations) ---
@import 'base/reset';
@import 'base/typography';
@import 'base/animations';

// --- 3. Layout (mise en page) ---
@import 'layout/grid';
@import 'layout/header';
@import 'layout/footer';
@import 'layout/sidebar';

// --- 4. Composants ---
@import 'components/buttons';
@import 'components/cards';
@import 'components/forms';
@import 'components/modals';
@import 'components/navigation';

// --- 5. Pages (styles spécifiques) ---
@import 'pages/home';
@import 'pages/about';
@import 'pages/contact';

// --- 6. Thèmes ---
@import 'themes/light';
@import 'themes/dark';

// --- 7. Utilitaires ---
@import 'utilities/spacing';
@import 'utilities/visibility';
@import 'utilities/text';

// --- 8. Vendors (derniers pour pouvoir overrider) ---
@import 'vendors/bootstrap-overrides';
```

---

## 8.9 Erreurs courantes

### Erreur 1 : Oublier l'underscore

```scss
// Si vous essayez d'importer un fichier sans underscore :
@import 'colors';  // Cherche _colors.scss OU colors.scss

// Sass cherche d'abord _colors.scss, puis colors.scss
// Mais si vous voulez être explicite, utilisez le nom complet :
@import '_colors';  // Pas recommandé, Sass gère automatiquement
```

### Erreur 2 : Compiler un fichier partial

```bash
# ❌ Ceci ne fonctionne PAS (ou génère un fichier vide)
sass _colors.scss colors.css

# ✅ Ceci fonctionne
sass main.scss main.css
```

### Erreur 3 : Boucles d'importation

```scss
// ❌ Ne JAMAIS faire :
// _a.scss contient : @import 'b';
// _b.scss contient : @import 'a';

// → Cela crée une boucle infinie et Sass plantera !
```

### Erreur 4 : Importer dans le mauvais ordre

```scss
// ❌ Mauvais ordre (les variables ne sont pas encore définies)
@import 'components/buttons';  // Utilise $color-primary
@import 'abstracts/colors';     // Définit $color-primary

// ✅ Bon ordre (abstracts en premier)
@import 'abstracts/colors';     // Définit $color-primary
@import 'components/buttons';   // Utilise $color-primary
```

---

## 8.10 Exercices

### Exercice 1 : Créer un fichier `_typography.scss`

Créez un fichier partial `_typography.scss` contenant :

1. Les variables pour les tailles de police (h1 à h6)
2. Les variables pour les hauteurs de ligne
3. Les variables pour les poids de police
4. Les mixins responsive pour la typographie
5. Les styles de base pour les éléments de texte

```scss
// Votre fichier _typography.scss ici

// 1. Variables de tailles
$font-size-h1: ???;
$font-size-h2: ???;
// ...

// 2. Mixins
@mixin responsive-heading($size) {
  ???
}

// 3. Styles de base
h1, h2, h3, h4, h5, h6 {
  ???
}
```

### Exercice 2 : Organiser un projet

Créez la structure de dossiers suivante et les fichiers partiels correspondants :

```
src/scss/
├── abstracts/
│   ├── _colors.scss        (palette de couleurs)
│   ├── _variables.scss     (variables globales)
│   └── _mixins.scss        (3 mixins au moins)
├── base/
│   └── _reset.scss         (reset CSS minimal)
├── components/
│   └── _buttons.scss       (styles de boutons)
└── main.scss               (point d'entrée)
```

### Exercice 3 : Utiliser les partiels

En vous basant sur les fichiers créés, écrivez le code Sass qui utilise :

1. Une couleur du fichier `_colors.scss` pour un élément
2. Un spacing du fichier `_variables.scss`
3. Un mixin du fichier `_mixins.scss`
4. Créez une classe `.hero-section` en combinant tout

### Exercice 4 : Créer une map de couleurs

Créez une map `$palette` contenant au moins 5 couleurs, puis écrivez une boucle `@each` qui génère des classes utilitaires :

```scss
$palette: (
  "primary": #3498db,
  "secondary": #2ecc71,
  // ... complétez
);

// Écrivez la boucle ici
@each ??? in ??? {
  // Générez .text-{nom} et .bg-{nom}
}
```

---

## Résumé

| Concept | Description |
|---------|-------------|
| Fichier partial | Fichier Sass commençant par `_` |
| Convention `_` | Indique au compilateur de ne pas générer de CSS |
| Import | Utiliser `@import` ou `@use` pour charger un partial |
| Avantages | Modularité, réutilisation, maintenabilité, collaboration |
| Ordre d'import | Abstracts → Base → Layout → Components → Pages → Utilities |
| Compilation | Ne compiler que `main.scss` |

---

**Prochain chapitre** : [Chapitre 9 : @use](chapitre-09-use.md) — La directive moderne pour importer des modules Sass.
