# Chapitre 21 : Les Chaînes — Module `sass:string`

## Introduction

Le module `sass:string` fournit des fonctions pour manipuler les chaînes de caractères en Sass. Bien que Sass soit principalement un langage de préprocesseur CSS, la manipulation de chaînes de caractères est essentielle pour générer du code CSS dynamique, créer des noms de classes, des variables CSS et des sélecteurs programmables. Ce chapitre explore en détail chaque fonction du module `sass:string` avec de nombreux exemples pratiques.

---

## 21.1 — Introduction au module `sass:string`

### Importation du module

```scss
@use 'sass:string';

.resultat {
  content: string.to-upper-case("bonjour");
}
```

### Types de chaînes en Sass

Sass reconnaît deux types de chaînes :

```scss
// Chaînes avec guillemets
$chaine-guillemets: "Bonjour le monde";
$chaine-apostrophes: 'Hello World';

// Chaînes sans guillemets (identifiants)
$identifiant: bonjour;

// Les deux sont des chaînes de caractères
$test: "texte" == text; // true
```

### Concaténation de chaînes

```scss
$prenom: "Jean";
$nom: "Dupont";

// Avec interpolation
$nom-complet: "#{$prenom} #{$nom}";

.contenu {
  content: $nom-complet; // "Jean Dupont"
}

// Sans interpolation (concaténation directe)
$chaine1: "Hello";
$chaine2: "World";
$concat: $chaine1 + " " + $chaine2; // "Hello World"
```

---

## 21.2 — `string.to-upper-case()` : Conversion en majuscules

La fonction `string.to-upper-case()` convertit tous les caractères d'une chaîne en majuscules.

### Syntaxe

```scss
string.to-upper-case($chaine)
```

### Exemples

```scss
$minuscule: string.to-upper-case("bonjour");
// Résultat: "BONJOUR"

$mixte: string.to-upper-case("bonJour MONde");
// Résultat: "BONJOUR MONDE"

$deja-majuscule: string.to-upper-case("HELLO");
// Résultat: "HELLO"

$avec-accents: string.to-upper-case("café résumé");
// Résultat: "CAFÉ RÉSUMÉ"
```

### Utilisation pratique

```scss
$nom-section: "hero";

.section-#{$nom-section} {
  &::before {
    content: string.to-upper-case($nom-section);
    // Affichera: "HERO"
  }
}

// Génération de classes CSS avec noms en majuscules
$etats: "active", "hover", "focus";

@each $etat in $etats {
  .bouton-#{$etat} {
    &::after {
      content: string.to-upper-case($etat);
    }
  }
}
```

### Système de labels

```scss
$niveaux: "primaire", "secondaire", "tertiaire";

@each $niveau in $niveaux {
  .label-#{$niveau} {
    &::before {
      content: string.to-upper-case($niveau);
      font-weight: bold;
      text-transform: uppercase;
      letter-spacing: 1px;
    }
  }
}
```

---

## 21.3 — `string.to-lower-case()` : Conversion en minuscules

La fonction `string.to-lower-case()` convertit tous les caractères d'une chaîne en minuscules.

### Syntaxe

```scss
string.to-lower-case($chaine)
```

### Exemples

```scss
$majuscule: string.to-lower-case("BONJOUR");
// Résultat: "bonjour"

$mixte: string.to-lower-case("BonJour MONde");
// Résultat: "bonjour monde"

$deja-minuscule: string.to-lower-case("hello");
// Résultat: "hello"

$avec-accents: string.to-lower-case("CAFÉ RÉSUMÉ");
// Résultat: "café résumé"
```

### Utilisation pratique

```scss
$nom-composant: "BoutonPrincipal";

// Conversion en kebab-case pour CSS
$nom-css: string.to-lower-case($nom-composant);
// "boutonprincipal"

// Meilleure approche avec une logique de conversion
@mixin kebab-case($chaine) {
  // Pour un usage réel, on utiliserait des mixins plus complexes
  $resultat: string.to-lower-case($chaine);
  // Dans un cas réel, on gérerait aussi l'insertion de tirets
}
```

### Normalisation des noms

```scss
$noms-utilisateurs: "ADMIN", "User", "MODERATOR";

@each $nom in $noms-utilisateurs {
  .role-#{string.to-lower-case($nom)} {
    // Génère .role-admin, .role-user, .role-moderator
  }
}
```

---

## 21.4 — `string.insert()` : Insérer du texte

La fonction `string.insert()` insère une sous-chaîne dans une chaîne à une position donnée.

### Syntaxe

```scss
string.insert($chaine, $inserer, $position)
```

La position peut être un nombre positif (depuis le début) ou négatif (depuis la fin).

### Exemples de base

```scss
$base: "Hello World";

$inserer-1: string.insert($base, "Beautiful ", 6);
// Résultat: "Hello Beautiful World"

$inserer-2: string.insert($base, " Amazing", -1);
// Résultat: "Hello World Amazing"

$inserer-3: string.insert($base, "Say ", 0);
// Résultat: "Say Hello World"

$inserer-4: string.insert($base, "!", -1);
// Résultat: "Hello World!"
```

### Position positive vs négative

```scss
$chaine: "abcdef";

$pos-1: string.insert($chaine, "X", 1);  // "Xabcdef"
$pos-2: string.insert($chaine, "X", 3);  // "abXcdef"
$pos-3: string.insert($chaine, "X", -1); // "abcdefX"
$pos-4: string.insert($chaine, "X", -3); // "abcXdef"
$pos-5: string.insert($chaine, "X", 0);  // "Xabcdef"
```

### Utilisation pratique : Préfixes et suffixes

```scss
$nom-base: "button";

@mixin ajouter-pref($base, $pref) {
  $resultat: string.insert($base, $pref, 0);
  .#{$resultat} {
    @content;
  }
}

@mixin ajouter-suffix($base, $suffix) {
  $resultat: string.insert($base, $suffix, -1);
  .#{$resultat} {
    @content;
  }
}

@include ajouter-pref($nom-base, "btn-") {
  // Génère .btn-button
  background-color: blue;
}

// Ajout de préfixes conditionnels
$prefixes: "-webkit-", "-moz-", "-ms-";
$prop: "transform";

@each $prefix in $prefixes {
  $prop-complete: string.insert($prop, $prefix, 0);
  // "-webkit-transform", "-moz-transform", "-ms-transform"
}
```

---

## 21.5 — `string.index()` : Trouver la position

La fonction `string.index()` retourne la position de la première occurrence d'une sous-chaîne dans une chaîne.

### Syntaxe

```scss
string.index($chaine, $recherche)
```

### Exemples de base

```scss
$position-1: string.index("Hello World", "World");
// Résultat: 7

$position-2: string.index("Hello World", "o");
// Résultat: 5

$position-3: string.index("Hello World", "xyz");
// Résultat: null (non trouvé)

$position-4: string.index("Hello World", "Hello");
// Résultat: 1

$position-5: string.index("Hello World", "");
// Résultat: 1 (chaîne vide trouvée au début)
```

### Recherche avec casse

```scss
$chaine: "Hello World HELLO";

$pos-1: string.index($chaine, "hello");  // null (casse différente)
$pos-2: string.index($chaine, "Hello");  // 1
$pos-3: string.index($chaine, "HELLO");  // 14
```

### Utilisation pratique : Vérification de contenu

```scss
$couleur: "#3498db";

@mixin verifier-couleur($chaine, $type) {
  $position: string.index($chaine, $type);

  @if $position {
    // La sous-chaîne a été trouvée
    @debug "#{$chaine} contient '#{$type}' à la position #{$position}";
  } @else {
    // La sous-chaîne n'a pas été trouvée
    @debug "#{$chaine} ne contient pas '#{$type}'";
  }
}

@include verifier-couleur("background-color", "color");
// Output: "background-color contient 'color' à la position 11"
```

### Parsing de propriétés CSS

```scss
$propriete: "background-color";

$contient-background: string.index($propriete, "background");
$contient-color: string.index($propriete, "color");

.test {
  @if $contient-background and $contient-color {
    // C'est une propriété de background avec color
    background: #3498db;
  }
}
```

---

## 21.6 — `string.length()` : Longueur d'une chaîne

La fonction `string.length()` retourne le nombre de caractères d'une chaîne.

### Syntaxe

```scss
string.length($chaine)
```

### Exemples

```scss
$longueur-1: string.length("Hello");      // 5
$longueur-2: string.length("");           // 0
$longueur-3: string.length("Bonjour");   // 7
$longueur-4: string.length("a");         // 1
$longueur-5: string.length("café");      // 4 (les accents comptent pour 1 caractère)
```

### Utilisation pratique

```scss
$chaine: "Sass est génial";

.conteneur {
  // Vérifier la longueur avant de tronquer
  @if string.length($chaine) > 20 {
    // Tronquer si trop long
    content: string.slice($chaine, 1, 20) + "...";
  } @else {
    content: $chaine;
  }
}
```

### Validation de saisie

```scss
$code-postal: "75001";

.input-code {
  @if string.length($code-postal) != 5 {
    border-color: red;

    &::after {
      content: "Le code postal doit contenir 5 chiffres";
    }
  } @else {
    border-color: green;
  }
}
```

---

## 21.7 — `string.slice()` : Extraire une sous-chaîne

La fonction `string.slice()` extrait une partie d'une chaîne de caractères.

### Syntaxe

```scss
string.slice($chaine, $debut, $fin: null)
```

### Exemples de base

```scss
$chaine: "Hello World";

$slice-1: string.slice($chaine, 1, 5);    // "Hello"
$slice-2: string.slice($chaine, 7);        // "World"
$slice-3: string.slice($chaine, 1, 1);     // "H"
$slice-4: string.slice($chaine, -5);       // "World"
$slice-5: string.slice($chaine, -5, -1);   // "Worl"
$slice-6: string.slice($chaine, 1, -6);    // "Hello"
```

### Position négative

```scss
$chaine: "abcdef";

$pos-neg-1: string.slice($chaine, -3);     // "def"
$pos-neg-2: string.slice($chaine, -3, -1); // "de"
$pos-neg-3: string.slice($chaine, 1, -3);  // "abc"
$pos-neg-4: string.slice($chaine, -4, -2); // "cde"
```

### Utilisation pratique : Tronquage de texte

```scss
$texte-complet: "Ceci est un très long texte qui doit être tronqué";

@mixin tronquer-texte($texte, $longueur-max) {
  @if string.length($texte) > $longueur-max {
    content: string.slice($texte, 1, $longueur-max) + "...";
  } @else {
    content: $texte;
  }
}

.titre-court {
  &::after {
    @include tronquer-texte($texte-complet, 20);
  }
}
```

### Extraction de parties d'un nom de fichier

```scss
$fichier: "style-components.css";

$nom: string.slice($fichier, 1, string.index($fichier, ".") - 1);
// "style-components"

$extension: string.slice($fichier, string.index($fichier, ".") + 1);
// "css"

.fichier-info {
  &::before {
    content: "Nom: #{$nom}";
  }
  &::after {
    content: "Extension: #{$extension}";
  }
}
```

---

## 21.8 — `string.quote()` et `string.unquote()`

Ces fonctions permettent respectivement d'ajouter et de supprimer les guillemets d'une chaîne.

### `string.quote()` — Ajouter des guillemets

```scss
$sans-guillemets: bonjour;
$avec-guillemets: string.quote($sans-guillemets);
// Résultat: "bonjour"

.content {
  content: $avec-guillemets;
}
```

### `string.unquote()` — Supprimer les guillemets

```scss
$avec-guillemets: "bonjour";
$sans-guillemets: string.unquote($avec-guillemets);
// Résultat: bonjour

.selector {
  // Utile pour générer des noms de classes dynamiques
  content: $sans-guillemets;
}
```

### Utilisation pratique : Noms de classes dynamiques

```scss
@mixin generer-classe($nom) {
  $nom-sans-quotes: string.unquote($nom);

  .#{$nom-sans-quotes} {
    @content;
  }
}

@include generer-classe("ma-classe") {
  background-color: blue;
  color: white;
}
// Génère: .ma-classe { background-color: blue; color: white; }
```

### Génération de variables CSS

```scss
$propriete: "font-size";
$valeur: "16px";

$prop-sans-quotes: string.unquote($propriete);
$val-sans-quotes: string.unquote($valeur);

:root {
  --#{$prop-sans-quotes}: #{$val-sans-quotes};
}
// Génère: --font-size: 16px;
```

---

## 21.9 — Exemples pratiques avancés

### Système de noms de composants

```scss
@use 'sass:string';

$composants: (
  "Button": "btn",
  "Input": "input",
  "Card": "card",
  "Modal": "modal"
);

@each $nom, $prefix in $composants {
  $nom-css: string.to-lower-case($nom);
  $classe: string.insert($nom-css, "#{$prefix}-", 0);

  .#{$classe} {
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 8px 12px;
  }

  // États
  .#{$classe}--active {
    border-color: #3498db;
  }

  .#{$classe}--disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}
```

### Parseur de propriétés CSS

```scss
@use 'sass:string';

@mixin analyser-propriete($prop) {
  $longueur: string.length($prop);

  @if $longueur > 20 {
    @warn "La propriété '#{$prop}' est très longue (#{$longueur} caractères)";
  }

  // Vérifier si c'est une propriété avec préfixe
  $a-prefixe-webkit: string.index($prop, "-webkit-");
  $a-prefixe-moz: string.index($prop, "-moz-");
  $a-prefixe-ms: string.index($prop, "-ms-");

  @if $a-prefixe-webkit or $a-prefixe-moz or $a-prefixe-ms {
    @debug "Propriété avec préfixe vendor: #{$prop}";
  }
}

@include analyser-propriete("-webkit-transform");
@include analyser-propriete("background-color");
```

### Générateur de classes utilitaires

```scss
@use 'sass:string';

$utilitaires: (
  "display": ("flex", "block", "inline", "none"),
  "position": ("relative", "absolute", "fixed", "sticky"),
  "text-align": ("left", "center", "right", "justify")
);

@each $prop, $valeurs in $utilitaires {
  @each $valeur in $valeurs {
    $nom-prop: string.replace($prop, " ", "-");
    $nom-valeur: $valeur;
    $classe: string.insert($nom-prop, $nom-valeur, 0);

    .#{$classe} {
      #{$prop}: $valeur;
    }
  }
}
```

### Système de naming convention

```scss
@use 'sass:string';

@mixin bem($block, $element: "", $modifier: "") {
  $block-unquote: string.unquote($block);

  .#{$block-unquote} {
    @content;

    @if $element != "" {
      $element-unquote: string.unquote($element);
      &__#{$element-unquote} {
        // Éléments du bloc
      }
    }

    @if $modifier != "" {
      $modifier-unquote: string.unquote($modifier);
      &--#{$modifier-unquote} {
        // Modificateurs du bloc
      }
    }
  }
}

@include bem("card", "header") {
  .card__header {
    padding: 16px;
    border-bottom: 1px solid #eee;
  }
}
```

---

## 21.10 — Exercices

### Exercice 1 : Normalisation de texte

Créez une fonction qui normalise une chaîne en la convertissant en minuscules, en supprimant les espaces en début et fin, et en remplaçant les espaces multiples par un seul espace.

### Exercice 2 : Générateur de slugs

Créez un mixin qui transforme un titre en slug URL-friendly (minuscules, tirets au lieu d'espaces, suppression des caractères spéciaux).

### Exercice 3 : Parseur de nom complet

Créez une fonction qui prend un nom complet "Prénom Nom" et retourne un objet avec les parties séparées.

### Exercice 4 : Vérificateur de format

Créez une fonction qui vérifie si une chaîne correspond à un format donné (email, téléphone, code postal).

### Exercice 5 : Système de templates

Créez un système de templates qui remplace des placeholders dans une chaîne (ex: "Bonjour {{nom}}, bienvenue à {{ville}}").

---

## 21.11 — Résumé

| Fonction | Description | Exemple | Résultat |
|----------|-------------|---------|----------|
| `string.to-upper-case()` | Conversion en majuscules | `string.to-upper-case("bonjour")` | "BONJOUR" |
| `string.to-lower-case()` | Conversion en minuscules | `string.to-lower-case("BONJOUR")` | "bonjour" |
| `string.insert()` | Insérer du texte | `string.insert("Hello", "World", 6)` | "HelloWorld" |
| `string.index()` | Trouver la position | `string.index("Hello", "ell")` | 2 |
| `string.length()` | Longueur de la chaîne | `string.length("Hello")` | 5 |
| `string.slice()` | Extraire une sous-chaîne | `string.slice("Hello", 1, 3)` | "Hel" |
| `string.quote()` | Ajouter des guillemets | `string.quote(hello)` | "hello" |
| `string.unquote()` | Supprimer les guillemets | `string.unquote("hello")` | hello |
| `string.replace()` | Remplacer du texte | `string.replace("Hello", "l", "r")` | "Herro" |

---

**Prochain chapitre :** Nous explorerons le module `sass:list` pour la manipulation des listes.
