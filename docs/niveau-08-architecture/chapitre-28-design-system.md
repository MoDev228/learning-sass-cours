# Chapitre 28 — Design System avec SCSS

## Introduction

Un **Design System** est une collection de tokens, composants, patterns et guidelines qui garantissent la cohérence visuelle d'un produit. SCSS est l'outil idéal pour construire un design token system : variables, maps, mixins et fonctions permettent de créer un langage visuel unique et maintenable.

Ce chapitre détaille la création d'un système complet de design tokens.

---

## Qu'est-ce qu'un Design Token ?

Un **design token** est la plus petite unité d'un design : une valeur nommée qui représente une décision visuelle. Au lieu d'écrire `#3498db` partout, on écrit `$color-primary` ou `var(--color-primary)`.

```
Token → Valeur CSS → Rendu visuel
$color-primary → #3498db → un bouton bleu
```

### Types de tokens

| Type | Exemple |
|---|---|
| Couleur | `$color-primary: #3498db` |
| Typographie | `$font-size-base: 16px` |
| Espacement | `$spacing-md: 16px` |
| Ombre | `$shadow-md: 0 4px 6px rgba(0,0,0,0.1)` |
| Bordure | `$border-radius-md: 8px` |
| Transition | `$transition-base: 300ms ease` |
| Z-index | `$z-modal: 1050` |

---

## 1. Palette de couleurs

### Couleurs de base

```scss
// tokens/_colors.scss

// === PRIMAIRES ===
$color-blue-50: #eff6ff;
$color-blue-100: #dbeafe;
$color-blue-200: #bfdbfe;
$color-blue-300: #93c5fd;
$color-blue-400: #60a5fa;
$color-blue-500: #3b82f6;   // Primary
$color-blue-600: #2563eb;
$color-blue-700: #1d4ed8;
$color-blue-800: #1e40af;
$color-blue-900: #1e3a8a;

$color-primary: $color-blue-500;
$color-primary-light: $color-blue-300;
$color-primary-dark: $color-blue-700;

// === SECONDAIRES ===
$color-green-50: #f0fdf4;
$color-green-100: #dcfce7;
$color-green-200: #bbf7d0;
$color-green-300: #86efac;
$color-green-400: #4ade80;
$color-green-500: #22c55e;   // Secondary
$color-green-600: #16a34a;
$color-green-700: #15803d;
$color-green-800: #166534;
$color-green-900: #14532d;

$color-secondary: $color-green-500;
$color-secondary-light: $color-green-300;
$color-secondary-dark: $color-green-700;

// === DANGER / ERREUR ===
$color-red-50: #fef2f2;
$color-red-100: #fee2e2;
$color-red-200: #fecaca;
$color-red-300: #fca5a5;
$color-red-400: #f87171;
$color-red-500: #ef4444;     // Danger
$color-red-600: #dc2626;
$color-red-700: #b91c1c;
$color-red-800: #991b1b;
$color-red-900: #7f1d1d;

$color-danger: $color-red-500;
$color-danger-light: $color-red-300;
$color-danger-dark: $color-red-700;

// === AVERTISSEMENT ===
$color-amber-50: #fffbeb;
$color-amber-100: #fef3c7;
$color-amber-200: #fde68a;
$color-amber-300: #fcd34d;
$color-amber-400: #fbbf24;
$color-amber-500: #f59e0b;   // Warning
$color-amber-600: #d97706;
$color-amber-700: #b45309;
$color-amber-800: #92400e;
$color-amber-900: #78350f;

$color-warning: $color-amber-500;
$color-warning-light: $color-amber-300;
$color-warning-dark: $color-amber-700;

// === NEUTRES ===
$color-gray-50: #f9fafb;
$color-gray-100: #f3f4f6;
$color-gray-200: #e5e7eb;
$color-gray-300: #d1d5db;
$color-gray-400: #9ca3af;
$color-gray-500: #6b7280;
$color-gray-600: #4b5563;
$color-gray-700: #374151;
$color-gray-800: #1f2937;
$color-gray-900: #111827;

// === SEMANTIQUES ===
$color-text: $color-gray-900;
$color-text-secondary: $color-gray-600;
$color-text-muted: $color-gray-400;
$color-text-inverse: #ffffff;

$color-bg: #ffffff;
$color-bg-secondary: $color-gray-50;
$color-bg-tertiary: $color-gray-100;

$color-border: $color-gray-200;
$color-border-hover: $color-gray-300;
$color-border-focus: $color-primary;
```

### Couleurs en tant que map

```scss
// tokens/_color-map.scss

$colors: (
  // Primaires
  'blue': (
    '50': $color-blue-50,
    '100': $color-blue-100,
    '200': $color-blue-200,
    '300': $color-blue-300,
    '400': $color-blue-400,
    '500': $color-blue-500,
    '600': $color-blue-600,
    '700': $color-blue-700,
    '800': $color-blue-800,
    '900': $color-blue-900,
  ),

  'green': (
    '50': $color-green-50,
    '100': $color-green-100,
    '200': $color-green-200,
    '300': $color-green-300,
    '400': $color-green-400,
    '500': $color-green-500,
    '600': $color-green-600,
    '700': $color-green-700,
    '800': $color-green-800,
    '900': $color-green-900,
  ),

  'red': (
    '50': $color-red-50,
    '100': $color-red-100,
    '200': $color-red-200,
    '300': $color-red-300,
    '400': $color-red-400,
    '500': $color-red-500,
    '600': $color-red-600,
    '700': $color-red-700,
    '800': $color-red-800,
    '900': $color-red-900,
  ),

  'amber': (
    '50': $color-amber-50,
    '100': $color-amber-100,
    '200': $color-amber-200,
    '300': $color-amber-300,
    '400': $color-amber-400,
    '500': $color-amber-500,
    '600': $color-amber-600,
    '700': $color-amber-700,
    '800': $color-amber-800,
    '900': $color-amber-900,
  ),

  'gray': (
    '50': $color-gray-50,
    '100': $color-gray-100,
    '200': $color-gray-200,
    '300': $color-gray-300,
    '400': $color-gray-400,
    '500': $color-gray-500,
    '600': $color-gray-600,
    '700': $color-gray-700,
    '800': $color-gray-800,
    '900': $color-gray-900,
  ),
);

// Fonction pour récupérer une couleur
@function color($family, $shade) {
  @return map-deep-get($colors, $family, $shade);
}
```

---

## 2. Échelle d'espacement (Spacing Scale)

Un système d'espacement basé sur une unité de base (4px ou 8px) crée de la cohérence visuelle.

```scss
// tokens/_spacing.scss

// Système basé sur 4px
$spacing-unit: 4px;

$spacing-0: 0;
$spacing-px: 1px;
$spacing-0-5: $spacing-unit * 0.5;   // 2px
$spacing-1: $spacing-unit * 1;       // 4px
$spacing-1-5: $spacing-unit * 1.5;   // 6px
$spacing-2: $spacing-unit * 2;       // 8px
$spacing-2-5: $spacing-unit * 2.5;   // 10px
$spacing-3: $spacing-unit * 3;       // 12px
$spacing-4: $spacing-unit * 4;       // 16px
$spacing-5: $spacing-unit * 5;       // 20px
$spacing-6: $spacing-unit * 6;       // 24px
$spacing-7: $spacing-unit * 7;       // 28px
$spacing-8: $spacing-unit * 8;       // 32px
$spacing-9: $spacing-unit * 9;       // 36px
$spacing-10: $spacing-unit * 10;     // 40px
$spacing-12: $spacing-unit * 12;     // 48px
$spacing-14: $spacing-unit * 14;     // 56px
$spacing-16: $spacing-unit * 16;     // 64px
$spacing-20: $spacing-unit * 20;     // 80px
$spacing-24: $spacing-unit * 24;     // 96px
$spacing-28: $spacing-unit * 28;     // 112px
$spacing-32: $spacing-unit * 32;     // 128px

// Map des espacements
$spacing-map: (
  0: $spacing-0,
  px: $spacing-px,
  0.5: $spacing-0-5,
  1: $spacing-1,
  1.5: $spacing-1-5,
  2: $spacing-2,
  2.5: $spacing-2-5,
  3: $spacing-3,
  4: $spacing-4,
  5: $spacing-5,
  6: $spacing-6,
  7: $spacing-7,
  8: $spacing-8,
  9: $spacing-9,
  10: $spacing-10,
  12: $spacing-12,
  14: $spacing-14,
  16: $spacing-16,
  20: $spacing-20,
  24: $spacing-24,
  28: $spacing-28,
  32: $spacing-32,
);

// Fonction utilitaire
@function space($size) {
  @return map-get($spacing-map, $size);
}

// Map pour les gaps
$gap-map: (
  none: 0,
  xs: $spacing-1,
  sm: $spacing-2,
  md: $spacing-4,
  lg: $spacing-6,
  xl: $spacing-8,
  2xl: $spacing-12,
);
```

### Génération automatique de classes utilitaires

```scss
// utilities/_spacing-generated.scss

@each $size-name, $size-value in $spacing-map {
  .m-#{$size-name} { margin: $size-value !important; }
  .mt-#{$size-name} { margin-top: $size-value !important; }
  .mr-#{$size-name} { margin-right: $size-value !important; }
  .mb-#{$size-name} { margin-bottom: $size-value !important; }
  .ml-#{$size-name} { margin-left: $size-value !important; }
  .mx-#{$size-name} {
    margin-left: $size-value !important;
    margin-right: $size-value !important;
  }
  .my-#{$size-name} {
    margin-top: $size-value !important;
    margin-bottom: $size-value !important;
  }

  .p-#{$size-name} { padding: $size-value !important; }
  .pt-#{$size-name} { padding-top: $size-value !important; }
  .pr-#{$size-name} { padding-right: $size-value !important; }
  .pb-#{$size-name} { padding-bottom: $size-value !important; }
  .pl-#{$size-name} { padding-left: $size-value !important; }
  .px-#{$size-name} {
    padding-left: $size-value !important;
    padding-right: $size-value !important;
  }
  .py-#{$size-name} {
    padding-top: $size-value !important;
    padding-bottom: $size-value !important;
  }
}

.mx-auto { margin-left: auto !important; margin-right: auto !important; }
.ml-auto { margin-left: auto !important; }
.mr-auto { margin-right: auto !important; }
```

---

## 3. Échelle typographique

### Système typographique complet

```scss
// tokens/_typography.scss

// Polices
$font-sans: (
  'Inter',
  -apple-system,
  BlinkMacSystemFont,
  'Segoe UI',
  Roboto,
  sans-serif,
);

$font-serif: (
  'Merriweather',
  Georgia,
  'Times New Roman',
  serif,
);

$font-mono: (
  'JetBrains Mono',
  'Fira Code',
  'Courier New',
  monospace,
);

// Tailles de police — échelle modulaire (ratio 1.25)
$font-size-base: 16px;

$font-size-xs: calc(#{$font-size-base} * 0.75);     // 12px
$font-size-sm: calc(#{$font-size-base} * 0.875);     // 14px
$font-size-md: $font-size-base;                       // 16px
$font-size-lg: calc(#{$font-size-base} * 1.125);     // 18px
$font-size-xl: calc(#{$font-size-base} * 1.25);      // 20px
$font-size-2xl: calc(#{$font-size-base} * 1.5);      // 24px
$font-size-3xl: calc(#{$font-size-base} * 1.875);    // 30px
$font-size-4xl: calc(#{$font-size-base} * 2.25);     // 36px
$font-size-5xl: calc(#{$font-size-base} * 3);        // 48px
$font-size-6xl: calc(#{$font-size-base} * 3.75);     // 60px

// Map des tailles
$font-size-map: (
  xs: $font-size-xs,
  sm: $font-size-sm,
  base: $font-size-md,
  lg: $font-size-lg,
  xl: $font-size-xl,
  '2xl': $font-size-2xl,
  '3xl': $font-size-3xl,
  '4xl': $font-size-4xl,
  '5xl': $font-size-5xl,
  '6xl': $font-size-6xl,
);

// Poids de police
$font-weight-thin: 100;
$font-weight-extralight: 200;
$font-weight-light: 300;
$font-weight-normal: 400;
$font-weight-medium: 500;
$font-weight-semibold: 600;
$font-weight-bold: 700;
$font-weight-extrabold: 800;
$font-weight-black: 900;

$font-weight-map: (
  thin: $font-weight-thin,
  extralight: $font-weight-extralight,
  light: $font-weight-light,
  normal: $font-weight-normal,
  medium: $font-weight-medium,
  semibold: $font-weight-semibold,
  bold: $font-weight-bold,
  extrabold: $font-weight-extrabold,
  black: $font-weight-black,
);

// Interlignes
$line-height-none: 1;
$line-height-tight: 1.25;
$line-height-snug: 1.375;
$line-height-normal: 1.5;
$line-height-relaxed: 1.625;
$line-height-loose: 2;

$line-height-map: (
  none: $line-height-none,
  tight: $line-height-tight,
  snug: $line-height-snug,
  normal: $line-height-normal,
  relaxed: $line-height-relaxed,
  loose: $line-height-loose,
);

// Interlettres
$tracking-tighter: -0.05em;
$tracking-tight: -0.025em;
$tracking-normal: 0em;
$tracking-wide: 0.025em;
$tracking-wider: 0.05em;
$tracking-widest: 0.1em;

$tracking-map: (
  tighter: $tracking-tighter,
  tight: $tracking-tight,
  normal: $tracking-normal,
  wide: $tracking-wide,
  wider: $tracking-wider,
  widest: $tracking-widest,
);
```

### Fonctions typographiques

```scss
// tokens/_typography-functions.scss

@function font-size($size) {
  @return map-get($font-size-map, $size);
}

@function weight($weight) {
  @return map-get($font-weight-map, $weight);
}

@function leading($line-height) {
  @return map-get($line-height-map, $line-height);
}

@function tracking($tracking) {
  @return map-get($tracking-map, $tracking);
}
```

### Classes utilitaires typographiques générées

```scss
// utilities/_typography-generated.scss

// Tailles
@each $name, $value in $font-size-map {
  .text-#{$name} {
    font-size: $value;
  }
}

// Poids
@each $name, $value in $font-weight-map {
  .font-#{$name} {
    font-weight: $value;
  }
}

// Interlignes
@each $name, $value in $line-height-map {
  .leading-#{$name} {
    line-height: $value;
  }
}

// Interlettres
@each $name, $value in $tracking-map {
  .tracking-#{$name} {
    letter-spacing: $value;
  }
}

// Familles
.font-sans { font-family: $font-sans; }
.font-serif { font-family: $font-serif; }
.font-mono { font-family: $font-mono; }
```

---

## 4. Border Radius

```scss
// tokens/_border-radius.scss

$border-radius-none: 0;
$border-radius-sm: 2px;
$border-radius-md: 6px;
$border-radius-lg: 8px;
$border-radius-xl: 12px;
$border-radius-2xl: 16px;
$border-radius-3xl: 24px;
$border-radius-full: 9999px;

$border-radius-map: (
  none: $border-radius-none,
  sm: $border-radius-sm,
  md: $border-radius-md,
  lg: $border-radius-lg,
  xl: $border-radius-xl,
  '2xl': $border-radius-2xl,
  '3xl': $border-radius-3xl,
  full: $border-radius-full,
);

// Classes générées
@each $name, $value in $border-radius-map {
  .rounded-#{$name} {
    border-radius: $value !important;
  }
}

// Bordures
$border-width-thin: 1px;
$border-width-base: 2px;
$border-width-thick: 4px;

$border-color: $color-gray-200;
$border-color-light: $color-gray-100;
$border-color-dark: $color-gray-400;

$border-map: (
  thin: $border-width-thin,
  base: $border-width-base,
  thick: $border-width-thick,
);
```

---

## 5. Ombres (Shadows)

```scss
// tokens/_shadows.scss

// Échelle d'ombres
$shadow-none: none;

$shadow-xs: (
  0 1px 2px 0 rgba(0, 0, 0, 0.05),
);

$shadow-sm: (
  0 1px 3px 0 rgba(0, 0, 0, 0.1),
  0 1px 2px -1px rgba(0, 0, 0, 0.1),
);

$shadow-md: (
  0 4px 6px -1px rgba(0, 0, 0, 0.1),
  0 2px 4px -2px rgba(0, 0, 0, 0.1),
);

$shadow-lg: (
  0 10px 15px -3px rgba(0, 0, 0, 0.1),
  0 4px 6px -4px rgba(0, 0, 0, 0.1),
);

$shadow-xl: (
  0 20px 25px -5px rgba(0, 0, 0, 0.1),
  0 8px 10px -6px rgba(0, 0, 0, 0.1),
);

$shadow-2xl: (
  0 25px 50px -12px rgba(0, 0, 0, 0.25),
);

$shadow-inner: inset 0 2px 4px 0 rgba(0, 0, 0, 0.05);

// Ombres colorées
$shadow-primary: 0 4px 14px 0 rgba($color-primary, 0.39);
$shadow-danger: 0 4px 14px 0 rgba($color-danger, 0.39);
$shadow-success: 0 4px 14px 0 rgba($color-secondary, 0.39);

// Map
$shadow-map: (
  none: $shadow-none,
  xs: $shadow-xs,
  sm: $shadow-sm,
  md: $shadow-md,
  lg: $shadow-lg,
  xl: $shadow-xl,
  '2xl': $shadow-2xl,
  inner: $shadow-inner,
  primary: $shadow-primary,
  danger: $shadow-danger,
  success: $shadow-success,
);

// Fonction
@function shadow($name) {
  @return map-get($shadow-map, $name);
}

// Classes générées
@each $name, $value in $shadow-map {
  .shadow-#{$name} {
    box-shadow: $value !important;
  }
}
```

---

## 6. Transitions

```scss
// tokens/_transitions.scss

// Durées
$duration-instant: 0ms;
$duration-fast: 100ms;
$duration-normal: 200ms;
$duration-base: 300ms;
$duration-slow: 500ms;
$duration-slower: 700ms;

$duration-map: (
  instant: $duration-instant,
  fast: $duration-fast,
  normal: $duration-normal,
  base: $duration-base,
  slow: $duration-slow,
  slower: $duration-slower,
);

// Courbes d'accélération
$ease-linear: linear;
$ease-in: cubic-bezier(0.4, 0, 1, 1);
$ease-out: cubic-bezier(0, 0, 0.2, 1);
$ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
$ease-bounce: cubic-bezier(0.68, -0.55, 0.27, 1.55);
$ease-elastic: cubic-bezier(0.175, 0.885, 0.32, 1.275);

$ease-map: (
  linear: $ease-linear,
  in: $ease-in,
  out: $ease-out,
  'in-out': $ease-in-out,
  bounce: $ease-bounce,
  elastic: $ease-elastic,
);

// Properties
$transition-colors: color, background-color, border-color, box-shadow;
$transition-transform: transform;
$transition-opacity: opacity;
$transition-all: all;

// Mixin de transition
@mixin transition($properties: all, $duration: $duration-base, $easing: $ease-in-out) {
  transition-property: $properties;
  transition-duration: $duration;
  transition-timing-function: $easing;
}

// Mixin de transition personnalisée
@mixin transition-map($map) {
  $result: '';

  @each $property, $value in $map {
    @if $result != '' {
      $result: $result + ',';
    }

    @if map-has-key($duration-map, map-get($value, duration)) {
      $value: map-merge($value, (duration: map-get($duration-map, map-get($value, duration))));
    }

    @if map-has-key($ease-map, map-get($value, ease)) {
      $value: map-merge($value, (ease: map-get($ease-map, map-get($value, ease))));
    }

    $result: #{$result} #{$property} #{map-get($value, duration) $duration-base} #{map-get($value, ease) $ease-in-out};
  }

  transition: #{$result};
}
```

### Utilisation pratique

```scss
// Composant utilisant les tokens de transition
.card {
  @include transition($transition-colors, $duration-base, $ease-in-out);

  &:hover {
    box-shadow: shadow(lg);
  }
}

.btn {
  @include transition(transform, $duration-fast, $ease-out);

  &:active {
    transform: scale(0.97);
  }
}

.modal {
  @include transition(opacity, $duration-normal, $ease-in-out);
}
```

---

## Système de thèmes (Dark/Light)

```scss
// tokens/_themes.scss

:root {
  // Couleurs
  --color-primary: #{$color-primary};
  --color-primary-light: #{$color-primary-light};
  --color-primary-dark: #{$color-primary-dark};
  --color-secondary: #{$color-secondary};
  --color-danger: #{$color-danger};
  --color-warning: #{$color-warning};

  --color-text: #{$color-text};
  --color-text-secondary: #{$color-text-secondary};
  --color-text-muted: #{$color-text-muted};
  --color-text-inverse: #{$color-text-inverse};

  --color-bg: #{$color-bg};
  --color-bg-secondary: #{$color-bg-secondary};
  --color-bg-tertiary: #{$color-bg-tertiary};

  --color-border: #{$color-border};
  --color-border-hover: #{$color-border-hover};
  --color-border-focus: #{$color-border-focus};

  // Ombres
  --shadow-sm: #{$shadow-sm};
  --shadow-md: #{$shadow-md};
  --shadow-lg: #{$shadow-lg};
  --shadow-xl: #{$shadow-xl};

  // Transitions
  --transition-fast: #{$duration-fast}ms #{$ease-in-out};
  --transition-base: #{$duration-base}ms #{$ease-in-out};
  --transition-slow: #{$duration-slow}ms #{$ease-in-out};
}

[data-theme="dark"],
.theme-dark {
  --color-primary: #{lighten($color-primary, 10%)};
  --color-primary-light: #{lighten($color-primary, 25%)};
  --color-primary-dark: #{$color-primary};
  --color-secondary: #{lighten($color-secondary, 10%)};
  --color-danger: #{lighten($color-danger, 10%)};
  --color-warning: #{lighten($color-warning, 10%)};

  --color-text: #e2e8f0;
  --color-text-secondary: #a0aec0;
  --color-text-muted: #718096;
  --color-text-inverse: #1a202c;

  --color-bg: #1a202c;
  --color-bg-secondary: #2d3748;
  --color-bg-tertiary: #4a5568;

  --color-border: #4a5568;
  --color-border-hover: #718096;
  --color-border-focus: var(--color-primary);

  --shadow-sm: 0 1px 3px 0 rgba(0, 0, 0, 0.3);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.3);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.3);
  --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.4);
}

// Thème entreprise personnalisé
.theme-enterprise {
  --color-primary: #003366;
  --color-primary-light: #004488;
  --color-primary-dark: #002244;
  --color-secondary: #0066cc;
  --color-danger: #cc0000;
  --font-sans: 'Roboto', sans-serif;
}
```

---

## Arbre du Design System

```
design-system/
├── tokens/
│   ├── _colors.scss
│   ├── _color-map.scss
│   ├── _spacing.scss
│   ├── _typography.scss
│   ├── _typography-functions.scss
│   ├── _border-radius.scss
│   ├── _shadows.scss
│   ├── _transitions.scss
│   └── _themes.scss
│
├── mixins/
│   ├── _responsive.scss
│   ├── _layout.scss
│   ├── _typography.scss
│   └── _animation.scss
│
├── utilities/
│   ├── _spacing-generated.scss
│   ├── _typography-generated.scss
│   ├── _display-generated.scss
│   ├── _shadow-generated.scss
│   └── _radius-generated.scss
│
└── main.scss
```

---

## Intégration CSS Custom Properties

Pour un design system moderne, on peut combiner SCSS pour la compilation et CSS Custom Properties pour le runtime :

```scss
// tokens/_runtime.scss

// Génère des custom properties à partir des tokens SCSS
@mixin generate-custom-properties($namespace) {
  @each $key, $value in $spacing-map {
    --#{$namespace}-space-#{$key}: #{$value};
  }

  @each $key, $value in $font-size-map {
    --#{$namespace}-font-size-#{$key}: #{$value};
  }

  @each $key, $value in $shadow-map {
    --#{$namespace}-shadow-#{$key}: #{$value};
  }

  @each $key, $value in $border-radius-map {
    --#{$namespace}-radius-#{$key}: #{$value};
  }
}

// Utilisation
:root {
  @include generate-custom-properties('ds');
}

// Résultat en CSS :
// :root {
//   --ds-space-0: 0;
//   --ds-space-1: 4px;
//   --ds-space-2: 8px;
//   --ds-space-4: 16px;
//   --ds-font-size-sm: 14px;
//   --ds-font-size-base: 16px;
//   --ds-shadow-md: 0 4px 6px -1px rgba(0,0,0,0.1);
//   --ds-radius-md: 6px;
//   ...
// }
```

---

## Résumé

Un design token system en SCSS crée une source unique de vérité pour toutes les valeurs visuelles d'un projet. En combinant des variables, des maps, des fonctions et des mixins, on obtient un système cohérent, maintenable et évolutif. Les classes générées automatiquement assurent une utilisation rapide en HTML tout en garantissant la cohérence visuelle.
