# Chapitre 24 : Les Sélecteurs — Module `sass:selector`

## Introduction

Le module `sass:selector` est un outil puissant pour manipuler et construire des sélecteurs CSS de manière programmatique. Il permet de créer des sélecteurs dynamiques, de les combiner, de les analyser et de les transformer. Ce module est particulièrement utile pour créer des systèmes de design flexibles, des mixins réutilisables et des composants modulaires. Ce chapitre explore en détail chaque fonction du module `sass:selector` avec de nombreux exemples pratiques.

---

## 24.1 — Introduction aux sélecteurs en Sass

### Sélecteurs simples

```scss
// Sélecteur de classe
.mon-classe {
  color: blue;
}

// Sélecteur d'identifiant
#mon-id {
  color: red;
}

// Sélecteur d'élément
div {
  color: green;
}

// Sélecteur universel
* {
  box-sizing: border-box;
}
```

### Sélecteurs composés

```scss
// Combinaison de sélecteurs
.mon-classe .sous-classe {
  color: blue;
}

// Combinateur descendant
.parent .enfant {
  margin-left: 20px;
}

// Combinateur enfant direct
.parent > .enfant {
  margin-left: 20px;
}

// Combinateur frère adjacent
.frere + .suivant {
  margin-top: 10px;
}

// Combinateur frère général
.frere ~ .suivant {
  margin-top: 10px;
}
```

### Interpolation de sélecteurs

```scss
$nom: "ma-classe";

.#{$nom} {
  color: blue;
}
// Génère: .ma-classe { color: blue; }

$element: "div";
$classe: "active";

#{$element}.#{$classe} {
  color: red;
}
// Génère: div.active { color: red; }
```

---

## 24.2 — `selector.parse()` : Analyser un sélecteur

La fonction `selector.parse()` convertit une chaîne de caractères en un objet sélecteur Sass.

### Syntaxe

```scss
selector.parse($chaine)
```

### Exemples

```scss
$sel-1: selector.parse(".mon-classe");
// Résultat: .mon-classe

$sel-2: selector.parse("div > .enfant");
// Résultat: div > .enfant

$sel-3: selector.parse(".a, .b, .c");
// Résultat: .a, .b, .c

$sel-4: selector.parse("#id .classe .sous-classe");
// Résultat: #id .classe .sous-classe
```

### Utilisation pratique

```scss
$mappage-selecteurs: (
  "btn": ".btn",
  "input": ".input",
  "card": ".card"
);

@each $nom, $sel in $mappage-selecteurs {
  $selecteur: selector.parse($sel);

  #{$selecteur} {
    border: 1px solid #ddd;
    border-radius: 4px;
  }
}
```

---

## 24.3 — `selector.append()` : Ajouter des sélecteurs

La fonction `selector.append()` combine plusieurs sélecteurs en les enchaînant.

### Syntaxe

```scss
selector.append($selecteur, $suffixes...)
```

### Exemples de base

```scss
$simple: selector.append(".btn", "--primary");
// Résultat: .btn--primary

$compose: selector.append(".card", "__header", "--large");
// Résultat: .card__header--large

$enfant: selector.append(".parent", "> .child");
// Résultat: .parent > .child
```

### Construction de sélecteurs BEM

```scss
$block: ".card";
$element: "__header";
$modifier: "--primary";

$sel-block: selector.parse($block);
$sel-element: selector.append($sel-block, $element);
$sel-modifier: selector.append($sel-element, $modifier);

.card {
  border: 1px solid #ddd;

  #{selector.append($sel-block, $element)} {
    padding: 16px;
    border-bottom: 1px solid #eee;
  }

  #{selector.append($sel-block, $element, $modifier)} {
    background-color: #3498db;
    color: white;
  }
}
```

### Sélecteurs avec états

```scss
$base: ".bouton";

.bouton {
  background-color: #3498db;

  // États
  #{selector.append($base, ":hover")} {
    background-color: darken(#3498db, 10%);
  }

  #{selector.append($base, ":focus")} {
    outline: 3px solid rgba(#3498db, 0.5);
  }

  #{selector.append($base, ":active")} {
    background-color: darken(#3498db, 15%);
  }

  #{selector.append($base, ":disabled")} {
    opacity: 0.5;
    cursor: not-allowed;
  }
}
```

### Construction de sélecteurs complexes

```scss
$container: ".page";
$section: ".section";
$element: ".titre";

$s1: selector.append($container, $section);
$s2: selector.append($s1, $element);

.page {
  &.section {
    padding: 40px;

    &.titre {
      font-size: 24px;
      font-weight: bold;
    }
  }
}

// Équivalent avec selector.append
.page .section .titre {
  font-size: 24px;
}
```

---

## 24.4 — `selector.nest()` : Imbriquer des sélecteurs

La fonction `selector.nest()` imbrique un sélecteur dans un autre, en gérant correctement les combinateurs.

### Syntaxe

```scss
selector.nest($selecteurs...)
```

### Exemples

```scss
$parent: ".container";
$enfant: ".row";
$petit-enfant: ".col";

$imbrication: selector.nest($parent, $enfant, $petit-enfant);
// Résultat: .container .row .col
```

### Différence avec l'interpolation

```scss
$sel-1: ".parent";
$sel-2: ".child";

// Avec interpolation
#{$sel-1} #{$sel-2} {
  color: red;
}

// Avec selector.nest()
$imbrication: selector.nest($sel-1, $sel-2);
#{$imbrication} {
  color: red;
}
```

### Utilisation pratique : navigation

```scss
$nav: ".navigation";
$nav-item: ".nav-item";
$nav-link: ".nav-link";
$dropdown: ".dropdown";

$nav-complet: selector.nest($nav, $nav-item, $nav-link);
// .navigation .nav-item .nav-link

$nav-dropdown: selector.nest($nav, $nav-item, $dropdown);
// .navigation .nav-item .dropdown

.navigation {
  display: flex;

  #{selector.nest($nav, $nav-item)} {
    position: relative;

    &:hover #{selector.nest($dropdown)} {
      display: block;
    }
  }
}
```

### Génération de sélecteurs dynamiques

```scss
$profondeurs: 3;
$base-selector: ".niveau";

@for $i from 1 through $profondeurs {
  $sel: $base-selector;

  @for $j from 1 through $i {
    $sel: selector.append($sel, "#{$base-selector}-#{$j}");
  }

  #{$sel} {
    color: rgb(random(255), random(255), random(255));
  }
}
```

---

## 24.5 — `selector.is-superselector()` : Comparaison de sélecteurs

La fonction `selector.is-superselector()` vérifie si un sélecteur est un super-sélecteur d'un autre (c'est-à-dire s'il correspond à tous les éléments auxquels l'autre sélecteur correspond, et potentiellement plus).

### Syntaxe

```scss
selector.is-superselector($grand, $petit)
```

### Exemples

```scss
$test-1: selector.is-superselector(".parent", ".parent .child");
// true — .parent correspond à plus d'éléments

$test-2: selector.is-superselector(".parent .child", ".parent");
// false — .parent .child est plus spécifique

$test-3: selector.is-superselector("*", ".anything");
// true — * correspond à tout

$test-4: selector.is-superselector(".a.b", ".a");
// true — .a.b est plus spécifique

$test-5: selector.is-superselector(".a", ".a.b");
// false — .a est moins spécifique
```

### Utilisation pratique : vérification de hiérarchie

```scss
$chemin: ".app .main .content";

@mixin verifier-hierarchie($parent, $enfant) {
  @if selector.is-superselector($parent, $enfant) {
    @debug "'#{$parent}' est un parent de '#{$enfant}'";
  } @else {
    @debug "'#{$parent}' n'est PAS un parent de '#{$enfant}'";
  }
}

@include verifier-hierarchie(".app", ".app .main");
@include verifier-hierarchie(".sidebar", ".app .main");
```

### Validation de sélecteurs

```scss
$selecteur-cible: ".container .row";
$selecteur-test: ".container .row .col";

@if selector.is-superselector($selecteur-cible, $selecteur-test) {
  .resultat {
    border: 2px solid green;
  }
} @else {
  .resultat {
    border: 2px solid red;
  }
}
```

---

## 24.6 — `selector.simple-selectors()` : Extraire les sélecteurs simples

La fonction `selector.simple-selectors()` décompose un sélecteur en ses sélecteurs simples.

### Syntaxe

```scss
selector.simple-selectors($selecteur)
```

### Exemples

```scss
$complexe: ".class#id[attr]";

$simples: selector.simple-selectors($complexe);
// Résultat: ".class", "#id", "[attr]"

$sel: "div.container.active";
$simples-2: selector.simple-selectors($sel);
// Résultat: "div", ".container", ".active"
```

### Utilisation pratique : analyse de sélecteurs

```scss
$selecteur: ".card#special[data-type='primary']";

$parties: selector.simple-selectors($selecteur);

@each $partie in $parties {
  @debug "Partie du sélecteur: #{$partie}";
}

// Vérification de composants
$composants-requis: (".card", "#special");

@mixin verifier-composants($sel) {
  $parties: selector.simple-selectors($sel);
  $tous-present: true;

  @each $requis in $composants-requis {
    @if not list.index($parties, $requis) {
      $tous-present: false;
      @warn "Le sélecteur '#{$sel}' ne contient pas '#{$requis}'";
    }
  }

  @if $tous-present {
    @debug "Le sélecteur '#{$sel}' contient tous les composants requis";
  }
}

@include verifier-composants(".card#special");
```

---

## 24.7 — `selector.unify()` : Unifier des sélecteurs

La fonction `selector.unify()` combine deux sélecteurs en un seul, en supprimant les parties redondantes.

### Syntaxe

```scss
selector.unify($selecteur1, $selecteur2)
```

### Exemples

```scss
$unifie-1: selector.unify(".a", ".b");
// Résultat: .a.b

$unifie-2: selector.unify(".a.b", ".b");
// Résultat: .a.b

$unifie-3: selector.unify(".a", ".a");
// Résultat: .a

$unifie-4: selector.unify(".a", ".c");
// Résultat: .a.c
```

### Unification de sélecteurs complexes

```scss
$sel-1: ".container .row";
$sel-2: ".row .col";

$unifie: selector.unify($sel-1, $sel-2);
// Résultat: .container .row .col

// Cas où l'unification échoue
$sel-3: ".a";
$sel-4: ".b.c";

$unifie-2: selector.unify($sel-3, $sel-4);
// Résultat: .a.b.c
```

### Utilisation pratique : fusion de styles

```scss
$themes: (
  "dark": ".theme-dark",
  "light": ".theme-light"
);

$etats: (
  "hover": ":hover",
  "focus": ":focus"
);

@each $theme-nom, $theme-sel in $themes {
  @each $etat-nom, $etat-sel in $etats {
    $sel-unifie: selector.unify($theme-sel, $etat-sel);

    #{$sel-unifie} {
      @if $theme-nom == "dark" {
        background-color: darken(#333, 10%);
      } @else {
        background-color: lighten(#fff, 10%);
      }
    }
  }
}
```

---

## 24.8 — Exemples pratiques avancés

### Système de composants BEM

```scss
@use 'sass:selector';

@mixin bem($block, $element: "", $modifier: "") {
  $base: selector.parse(".#{$block}");

  @if $element != "" {
    $el-sel: selector.append($base, "__#{$element}");

    #{$el-sel} {
      @content;
    }
  } @else {
    #{$base} {
      @content;
    }

    @if $modifier != "" {
      $mod-sel: selector.append($base, "--#{$modifier}");

      #{$mod-sel} {
        @content;
      }
    }
  }
}

@include bem("card") {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
}

@include bem("card", "header") {
  padding: 16px;
  border-bottom: 1px solid #eee;
  font-weight: bold;
}

@include bem("card", "", "primary") {
  border-color: #3498db;
  box-shadow: 0 4px 15px rgba(#3498db, 0.2);
}
```

### Système de variantes dynamiques

```scss
@use 'sass:selector';

$variantes: (
  "primary": (
    "bg": #3498db,
    "text": #ffffff,
    "border": #2980b9
  ),
  "success": (
    "bg": #2ecc71,
    "text": #ffffff,
    "border": #27ae60
  ),
  "danger": (
    "bg": #e74c3c,
    "text": #ffffff,
    "border": #c0392b
  )
);

$etats: (
  "hover": ":hover",
  "focus": ":focus",
  "active": ":active",
  "disabled": ":disabled"
);

@each $var-nom, $var-props in $variantes {
  $base-sel: selector.parse(".btn-#{$var-nom}");

  // Style de base
  #{$base-sel} {
    background-color: map-get($var-props, "bg");
    color: map-get($var-props, "text");
    border: 2px solid map-get($var-props, "border");
  }

  // États
  @each $etat-nom, $etat-sel in $etats {
    $etat-sel-full: selector.append($base-sel, $etat-sel);

    #{$etat-sel-full} {
      @if $etat-nom == "hover" {
        background-color: darken(map-get($var-props, "bg"), 10%);
      } @else if $etat-nom == "focus" {
        box-shadow: 0 0 0 3px rgba(map-get($var-props, "bg"), 0.3);
      } @else if $etat-nom == "active" {
        background-color: darken(map-get($var-props, "bg"), 15%);
      } @else if $etat-nom == "disabled" {
        opacity: 0.5;
        cursor: not-allowed;
      }
    }
  }
}
```

### Système de grilles avec sélecteurs dynamiques

```scss
@use 'sass:selector';

$grille: (
  "container": ".container",
  "row": ".row",
  "col": ".col"
);

$tailles: (
  "sm": 576px,
  "md": 768px,
  "lg": 992px,
  "xl": 1200px
);

$nbre-colonnes: 12;

@each $taille-nom, $taille-px in $tailles {
  @media (min-width: $taille-px) {
    @for $i from 1 through $nbre-colonnes {
      $sel: selector.parse("#{$grille-col}.#{$taille-nom}-#{$i}");

      #{$sel} {
        width: percentage($i / $nbre-colonnes);
      }
    }
  }
}
```

### Sélecteurs conditionnels

```scss
@use 'sass:selector';

$mode: "dark";
$avec-animations: true;

@mixin generer-selecteurs($theme) {
  $base: selector.parse(".#{$theme}-theme");

  #{$base} {
    background-color: if($theme == "dark", #1a1a2e, #ffffff);
    color: if($theme == "dark", #e0e0e0, #333333);
  }

  @if $avec-animations {
    $anime-sel: selector.append($base, ".animated");

    #{$anime-sel} {
      transition: all 0.3s ease;
    }
  }
}

@include generer-selecteurs($mode);
```

---

## 24.9 — Exercices

### Exercice 1 : Système BEM

Créez un système BEM complet qui génère automatiquement les sélecteurs pour les blocs, éléments et modificateurs.

### Exercice 2 : Navigation responsive

Créez un système de navigation qui génère différents sélecteurs selon la taille d'écran.

### Exercice 3 : Thème dynamique

Créez un système de thème qui applique des styles différents selon la classe du thème.

### Exercice 4 : Grille responsive

Créez un système de grille qui génère les classes de colonnes pour différents breakpoints.

### Exercice 5 : Composants avec états

Créez un système de composants qui gère automatiquement les états (hover, focus, active, disabled).

---

## 24.10 — Résumé

| Fonction | Description | Exemple |
|----------|-------------|---------|
| `selector.parse()` | Analyser un sélecteur | `selector.parse(".class")` |
| `selector.append()` | Ajouter des suffixes | `selector.append(".btn", "--primary")` |
| `selector.nest()` | Imbriquer des sélecteurs | `selector.nest(".parent", ".child")` |
| `selector.is-superselector()` | Comparer des sélecteurs | `selector.is-superselector(".a", ".a .b")` |
| `selector.simple-selectors()` | Extraire les parties | `selector.simple-selectors(".a.b")` |
| `selector.unify()` | Unifier des sélecteurs | `selector.unify(".a", ".b")` |

---

**Prochain chapitre :** Nous explorerons le module `sass:meta` pour les méta-fonctions et le débogage.
