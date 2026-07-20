# Chapitre 25 : Les Méta-fonctions — Module `sass:meta`

## Introduction

Le module `sass:meta` est un outil avancé de Sass qui fournit des fonctions pour inspecter et manipuler les types de données, vérifier l'existence de variables, et gérer les arguments de fonctions. Ce module est particulièrement utile pour le débogage, la création de fonctions robustes et le développement de bibliothèques Sass. Ce chapitre explore en détail chaque fonction du module `sass:meta` avec de nombreux exemples pratiques.

---

## 25.1 — Introduction au module `sass:meta`

### Importation du module

```scss
@use 'sass:meta';

$type: meta.type-of(42);
// Résultat: "number"
```

### Pourquoi utiliser `sass:meta` ?

Le module `sass:meta` est essentiel pour :

1. **Le débogage** : inspecter les types de données
2. **La validation** : vérifier les types avant d'exécuter du code
3. **La flexibilité** : créer des fonctions qui acceptent différents types
4. **La robustesse** : gérer les erreurs gracieusement

---

## 25.2 — `meta.inspect()` : Inspection de données

La fonction `meta.inspect()` retourne une représentation textuelle de n'importe quel valeur Sass.

### Syntaxe

```scss
meta.inspect($valeur)
```

### Exemples

```scss
$inspect-1: meta.inspect(42);
// Résultat: "42"

$inspect-2: meta.inspect("hello");
// Résultat: "\"hello\""

$inspect-3: meta.inspect(#3498db);
// Résultat: "#3498db"

$inspect-4: meta.inspect((1, 2, 3));
// Résultat: "(1, 2, 3)"

$inspect-5: meta.inspect(("a": 1, "b": 2));
// Résultat: "(\"a\": 1, \"b\": 2)"

$inspect-6: meta.inspect(true);
// Résultat: "true"

$inspect-7: meta.inspect(null);
// Résultat: "null"

$inspect-8: meta.inspect($variable-non-definie);
// Résultat: "null"
```

### Utilisation pour le débogage

```scss
$ma-liste: "a", "b", "c";
$ma-map: ("x": 1, "y": 2);
$ma-couleur: #e74c3c;

@debug "Liste: #{meta.inspect($ma-liste)}";
@debug "Map: #{meta.inspect($ma-map)}";
@debug "Couleur: #{meta.inspect($ma-couleur)}";
```

### Inspection de structures complexes

```scss
$structure-complexe: (
  "utilisateurs": (
    "alice": (
      "age": 25,
      "roles": ("admin", "editor")
    ),
    "bob": (
      "age": 30,
      "roles": ("viewer",)
    )
  )
);

@debug "Structure complète: #{meta.inspect($structure-complexe)}";
```

---

## 25.3 — `meta.type-of()` : Détection de type

La fonction `meta.type-of()` retourne le type d'une valeur Sass.

### Syntaxe

```scss
meta.type-of($valeur)
```

### Types disponibles

```scss
$type-number: meta.type-of(42);        // "number"
$type-string: meta.type-of("hello");   // "string"
$type-bool: meta.type-of(true);        // "bool"
$type-list: meta.type-of((1, 2, 3));   // "list"
$type-map: meta.type-of(("a": 1));     // "map"
$type-color: meta.type-of(#3498db);    // "color"
$type-null: meta.type-of(null);        // "null"
```

### Exemples de chaque type

```scss
// Nombre
$nb-1: meta.type-of(42);           // "number"
$nb-2: meta.type-of(3.14);         // "number"
$nb-3: meta.type-of(100px);        // "number"
$nb-4: meta.type-of(50%);          // "number"

// Chaîne
$str-1: meta.type-of("hello");     // "string"
$str-2: meta.type-of('world');     // "string"
$str-3: meta.type-of(bonjour);     // "string" (identifiant)

// Booléen
$bool-1: meta.type-of(true);       // "bool"
$bool-2: meta.type-of(false);      // "bool"

// Liste
$list-1: meta.type-of((1, 2, 3));  // "list"
$list-2: meta.type-of(1 2 3);      // "list"
$list-3: meta.type-of(());         // "list"

// Map
$map-1: meta.type-of(("a": 1));    // "map"
$map-2: meta.type-of(());          // "map"

// Couleur
$color-1: meta.type-of(#3498db);   // "color"
$color-2: meta.type-of(red);       // "color"
$color-3: meta.type-of(rgba(0, 0, 0, 0.5)); // "color"

// Null
$null-1: meta.type-of(null);       // "null"
```

### Utilisation pratique : validation de types

```scss
@mixin valeur-securisee($valeur, $type-requis) {
  $type-reel: meta.type-of($valeur);

  @if $type-reel != $type-requis {
    @warn "Type incorrect: attendu '#{$type-requis}', reçu '#{$type-reel}'";
  } @else {
    @content;
  }
}

@include valeur-securisee(42, "number") {
  .bloc {
    width: 42px;
  }
}

@include valeur-securisee("hello", "string") {
  .texte {
    content: "hello";
  }
}
```

---

## 25.4 — Fonctions de vérification de type

### `meta.is-number()`

Vérifie si une valeur est un nombre.

```scss
$is-num-1: meta.is-number(42);        // true
$is-num-2: meta.is-number("hello");   // false
$is-num-3: meta.is-number(100px);     // true (les nombres avec unites sont des nombres)
$is-num-4: meta.is-number(true);      // false
```

### `meta.is-string()`

Vérifie si une valeur est une chaîne.

```scss
$is-str-1: meta.is-string("hello");   // true
$is-str-2: meta.is-string('world');   // true
$is-str-3: meta.is-string(42);        // false
$is-str-4: meta.is-string(bonjour);   // true (identifiant = chaîne non quotée)
```

### `meta.is-bool()`

Vérifie si une valeur est un booléen.

```scss
$is-bool-1: meta.is-bool(true);       // true
$is-bool-2: meta.is-bool(false);      // true
$is-bool-3: meta.is-bool(1);          // false
$is-bool-4: meta.is-bool("true");     // false
```

### `meta.is-list()`

Vérifie si une valeur est une liste.

```scss
$is-list-1: meta.is-list((1, 2, 3));  // true
$is-list-2: meta.is-list(1 2 3);      // true
$is-list-3: meta.is-list("hello");    // false
$is-list-4: meta.is-list(());         // true (liste vide)
```

### `meta.is-map()`

Vérifie si une valeur est une map.

```scss
$is-map-1: meta.is-map(("a": 1));     // true
$is-map-2: meta.is-map((1, 2, 3));    // false
$is-map-3: meta.is-map(());           // true (map vide)
$is-map-4: meta.is-map("hello");      // false
```

### `meta.is-color()`

Vérifie si une valeur est une couleur.

```scss
$is-color-1: meta.is-color(#3498db);  // true
$is-color-2: meta.is-color(red);      // true
$is-color-3: meta.is-color(42);       // false
$is-color-4: meta.is-color("blue");   // false
```

### `meta.is-null()`

Vérifie si une valeur est null.

```scss
$is-null-1: meta.is-null(null);       // true
$is-null-2: meta.is-null(42);         // false
$is-null-3: meta.is-null("hello");    // false
$is-null-4: meta.is-null(false);      // false
```

### Utilisation pratique : validation de configuration

```scss
$mappage-types: (
  "number": meta.is-number,
  "string": meta.is-string,
  "bool": meta.is-bool,
  "list": meta.is-list,
  "map": meta.is-map,
  "color": meta.is-color
);

$config: (
  "padding": 16px,
  "margin": "8px 16px",
  "visible": true,
  "colors": ("primary": #3498db)
);

$types-requis: (
  "padding": "number",
  "margin": "string",
  "visible": "bool",
  "colors": "map"
);

@each $cle, $type-requis in $types-requis {
  $valeur: map-get($config, $cle);

  @if $valeur {
    $type-reel: meta.type-of($valeur);

    @if $type-reel != $type-requis {
      @warn "La clé '#{$cle}' doit être de type '#{$type-requis}', mais est de type '#{$type-reel}'";
    }
  }
}
```

---

## 25.5 — `meta.keywords()` et `meta.get-keywords()`

### `meta.keywords()`

Retourne les mots-clés passés à un mixin avec `...`.

### Syntaxe

```scss
meta.keywords($args)
```

### Exemples

```scss
@mixin ma-config($args...) {
  $cles: meta.keywords($args);

  @if map-has-key($cles, "color") {
    color: map-get($cles, "color");
  }

  @if map-has-key($cles, "size") {
    font-size: map-get($cles, "size");
  }

  @if map-has-key($cles, "weight") {
    font-weight: map-get($cles, "weight");
  }
}

.texte {
  @include ma-config(
    $color: #3498db,
    $size: 16px,
    $weight: bold
  );
}
```

### Utilisation pratique : mixin flexible

```scss
@mixin spacing($args...) {
  $cles: meta.keywords($args);

  @each $prop, $valeur in $cles {
    @if $valeur {
      #{$prop}: $valeur;
    }
  }
}

.bloc {
  @include spacing(
    $margin-top: 10px,
    $margin-bottom: 20px,
    $padding-left: 15px
  );
}
```

### `meta.get-keywords()`

Similaire à `meta.keywords()`, retourne les mots-clés d'un argument.

```scss
@mixin configurer($args...) {
  $cles: meta.get-keywords($args);

  @debug "Mots-clés reçus: #{meta.inspect($cles)}";

  @each $cle, $valeur in $cles {
    @debug "#{$cle}: #{$valeur}";
  }
}

@include configurer($a: 1, $b: 2, $c: 3);
```

---

## 25.6 — `meta.same-type()`

La fonction `meta.same-type()` vérifie si deux valeurs ont le même type.

### Syntaxe

```scss
meta.same-type($valeur1, $valeur2)
```

### Exemples

```scss
$meme-type-1: meta.same-type(42, 100);        // true
$meme-type-2: meta.same-type(42, "hello");    // false
$meme-type-3: meta.same-type("a", "b");       // true
$meme-type-4: meta.same-type(true, false);    // true
$meme-type-5: meta.same-type((1, 2), (3, 4)); // true
$meme-type-6: meta.same-type((1, 2), ("a": 1)); // false
$meme-type-7: meta.same-type(#3498db, red);   // true
$meme-type-8: meta.same-type(null, null);     // true
```

### Utilisation pratique : comparaison de types

```scss
@mixin comparer-types($a, $b) {
  $type-a: meta.type-of($a);
  $type-b: meta.type-of($b);

  @if meta.same-type($a, $b) {
    @debug "Les deux valeurs ont le même type: #{$type-a}";

    @if $a == $b {
      @debug "Les deux valeurs sont égales";
    } @else {
      @debug "Les valeurs sont différentes mais du même type";
    }
  } @else {
    @debug "Types différents: '#{$type-a}' vs '#{$type-b}'";
  }
}

@include comparer-types(42, 100);
@include comparer-types(42, "hello");
@include comparer-types(#3498db, #e74c3c);
```

---

## 25.7 — `meta.content-exists()`

La fonction `meta.content-exists()` vérifie si un mixin a reçu du contenu via `@content`.

### Syntaxe

```scss
meta.content-exists()
```

### Exemples

```scss
@mixin conteneur($args...) {
  @if meta.content-exists() {
    .contenu {
      @content;
    }
  } @else {
    .contenu {
      @debug "Aucun contenu fourni";
    }
  }
}

// Avec contenu
@include conteneur() {
  padding: 20px;
  color: blue;
}

// Sans contenu
@include conteneur();
```

### Utilisation pratique : composants flexibles

```scss
@mixin card($avec-header: true, $avec-footer: false) {
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;

  @if $avec-header {
    .card-header {
      padding: 16px;
      border-bottom: 1px solid #eee;
      background-color: #f8f9fa;

      @if meta.content-exists() {
        @content;
      } @else {
        .titre {
          font-size: 18px;
          font-weight: bold;
        }
      }
    }
  }

  .card-body {
    padding: 20px;
  }

  @if $avec-footer {
    .card-footer {
      padding: 16px;
      border-top: 1px solid #eee;
      background-color: #f8f9fa;
    }
  }
}

// Avec contenu personnalisé
@include card(true) {
  .titre-personnalise {
    color: #3498db;
    font-size: 20px;
  }
}

// Sans contenu (utilise le défaut)
@include card(true, true);
```

---

## 25.8 — Variables et portée

### `meta.variable-exists()`

Vérifie si une variable existe dans la portée actuelle.

### Syntaxe

```scss
meta.variable-exists($nom)
```

### Exemples

```scss
$ma-variable: "valeur";

$existe-1: meta.variable-exists("ma-variable");    // true
$existe-2: meta.variable-exists("autre-variable"); // false
```

### `meta.global-variable-exists()`

Vérifie si une variable globale existe.

### Syntaxe

```scss
meta.global-variable-exists($nom)
```

### Exemples

```scss
$variable-globale: 42;

$existe-1: meta.global-variable-exists("variable-globale");    // true
$existe-2: meta.global-variable-exists("variable-locale");     // false
```

### Utilisation pratique : configuration flexible

```scss
$theme-primaire: #3498db; // Définie dans un fichier de configuration

@mixin couleur-avec-fallback($nom, $fallback) {
  @if meta.global-variable-exists($nom) {
    color: #{unquote($nom)};
  } @else {
    color: $fallback;
  }
}

// Utilisation
.bouton {
  @include couleur-avec-fallback("theme-primaire", #000000);
}
```

### `meta.mixin-exists()`

Vérifie si un mixin existe.

```scss
@mixin mon-mixin() {
  color: red;
}

$existe-1: meta.mixin-exists("mon-mixin");    // true
$existe-2: meta.mixin-exists("autre-mixin"); // false
```

### `meta.function-exists()`

Vérifie si une fonction existe.

```scss
$existe-1: meta.function-exists("red");     // true (fonction native)
$existe-2: meta.function-exists("rgb");     // true
$existe-3: meta.function-exists("ma-fonc"); // false
```

---

## 25.9 — `@debug`, `@warn` et `@error`

Ces directives sont essentielles pour le débogage et la gestion des erreurs en Sass.

### `@debug`

Affiche un message de débogage dans la console.

```scss
$couleur: #3498db;

@debug "La couleur est: #{$couleur}";
@debug "Type: #{meta.type-of($couleur)}";
@debug "Longueur en caractères: #{str-length("#{$couleur}")}";

// Affiche dans la console:
// DEPRECATION WARNING: ...
// Line 1: La couleur est: #3498db
// Line 2: Type: color
// Line 3: Longueur en caractères: 7
```

### `@warn`

Affiche un avertissement (peut être désactivé).

```scss
@mixin ancienne-syntaxe() {
  @warn "Ce mixin est déprécié. Utilisez 'nouveau-mixin' à la place.";
  // Code...
}

@mixin config-depreciee($valeur) {
  @warn "Le paramètre '$valeur' est déprécié. Utilisez '$nouveau-parametre'.";

  // Code...
}
```

### `@error`

Affiche une erreur et arrête la compilation.

```scss
@mixin espace-securise($valeur) {
  @if meta.type-of($valeur) != "number" {
    @error "La valeur doit être un nombre, reçu: #{meta.type-of($valeur)}";
  }

  @if $valeur < 0 {
    @error "La valeur doit être positive, reçu: #{$valeur}";
  }

  padding: $valeur;
}

// Ceci provoquera une erreur
// @include espace-securise(-10px);

// Ceci fonctionnera
@include espace-securise(10px);
```

### Utilisation pratique : validation de configuration

```scss
@Configuration: (
  "theme": "dark",
  "langue": "fr",
  "nbre-max": 10
);

$mappage-types-requis: (
  "theme": "string",
  "langue": "string",
  "nbre-max": "number"
);

@mixin valider-config($config, $types-requis) {
  @each $cle, $type-requis in $types-requis {
    @if not map-has-key($config, $cle) {
      @error "Clé manquante dans la configuration: '#{$cle}'";
    }

    $valeur: map-get($config, $cle);
    $type-reel: meta.type-of($valeur);

    @if $type-reel != $type-requis {
      @error "Type incorrect pour '#{$cle}': attendu '#{$type-requis}', reçu '#{$type-reel}'";
    }
  }

  @debug "Configuration valide!";
}

@include valider-config($Configuration, $mappage-types-requis);
```

---

## 25.10 — Exemples pratiques avancés

### Système de typage dynamique

```scss
@use 'sass:meta';

@mixin espace($valeur, $prop: "padding") {
  $type: meta.type-of($valeur);

  @if $type == "number" {
    #{$prop}: $valeur;
  } @else if $type == "list" {
    @if length($valeur) == 1 {
      #{$prop}: nth($valeur, 1);
    } @else if length($valeur) == 2 {
      #{$prop}: nth($valeur, 1) nth($valeur, 2);
    } @else if length($valeur) == 3 {
      #{$prop}: nth($valeur, 1) nth($valeur, 2) nth($valeur, 3);
    } @else if length($valeur) == 4 {
      #{$prop}: nth($valeur, 1) nth($valeur, 2) nth($valeur, 3) nth($valeur, 4);
    }
  } @else if $type == "map" {
    @if map-has-key($valeur, "top") {
      #{$prop}-top: map-get($valeur, "top");
    }
    @if map-has-key($valeur, "right") {
      #{$prop}-right: map-get($valeur, "right");
    }
    @if map-has-key($valeur, "bottom") {
      #{$prop}-bottom: map-get($valeur, "bottom");
    }
    @if map-has-key($valeur, "left") {
      #{$prop}-left: map-get($valeur, "left");
    }
  } @else {
    @error "Type non supporté pour #{$prop}: #{$type}";
  }
}

// Utilisation
.bloc-1 {
  @include espace(16px);                              // padding: 16px
}

.bloc-2 {
  @include espace(10px 20px);                         // padding: 10px 20px
}

.bloc-3 {
  @include espace((top: 10px, right: 20px, bottom: 30px, left: 40px));
}

.bloc-4 {
  @include espace(10px, "margin");                    // margin: 10px
}
```

### Système de variantes intelligent

```scss
@use 'sass:meta';

$composants: (
  "bouton": (
    "types": ("primary", "secondary", "danger"),
    "tailles": ("sm", "md", "lg")
  ),
  "input": (
    "types": ("text", "password", "email"),
    "tailles": ("sm", "md", "lg")
  )
);

@mixin generer-composant($nom, $config) {
  $types: map-get($config, "types");
  $tailles: map-get($config, "tailles");

  @each $type in $types {
    @each $taille in $tailles {
      .#{$nom}-#{$type}-#{$taille} {
        @if $nom == "bouton" {
          @if $type == "primary" {
            background-color: #3498db;
            color: white;
          } @else if $type == "secondary" {
            background-color: #95a5a6;
            color: white;
          } @else if $type == "danger" {
            background-color: #e74c3c;
            color: white;
          }
        }

        @if $taille == "sm" {
          padding: 6px 12px;
          font-size: 12px;
        } @else if $taille == "md" {
          padding: 10px 20px;
          font-size: 14px;
        } @else if $taille == "lg" {
          padding: 14px 28px;
          font-size: 16px;
        }
      }
    }
  }
}

@each $nom, $config in $composants {
  @include generer-composant($nom, $config);
}
```

### Système de configuration robuste

```scss
@use 'sass:meta';

$theme-base: (
  "couleurs": (
    "primaire": #3498db,
    "secondaire": #2ecc71,
    "danger": #e74c3c
  ),
  "espacement": (
    "xs": 4px,
    "sm": 8px,
    "md": 16px,
    "lg": 24px,
    "xl": 32px
  ),
  "typographie": (
    "famille": "Arial, sans-serif",
    "tailles": (
      "xs": 12px,
      "sm": 14px,
      "base": 16px,
      "lg": 18px,
      "xl": 24px
    )
  )
);

@mixin valider-theme($theme) {
  @if meta.type-of($theme) != "map" {
    @error "Le thème doit être une map";
  }

  @each $cle, $valeur in $theme {
    @if meta.type-of($valeur) == "map" {
      @each $sous-cle, $sous-valeur in $valeur {
        @if meta.type-of($sous-valeur) == "map" {
          @each $sous-sous-cle, $sous-sous-valeur in $sous-valeur {
            @debug "Propriété trouvée: #{$cle}.#{$sous-cle}.#{$sous-cle} = #{$sous-sous-valeur}";
          }
        } @else {
          @debug "Propriété trouvée: #{$cle}.#{$sous-cle} = #{$sous-valeur}";
        }
      }
    } @else {
      @debug "Propriété trouvée: #{$cle} = #{$valeur}";
    }
  }
}

@include valider-theme($theme-base);
```

### Système de hooks

```scss
@use 'sass:meta';

$hooks: () !default;

@mixin enregistrer-hook($nom, $callback) {
  $hooks: map-merge($hooks, ($nom: $callback)) !global;
}

@mixin executer-hook($nom, $args...) {
  @if map-has-key($hooks, $nom) {
    $callback: map-get($hooks, $nom);
    @debug "Exécution du hook '#{$nom}'";
  } @else {
    @debug "Aucun hook enregistré pour '#{$nom}'";
  }
}

// Enregistrer un hook
@include enregistrer-hook("avant-rendu", () {
  @debug "Avant le rendu";
});

// Exécuter le hook
@include executer-hook("avant-rendu");
```

---

## 25.11 — Exercices

### Exercice 1 : Système de débogage

Créez un système de débogage qui affiche des informations détaillées sur n'importe quelle variable Sass (type, valeur, taille, etc.).

### Exercice 2 : Validation de configuration

Créez un système de validation qui vérifie automatiquement les types de toutes les valeurs dans une configuration.

### Exercice 3 : Fonction flexible

Créez une fonction qui accepte différents types de données et retourne le résultat approprié.

### Exercice 4 : Système de hooks

Créez un système de hooks qui permet d'exécuter du code à différents points du processus de compilation.

### Exercice 5 : Gestionnaire d'erreurs

Créez un gestionnaire d'erreurs qui fournit des messages clairs et des suggestions de correction.

---

## 25.12 — Résumé

| Fonction | Description | Exemple |
|----------|-------------|---------|
| `meta.inspect()` | Représentation textuelle | `meta.inspect(42)` |
| `meta.type-of()` | Type d'une valeur | `meta.type-of("hello")` |
| `meta.is-number()` | Vérifie si c'est un nombre | `meta.is-number(42)` |
| `meta.is-string()` | Vérifie si c'est une chaîne | `meta.is-string("hello")` |
| `meta.is-bool()` | Vérifie si c'est un booléen | `meta.is-bool(true)` |
| `meta.is-list()` | Vérifie si c'est une liste | `meta.is-list((1, 2))` |
| `meta.is-map()` | Vérifie si c'est une map | `meta.is-map(("a": 1))` |
| `meta.is-null()` | Vérifie si c'est null | `meta.is-null(null)` |
| `meta.is-color()` | Vérifie si c'est une couleur | `meta.is-color(#3498db)` |
| `meta.same-type()` | Compare les types | `meta.same-type(42, 100)` |
| `meta.content-exists()` | Contenu existe ? | `meta.content-exists()` |
| `meta.variable-exists()` | Variable existe ? | `meta.variable-exists("x")` |
| `meta.global-variable-exists()` | Variable globale ? | `meta.global-variable-exists("x")` |
| `meta.keywords()` | Mots-clés d'un mixin | `meta.keywords($args...)` |
| `meta.get-keywords()` | Mots-clés (variante) | `meta.get-keywords($args...)` |

### Directives de débogage

| Directive | Description | Exemple |
|-----------|-------------|---------|
| `@debug` | Message de débogage | `@debug "test"` |
| `@warn` | Avertissement | `@warn "déprécié"` |
| `@error` | Erreur fatale | `@error "erreur"` |

---

## Conclusion

Ce chapitre termine notre exploration des modules Sass. Vous maîtrisez maintenant :

1. **`sass:color`** — Manipulation des couleurs
2. **`sass:math`** — Opérations mathématiques
3. **`sass:string`** — Manipulation de chaînes
4. **`sass:list`** — Manipulation de listes
5. **`sass:map`** — Manipulation de maps
6. **`sass:selector`** — Manipulation de sélecteurs
7. **`sass:meta`** — Méta-fonctions et débogage

Ces modules vous permettent de créer des feuilles de styles Sass puissantes, maintenables et robustes. Continuez à pratiquer et à explorer les possibilités infinies de Sass !

---

**Fin du cours sur les modules Sass.**
