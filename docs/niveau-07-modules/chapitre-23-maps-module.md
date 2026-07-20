# Chapitre 23 : Les Maps (Module) — Module `sass:map`

## Introduction

Les maps (ou dictionnaires, tableaux associatifs) sont une structure de données essentielle en Sass. Elles permettent de stocker des paires clé-valeur, ce qui est parfait pour organiser des configurations, des thèmes, des tokens de design et bien plus. Le module `sass:map` fournit des fonctions puissantes pour manipuler, interroger et transformer les maps. Ce chapitre explore en détail chaque fonction du module `sass:map` avec de nombreux exemples pratiques.

---

## 23.1 — Introduction aux maps en Sass

### Création de maps

```scss
// Map simple
$couleurs: (
  "primaire": #3498db,
  "secondaire": #2ecc71,
  "danger": #e74c3c
);

// Map avec clés non-quoted
$font-sizes: (
  sm: 14px,
  base: 16px,
  lg: 18px,
  xl: 24px
);

// Map imbriquée
$theme: (
  "light": (
    "bg": #ffffff,
    "text": #333333
  ),
  "dark": (
    "bg": #1a1a2e,
    "text": #e0e0e0
  )
);

// Map vide
$vide: ();
```

### Accès aux valeurs

```scss
$couleurs: (
  "primaire": #3498db,
  "secondaire": #2ecc71
);

$primaire: map-get($couleurs, "primaire"); // #3498db
$sans-cle: map-get($couleurs, "tertiaire"); // null
```

### Vérification de l'existence d'une clé

```scss
$couleurs: (
  "primaire": #3498db
);

@a-clé: map-has-key($couleurs, "primaire");     // true
@sans-clé: map-has-key($couleurs, "secondaire"); // false
```

---

## 23.2 — `map.merge()` : Fusionner des maps

La fonction `map.merge()` fusionne deux maps. Si des clés existent dans les deux maps, les valeurs de la deuxième map écrasent celles de la première.

### Syntaxe

```scss
map.merge($map1, $map2)
```

### Exemples de base

```scss
$base: (
  "primaire": #3498db,
  "secondaire": #2ecc71
);

$personnalisation: (
  "primaire": #e74c3c,
  "tertiaire": #9b59b6
);

$fusionnee: map.merge($base, $personnalisation);
// Résultat: ("primaire": #e74c3c, "secondaire": #2ecc71, "tertiaire": #9b59b6)
```

### Fusion de thèmes

```scss
$theme-defaut: (
  "padding": 16px,
  "margin": 8px,
  "border-radius": 4px,
  "font-size": 16px
);

$theme-compact: (
  "padding": 8px,
  "margin": 4px,
  "font-size": 14px
);

$theme-final: map.merge($theme-defaut, $theme-compact);
// ("padding": 8px, "margin": 4px, "border-radius": 4px, "font-size": 14px)
```

### Fusion multiple

```scss
$config-1: ("a": 1, "b": 2);
$config-2: ("b": 3, "c": 4);
$config-3: ("c": 5, "d": 6);

$fusion: map.merge(map.merge($config-1, $config-2), $config-3);
// ("a": 1, "b": 3, "c": 5, "d": 6)
```

### Utilisation pratique : configuration de composants

```scss
$button-defaults: (
  "padding": "10px 20px",
  "border-radius": "4px",
  "font-size": "14px",
  "background-color": "#3498db",
  "color": "#ffffff"
);

$button-outlined: (
  "background-color": "transparent",
  "border": "2px solid #3498db",
  "color": "#3498db"
);

$button-solid: (
  "background-color": "#3498db",
  "color": "#ffffff"
);

.bouton {
  @each $prop, $val in $button-defaults {
    #{$prop}: #{$val};
  }

  &--outlined {
    @each $prop, $val in map.merge($button-defaults, $button-outlined) {
      #{$prop}: #{$val};
    }
  }

  &--solid {
    @each $prop, $val in map.merge($button-defaults, $button-solid) {
      #{$prop}: #{$val};
    }
  }
}
```

---

## 23.3 — `map.deep-merge()` : Fusion profonde

La fonction `map.deep-merge()` fusionne deux maps de manière récursive, en fusionnant les maps imbriquées au lieu de les écraser.

### Syntaxe

```scss
map.deep-merge($map1, $map2)
```

### Différence entre `merge` et `deep-merge`

```scss
$map1: (
  "a": (
    "x": 1,
    "y": 2
  )
);

$map2: (
  "a": (
    "y": 3,
    "z": 4
  )
);

// merge écrase la sous-map
$merge-resultat: map.merge($map1, $map2);
// ("a": ("y": 3, "z": 4)) — la sous-map "a" est remplacée

// deep-merge fusionne récursivement
$deep-resultat: map.deep-merge($map1, $map2);
// ("a": ("x": 1, "y": 3, "z": 4)) — les sous-maps sont fusionnées
```

### Utilisation pratique : thèmes multi-niveaux

```scss
$theme-base: (
  "colors": (
    "primary": #3498db,
    "secondary": #2ecc71
  ),
  "spacing": (
    "sm": 8px,
    "md": 16px
  ),
  "typography": (
    "font-size": (
      "sm": 14px,
      "base": 16px
    )
  )
);

$theme-custom: (
  "colors": (
    "primary": #e74c3c
  ),
  "spacing": (
    "lg": 32px
  ),
  "typography": (
    "font-size": (
      "lg": 20px
    )
  )
);

$theme-final: map.deep-merge($theme-base, $theme-custom);
// Fusion récursive de toutes les sous-maps
```

---

## 23.4 — `map.deep-remove()` : Suppression profonde

La fonction `map.deep-remove()` supprime récursivement une clé d'une map imbriquée.

### Syntaxe

```scss
map.deep-remove($map, $keys...)
```

### Exemples

```scss
$theme: (
  "colors": (
    "primary": #3498db,
    "secondary": #2ecc71,
    "danger": #e74c3c
  )
);

$theme-sans-danger: map.deep-remove($theme, "colors", "danger");
// ("colors": ("primary": #3498db, "secondary": #2ecc71))
```

### Suppression de plusieurs clés

```scss
$config: (
  "a": 1,
  "b": 2,
  "c": 3,
  "d": 4
);

$config-sans-b-d: map.deep-remove(map.deep-remove($config, "b"), "d");
// ("a": 1, "c": 3)
```

### Utilisation pratique

```scss
$config-complete: (
  "debug": true,
  "verbose": true,
  "production": false,
  "data": (
    "users": 100,
    "items": 500
  )
);

// En production, on supprime les options de debug
$config-prod: map.deep-remove(
  map.deep-remove($config-complete, "debug"),
  "verbose"
);
// ("production": false, "data": ("users": 100, "items": 500))
```

---

## 23.5 — `map.get()` : Récupérer une valeur

La fonction `map.get()` retourne la valeur associée à une clé dans une map.

### Syntaxe

```scss
map.get($map, $cle)
```

### Exemples

```scss
$couleurs: (
  "primaire": #3498db,
  "secondaire": #2ecc71
);

$pri: map-get($couleurs, "primaire");  // #3498db
$sec: map-get($couleurs, "secondaire"); // #2ecc71
$ter: map-get($couleurs, "tertiaire");  // null
```

### Accès à des valeurs imbriquées

```scss
$theme: (
  "colors": (
    "primary": #3498db
  )
);

$primary: map-get(map-get($theme, "colors"), "primary"); // #3498db
```

### Utilisation pratique avec des fallbacks

```scss
$couleurs: (
  "primaire": #3498db
);

@mixin couleur-avec-fallback($map, $cle, $fallback) {
  $valeur: map-get($map, $cle);

  @if $valeur {
    color: $valeur;
  } @else {
    color: $fallback;
  }
}

.texte {
  @include couleur-avec-fallback($couleurs, "primaire", #000000);
}
```

---

## 23.6 — `map.has-key()` : Vérification de clé

La fonction `map.has-key()` vérifie si une clé (ou une séquence de clés) existe dans une map.

### Syntaxe

```scss
map.has-key($map, $cles...)
```

### Exemples

```scss
$couleurs: (
  "primaire": #3498db,
  "secondaire": #2ecc71
);

$test-1: map-has-key($couleurs, "primaire");     // true
$test-2: map-has-key($couleurs, "tertiaire");    // false
$test-3: map-has-key($couleurs, "primaire", "x"); // false (pas de sous-clé)
```

### Vérification de clés imbriquées

```scss
$theme: (
  "colors": (
    "primary": #3498db,
    "secondary": #2ecc71
  )
);

$test-1: map-has-key($theme, "colors");                    // true
$test-2: map.has-key($theme, "colors", "primary");         // true
$test-3: map.has-key($theme, "colors", "tertiary");        // false
```

### Utilisation pratique : configuration conditionnelle

```scss
$config: (
  "debug": true,
  "theme": "dark"
);

@mixin config-option($map, $cle, $valeur-defaut) {
  @if map-has-key($map, $cle) {
    $valeur: map-get($map, $cle);
    @debug "Option '#{$cle}' trouvée: #{$valeur}";
  } @else {
    @debug "Option '#{$cle}' non trouvée, utilisation de la valeur par défaut: #{$valeur-defaut}";
  }
}

@include config-option($config, "debug", false);
@include config-option($config, "language", "fr");
```

---

## 23.7 — `map.keys()` : Extraire les clés

La fonction `map.keys()` retourne une liste de toutes les clés d'une map.

### Syntaxe

```scss
map.keys($map)
```

### Exemples

```scss
$couleurs: (
  "primaire": #3498db,
  "secondaire": #2ecc71,
  "danger": #e74c3c
);

$cles: map-keys($couleurs);
// Résultat: "primaire", "secondaire", "danger"
```

### Itération sur les clés

```scss
$font-sizes: (
  "sm": 14px,
  "base": 16px,
  "lg": 18px,
  "xl": 24px
);

@each $cle in map-keys($font-sizes) {
  .text-#{$cle} {
    font-size: map-get($font-sizes, $cle);
  }
}
```

### Utilisation pratique : génération de classes

```scss
$display-options: (
  "block": block,
  "flex": flex,
  "grid": grid,
  "none": none,
  "inline": inline,
  "inline-block": inline-block
);

@each $nom in map-keys($display-options) {
  .d-#{$nom} {
    display: map-get($display-options, $nom);
  }
}
```

---

## 23.8 — `map.values()` : Extraire les valeurs

La fonction `map.values()` retourne une liste de toutes les valeurs d'une map.

### Syntaxe

```scss
map.values($map)
```

### Exemples

```scss
$couleurs: (
  "primaire": #3498db,
  "secondaire": #2ecc71,
  "danger": #e74c3c
);

$valeurs: map-values($couleurs);
// Résultat: #3498db, #2ecc71, #e74c3c
```

### Utilisation pratique

```scss
$padding-options: (
  "xs": 4px,
  "sm": 8px,
  "md": 16px,
  "lg": 24px,
  "xl": 32px
);

$valeurs: map-values($padding-options);

// Créer une plage de padding
.bloc {
  padding: min($valeurs...);
}

.bloc-grand {
  padding: max($valeurs...);
}
```

---

## 23.9 — `map.remove()` : Supprimer des clés

La fonction `map.remove()` retourne une nouvelle map sans les clés spécifiées.

### Syntaxe

```scss
map.remove($map, $cles...)
```

### Exemples

```scss
$couleurs: (
  "primaire": #3498db,
  "secondaire": #2ecc71,
  "danger": #e74c3c
);

$sans-danger: map.remove($couleurs, "danger");
// ("primaire": #3498db, "secondaire": #2ecc71)

$sans-primaire-ni-danger: map.remove($couleurs, "primaire", "danger");
// ("secondaire": #2ecc71)
```

### Suppression de clés multiples

```scss
$config: (
  "a": 1,
  "b": 2,
  "c": 3,
  "d": 4,
  "e": 5
);

$sans-b-c-d: map.remove($config, "b", "c", "d");
// ("a": 1, "e": 5)
```

### Utilisation pratique : nettoyage de configuration

```scss
$config-complete: (
  "debug": true,
  "verbose": true,
  "production": false,
  "minify": true,
  "source-maps": true
);

// En production, on supprime les options de développement
$config-prod: map.remove($config-complete, "debug", "verbose", "source-maps");
// ("production": false, "minify": true)
```

---

## 23.10 — Exemples pratiques avancés

### Système de design tokens complet

```scss
@use 'sass:map';

// Tokens de base
$tokens: (
  "colors": (
    "primary": (
      "50": #eff6ff,
      "100": #dbeafe,
      "200": #bfdbfe,
      "300": #93c5fd,
      "400": #60a5fa,
      "500": #3b82f6,
      "600": #2563eb,
      "700": #1d4ed8,
      "800": #1e40af,
      "900": #1e3a8a
    ),
    "gray": (
      "50": #f9fafb,
      "100": #f3f4f6,
      "200": #e5e7eb,
      "300": #d1d5db,
      "400": #9ca3af,
      "500": #6b7280,
      "600": #4b5563,
      "700": #374151,
      "800": #1f2937,
      "900": #111827
    )
  ),
  "spacing": (
    "0": 0,
    "1": 4px,
    "2": 8px,
    "3": 12px,
    "4": 16px,
    "5": 20px,
    "6": 24px,
    "8": 32px,
    "10": 40px,
    "12": 48px
  ),
  "font-sizes": (
    "xs": 12px,
    "sm": 14px,
    "base": 16px,
    "lg": 18px,
    "xl": 20px,
    "2xl": 24px,
    "3xl": 30px,
    "4xl": 36px
  )
);

// Génération des classes utilitaires
@each $groupe-nom, $groupe in $tokens {
  @each $token-nom, $token-valeur in $groupe {
    @if type-of($token-valeur) == "map" {
      @each $sous-nom, $sous-valeur in $token-valeur {
        .#{$groupe-nom}-#{$token-nom}-#{$sous-nom} {
          @if $groupe-nom == "colors" {
            color: $sous-valeur;
          } @else {
            #{$groupe-nom}: $sous-valeur;
          }
        }
      }
    } @else {
      .#{$groupe-nom}-#{$token-nom} {
        @if $groupe-nom == "colors" {
          color: $token-valeur;
        } @else {
          #{$groupe-nom}: $token-valeur;
        }
      }
    }
  }
}
```

### Système de configuration de composants

```scss
@use 'sass:map';

// Configuration par défaut
$config-defaut: (
  "button": (
    "padding": "10px 20px",
    "border-radius": "4px",
    "font-size": "14px",
    "transition": "all 0.3s ease"
  ),
  "input": (
    "padding": "8px 12px",
    "border": "1px solid #ccc",
    "border-radius": "4px",
    "font-size": "14px"
  ),
  "card": (
    "padding": "20px",
    "border-radius": "8px",
    "shadow": "0 2px 10px rgba(0,0,0,0.1)"
  )
);

// Configuration personnalisée
$config-custom: (
  "button": (
    "padding": "12px 24px",
    "border-radius": "8px"
  ),
  "card": (
    "shadow": "0 4px 20px rgba(0,0,0,0.15)"
  )
);

$config-final: map.deep-merge($config-defaut, $config-custom);

// Génération des composants
@each $composant, $props in $config-final {
  .#{$composant} {
    @each $prop, $val in $props {
      #{$prop}: #{$val};
    }
  }
}
```

### Système de breakpoints responsive

```scss
@use 'sass:map';

$breakpoints: (
  "sm": 576px,
  "md": 768px,
  "lg": 992px,
  "xl": 1200px,
  "2xl": 1400px
);

// Ordre des breakpoints (pour les media queries)
$breakpoint-order: map-keys($breakpoints);

@mixin respond-above($bp) {
  $index: list.index($breakpoint-order, $bp);

  @if $index {
    @for $i from $index through list.length($breakpoint-order) {
      $current-bp: nth($breakpoint-order, $i);
      $min-width: map-get($breakpoints, $current-bp);

      @media (min-width: $min-width) {
        @content($current-bp);
      }
    }
  }
}

@mixin respond-below($bp) {
  $index: list.index($breakpoint-order, $bp);

  @if $index and $index > 1 {
    $prev-index: $index - 1;
    $prev-bp: nth($breakpoint-order, $prev-index);
    $max-width: map-get($breakpoints, $prev-bp);

    @media (max-width: $max-width - 1px) {
      @content($prev-bp);
    }
  }
}

// Utilisation
.container {
  @include respond-above("md") {
    padding: map-get($breakpoints, $current-bp) * 0.02;
  }
}
```

### Système de variantes

```scss
@use 'sass:map';

$variantes: (
  "colors": (
    "primary": (
      "bg": #3498db,
      "text": #ffffff,
      "border": #2980b9,
      "hover": #2980b9
    ),
    "success": (
      "bg": #2ecc71,
      "text": #ffffff,
      "border": #27ae60,
      "hover": #27ae60
    ),
    "danger": (
      "bg": #e74c3c,
      "text": #ffffff,
      "border": #c0392b,
      "hover": #c0392b
    )
  ),
  "sizes": (
    "sm": (
      "padding": "4px 8px",
      "font-size": "12px",
      "border-radius": "2px"
    ),
    "md": (
      "padding": "8px 16px",
      "font-size": "14px",
      "border-radius": "4px"
    ),
    "lg": (
      "padding": "12px 24px",
      "font-size": "16px",
      "border-radius": "6px"
    )
  )
);

// Génération des boutons
@each $type, $valeurs in map-get($variantes, "colors") {
  @each $taille, $props in map-get($variantes, "sizes") {
    .btn-#{$type}-#{$taille} {
      background-color: map-get($valeurs, "bg");
      color: map-get($valeurs, "text");
      border: 2px solid map-get($valeurs, "border");
      padding: map-get($props, "padding");
      font-size: map-get($props, "font-size");
      border-radius: map-get($props, "border-radius");
      cursor: pointer;
      transition: all 0.3s ease;

      &:hover {
        background-color: map-get($valeurs, "hover");
      }
    }
  }
}
```

---

## 23.11 — Exercices

### Exercice 1 : Système de thème

Créez un système de thème complet avec les modes clair et sombre, en utilisant `map.deep-merge()` pour personnaliser les thèmes.

### Exercice 2 : Configuration de grille

Créez un système de configuration de grille qui prend en charge différents breakpoints et nombres de colonnes.

### Exercice 3 : Design tokens

Créez un système de design tokens qui génère automatiquement les classes utilitaires pour les couleurs, espacements et tailles de police.

### Exercice 4 : Variante de composants

Créez un système de variantes pour les composants (boutons, inputs, cards) avec différents types et tailles.

### Exercice 5 : Gestionnaire de configuration

Créez un gestionnaire de configuration qui permet de fusionner, supprimer et valider les options.

---

## 23.12 — Résumé

| Fonction | Description | Exemple |
|----------|-------------|---------|
| `map.get()` | Récupérer une valeur | `map.get($m, "cle")` |
| `map.has-key()` | Vérifier l'existence d'une clé | `map.has-key($m, "cle")` |
| `map.keys()` | Extraire les clés | `map.keys($m)` |
| `map.values()` | Extraire les valeurs | `map.values($m)` |
| `map.merge()` | Fusionner deux maps | `map.merge($m1, $m2)` |
| `map.deep-merge()` | Fusion profonde | `map.deep-merge($m1, $m2)` |
| `map.remove()` | Supprimer des clés | `map.remove($m, "cle")` |
| `map.deep-remove()` | Suppression profonde | `map.deep-remove($m, "cle1", "cle2")` |

---

**Prochain chapitre :** Nous explorerons le module `sass:selector` pour la manipulation des sélecteurs CSS.
