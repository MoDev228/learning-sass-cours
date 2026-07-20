# Chapitre 18 : Les Conditions en Sass

## Introduction

Les conditions sont un pilier fondamental de toute language de programmation, et Sass ne fait pas exception. Grâce aux instructions conditionnelles `@if`, `@else` et `@else if`, vous pouvez créer des feuilles de styles dynamiques qui s'adaptent en fonction de variables, de paramètres ou de toute autre expression évaluable. Ce chapitre vous guidera à travers tous les aspects des conditions en Sass, des bases jusqu'aux cas avancés d'utilisation.

---

## 18.1 — L'instruction `@if`

L'instruction `@if` est la condition la plus simple et la plus utilisée en Sass. Elle évalue une expression et n'exécute le bloc de code qui suit que si l'expression retourne `true`.

### Syntaxe de base

```scss
$condition: true;

@if $condition {
  // Ce code s'exécute si $condition est true
  color: red;
}
```

### Exemple simple avec une variable booléenne

```scss
$theme-dark: true;

@if $theme-dark {
  body {
    background-color: #1a1a2e;
    color: #e0e0e0;
  }
}
```

Résultat CSS :

```css
body {
  background-color: #1a1a2e;
  color: #e0e0e0;
}
```

### Vérification d'existence d'une variable

Une utilisation très courante de `@if` est de vérifier si une variable a été définie avant de l'utiliser :

```scss
$primary-color: #3498db;

.container {
  @if $primary-color {
    border: 2px solid $primary-color;
    color: darken($primary-color, 10%);
  }
}
```

### Utilisation avec des opérations de comparaison

```scss
$largeur-ecran: 1200px;
$marge: 20px;

@media (min-width: 768px) {
  .layout {
    @if $largeur-ecran > 1000px {
      padding: $marge * 2;
    } @else {
      padding: $marge;
    }
  }
}
```

---

## 18.2 — L'instruction `@else`

L'instruction `@else` permet d'exécuter un bloc de code alternatif lorsque la condition `@if` retourne `false`.

### Syntaxe

```scss
$variable: false;

@if $variable {
  // Ce code s'exécute si $variable est true
  color: green;
} @else {
  // Ce code s'exécute si $variable est false
  color: gray;
}
```

### Exemple pratique : thème clair / thème sombre

```scss
$theme-sombre: false;

body {
  @if $theme-sombre {
    background-color: #121212;
    color: #f5f5f5;
    a {
      color: #bb86fc;
    }
  } @else {
    background-color: #ffffff;
    color: #333333;
    a {
      color: #007bff;
    }
  }
}
```

Résultat CSS :

```css
body {
  background-color: #ffffff;
  color: #333333;
}
body a {
  color: #007bff;
}
```

### Exemple avec une variable numérique

```scss
$nombre-elements: 0;

.grid {
  @if $nombre-elements > 0 {
    display: grid;
    grid-template-columns: repeat($nombre-elements, 1fr);
  } @else {
    display: block;
  }
}
```

---

## 18.3 — L'instruction `@else if`

L'instruction `@else if` permet de tester plusieurs conditions de manière séquentielle. C'est l'équivalent du `else if` dans d'autres langages.

### Syntaxe

```scss
$jour: "lundi";

@if $jour == "lundi" {
  color: red;
} @else if $jour == "mardi" {
  color: orange;
} @else if $jour == "mercredi" {
  color: yellow;
} @else {
  color: gray;
}
```

### Exemple complet : système d'alertes

```scss
$type-alerte: "info";

.alerte {
  padding: 15px 20px;
  border-radius: 5px;
  margin-bottom: 10px;

  @if $type-alerte == "succes" {
    background-color: #d4edda;
    border: 1px solid #28a745;
    color: #155724;
  } @else if $type-alerte == "avertissement" {
    background-color: #fff3cd;
    border: 1px solid #ffc107;
    color: #856404;
  } @else if $type-alerte == "danger" {
    background-color: #f8d7da;
    border: 1px solid #dc3545;
    color: #721c24;
  } @else if $type-alerte == "info" {
    background-color: #d1ecf1;
    border: 1px solid #17a2b8;
    color: #0c5460;
  } @else {
    background-color: #f8f9fa;
    border: 1px solid #dee2e6;
    color: #212529;
  }
}
```

Résultat CSS :

```css
.alerte {
  padding: 15px 20px;
  border-radius: 5px;
  margin-bottom: 10px;
  background-color: #d1ecf1;
  border: 1px solid #17a2b8;
  color: #0c5460;
}
```

### Chaînage multiple de `@else if`

```scss
$note: 15;

@appreciation {
  @if $note >= 16 {
    content: "Très bien";
  } @else if $note >= 14 {
    content: "Bien";
  } @else if $note >= 12 {
    content: "Assez bien";
  } @else if $note >= 10 {
    content: "Passable";
  } @else {
    content: "Insuffisant";
  }
}
```

---

## 18.4 — Les comparaisons

Sass supporte plusieurs opérateurs de comparaison qui peuvent être utilisés dans les expressions conditionnelles.

### Les opérateurs disponibles

| Opérateur | Signification |
|-----------|---------------|
| `==`      | Égal à        |
| `!=`      | Différent de  |
| `>`       | Supérieur à   |
| `<`       | Inférieur à   |
| `>=`      | Supérieur ou égal à |
| `<=`      | Inférieur ou égal à |

### Exemples de chaque opérateur

#### Égalité (`==`)

```scss
$langue: "fr";

.header {
  @if $langue == "fr" {
    content: "Bienvenue sur notre site";
  } @else if $langue == "en" {
    content: "Welcome to our website";
  } @else {
    content: "Welcome / Bienvenue";
  }
}
```

#### Différence (`!=`)

```scss
$role: "admin";

.dashboard {
  @if $role != "admin" {
    display: none;
  } @else {
    display: block;
  }
}
```

#### Supériorité (`>`)

```scss
$nbre-enfants: 5;

.liste-parente {
  @if $nbre-enfants > 3 {
    columns: 2;
    column-gap: 20px;
  } @else {
    columns: 1;
  }
}
```

#### Infériorité (`<`)

```scss
$z-index-modal: 100;

.overlay {
  z-index: $z-index-modal - 1;

  @if $z-index-modal < 50 {
    position: fixed;
  } @else {
    position: absolute;
  }
}
```

#### Supériorité ou égalité (`>=`)

```scss
$ breakpoint: 768px;

@media (min-width: $breakpoint) {
  .sidebar {
    @if $breakpoint >= 1024px {
      width: 300px;
    } @else {
      width: 250px;
    }
  }
}
```

#### Infériorité ou égalité (`<=`)

```scss
$espace-restant: 10px;

.bouton {
  @if $espace-restant <= 5px {
    padding: 2px 5px;
  } @else if $espace-restant <= 15px {
    padding: 5px 10px;
  } @else {
    padding: 10px 20px;
  }
}
```

### Comparaison de chaînes de caractères

```scss
$mode: "production";

.application {
  @if $mode == "production" {
    box-shadow: none;
  } @else if $mode == "développement" {
    box-shadow: 0 0 0 2px red;
  }
}
```

### Comparaison avec des couleurs

```scss
$couleur-primaire: #3498db;

.bandeau {
  @if $couleur-primaire == #3498db {
    border: 3px solid $couleur-primaire;
  }
}
```

---

## 18.5 — Les valeurs booléennes

Sass possède deux valeurs booléennes natifs : `true` et `false`. Elles sont essentielles pour les conditions.

### Déclaration de variables booléennes

```scss
$afficher-ombre: true;
$mode-compact: false;
$utiliser-animation: true;
```

### Utilisation dans des conditions

```scss
.card {
  padding: 20px;
  border: 1px solid #ddd;

  @if $afficher-ombre {
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }

  @if $mode-compact {
    padding: 10px;
    font-size: 0.9em;
  }
}
```

### Inverser un booléen avec `not`

```scss
$is-visible: true;

.modal {
  @if not $is-visible {
    display: none;
  } @else {
    display: block;
  }
}
```

### Combinaison de booléens

```scss
$theme-dark: true;
$reduced-motion: false;
$high-contrast: true;

body {
  @if $theme-dark and $high-contrast {
    background-color: #000000;
    color: #ffffff;
  } @else if $theme-dark {
    background-color: #1a1a2e;
    color: #e0e0e0;
  } @else {
    background-color: #ffffff;
    color: #333333;
  }

  @if $reduced-motion {
    * {
      animation: none !important;
      transition: none !important;
    }
  }
}
```

---

## 18.6 — Exemples pratiques avancés

### Système de grille responsive

```scss
$taille-ecran: "desktop";
$nbre-colonnes: 12;
$gouttiere: 20px;

.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 $gouttiere;

  @if $taille-ecran == "mobile" {
    max-width: 576px;
    padding: 0 15px;
  } @else if $taille-ecran == "tablet" {
    max-width: 768px;
    padding: 0 20px;
  } @else if $taille-ecran == "desktop" {
    max-width: 1200px;
    padding: 0 30px;
  } @else if $taille-ecran == "wide" {
    max-width: 1400px;
    padding: 0 40px;
  }
}

.colonne {
  display: inline-block;
  margin: 0 ($gouttiere / 2);

  @if $nbre-colonnes == 12 {
    width: calc(100% / 12 - #{$gouttiere});
  } @else if $nbre-colonnes == 6 {
    width: calc(100% / 6 - #{$gouttiere});
  } @else if $nbre-colonnes == 4 {
    width: calc(100% / 4 - #{$gouttiere});
  } @else {
    width: calc(100% / #{$nbre-colonnes} - #{$gouttiere});
  }
}
```

### Système de typographie adaptatif

```scss
$taille-police: "medium";

:root {
  @if $taille-police == "small" {
    font-size: 14px;
    line-height: 1.4;
  } @else if $taille-police == "medium" {
    font-size: 16px;
    line-height: 1.5;
  } @else if $taille-police == "large" {
    font-size: 18px;
    line-height: 1.6;
  } @else if $taille-police == "xlarge" {
    font-size: 20px;
    line-height: 1.7;
  }
}

h1 {
  @if $taille-police == "small" {
    font-size: 2em;
  } @else if $taille-police == "medium" {
    font-size: 2.5em;
  } @else {
    font-size: 3em;
  }
}

h2 {
  @if $taille-police == "small" {
    font-size: 1.5em;
  } @else if $taille-police == "medium" {
    font-size: 1.75em;
  } @else {
    font-size: 2em;
  }
}
```

### Gestion des prefixes navigateurs

```scss
$prefixes-navigateurs: true;

.flex-container {
  display: flex;

  @if $prefixes-navigateurs {
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
  }

  flex-wrap: wrap;

  @if $prefixes-navigateurs {
    -webkit-flex-wrap: wrap;
    -ms-flex-wrap: wrap;
  }

  justify-content: center;

  @if $prefixes-navigateurs {
    -webkit-justify-content: center;
    -ms-flex-pack: center;
  }
}
```

### Système de couleurs conditionnel

```scss
$couleur-primaire: #3498db;
$mode-accessible: true;
$luminosite: "normale";

.bouton {
  background-color: $couleur-primaire;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;

  @if $mode-accessible {
    border: 2px solid darken($couleur-primaire, 20%);
    font-weight: bold;
    font-size: 16px;
  }

  @if $luminosite == "forte" {
    box-shadow: 0 4px 15px rgba($couleur-primaire, 0.5);
  } @else if $luminosite == "faible" {
    box-shadow: 0 2px 5px rgba($couleur-primaire, 0.2);
  } @else {
    box-shadow: 0 2px 10px rgba($couleur-primaire, 0.3);
  }

  &:hover {
    @if $mode-accessible {
      background-color: darken($couleur-primaire, 15%);
    } @else {
      background-color: darken($couleur-primaire, 10%);
    }
  }

  &:disabled {
    @if $mode-accessible {
      opacity: 0.5;
      cursor: not-allowed;
    } @else {
      opacity: 0.7;
      cursor: pointer;
    }
  }
}
```

### Système de thème multi-contextes

```scss
$contexte: "email";
$format: "html";

.email-template {
  @if $contexte == "email" {
    max-width: 600px;
    margin: 0 auto;
    font-family: Arial, sans-serif;

    @if $format == "text" {
      border: none;
      padding: 10px;
    } @else {
      border: 1px solid #ddd;
      padding: 20px;
      border-radius: 8px;
    }
  } @else if $contexte == "landing" {
    max-width: 1200px;
    margin: 0 auto;
    font-family: 'Segoe UI', sans-serif;
    padding: 40px;
  } @else if $contexte == "blog" {
    max-width: 800px;
    margin: 0 auto;
    font-family: Georgia, serif;
    padding: 30px;
    line-height: 1.8;
  }
}
```

### Conditions imbriquées

```scss
$est-connecte: true;
$role-utilisateur: "premium";
$peut-telecharger: true;

.contenu-prive {
  @if $est-connecte {
    display: block;

    @if $role-utilisateur == "premium" {
      .ressources {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 20px;
      }

      @if $peut-telecharger {
        .bouton-telecharger {
          display: inline-block;
          background-color: #28a745;
          color: white;
          padding: 8px 16px;
          border-radius: 4px;
        }
      }
    } @else if $role-utilisateur == "gratuit" {
      .ressources {
        display: grid;
        grid-template-columns: repeat(1, 1fr);
        gap: 10px;
      }

      .bouton-telecharger {
        display: none;
      }
    }
  } @else {
    display: none;

    .message-connexion {
      display: block;
      text-align: center;
      padding: 40px;
      background-color: #f8f9fa;
      border-radius: 8px;
    }
  }
}
```

### Création de classes utilitaires conditionnelles

```scss
$avec-marges: true;
$avec-paddings: true;
$avec-ombres: false;
$avec-bordures: true;

@mixin utilitaires-base {
  @if $avec-marges {
    .m-0 { margin: 0; }
    .m-1 { margin: 0.25rem; }
    .m-2 { margin: 0.5rem; }
    .m-3 { margin: 1rem; }
    .m-4 { margin: 1.5rem; }
    .m-5 { margin: 3rem; }
  }

  @if $avec-paddings {
    .p-0 { padding: 0; }
    .p-1 { padding: 0.25rem; }
    .p-2 { padding: 0.5rem; }
    .p-3 { padding: 1rem; }
    .p-4 { padding: 1.5rem; }
    .p-5 { padding: 3rem; }
  }

  @if $avec-ombres {
    .ombre-sm { box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12); }
    .ombre-md { box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); }
    .ombre-lg { box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15); }
  }

  @if $avec-bordures {
    .b-0 { border: none; }
    .b-1 { border: 1px solid #dee2e6; }
    .b-2 { border: 2px solid #dee2e6; }
  }
}

@include utilitaires-base;
```

---

## 18.7 — Expressions complexes dans les conditions

### Utilisation de `and` et `or`

```scss
$age: 25;
$est-etudiant: false;

.tarif {
  @if $age < 18 or $est-etudiant {
    prix: 10px;
    label: "Tarif réduit";
  } @else if $age >= 18 and $age < 65 {
    prix: 20px;
    label: "Tarif normal";
  } @else {
    prix: 15px;
    label: "Tarif senior";
  }
}
```

### Conditions avec des opérations mathématiques

```scss
$nbre-produits: 15;
$prix-unitaire: 29.99;

.panier {
  total: $nbre-produits * $prix-unitaire;

  @if $nbre-produits >= 10 {
    remise: 15%;
    total-apres-remise: ($nbre-produits * $prix-unitaire) * 0.85;
  } @else if $nbre-produits >= 5 {
    remise: 10%;
    total-apres-remise: ($nbre-produits * $prix-unitaire) * 0.90;
  } @else {
    remise: 0%;
    total-apres-remise: $nbre-produits * $prix-unitaire;
  }
}
```

### Conditions avec des listes

```scss
$fruits: "pomme", "banane", "cerise";

.liste-fruits {
  @if length($fruits) > 2 {
    columns: 3;
  } @else if length($fruits) > 1 {
    columns: 2;
  } @else {
    columns: 1;
  }

  @each $fruit in $fruits {
    .#{$fruit}::before {
      content: "#{$fruit}";
    }
  }
}
```

---

## 18.8 — Bonnes pratiques

### 1. Limiter l'imbrication des conditions

```scss
// ❌ À éviter : trop d'imbrication
@if $a {
  @if $b {
    @if $c {
      @if $d {
        color: red;
      }
    }
  }
}

// ✅ À préférer : conditions combinées
@if $a and $b and $c and $d {
  color: red;
}
```

### 2. Utiliser des variables descriptives

```scss
// ❌ Difficile à comprendre
@if $x == 1 {
  color: red;
}

// ✅ Claire et lisible
$etat-actif: true;

@if $etat-actif {
  color: red;
}
```

### 3. Organiser les conditions du plus spécifique au plus général

```scss
$role: "super-admin";

.permissions {
  @if $role == "super-admin" {
    // Le plus spécifique en premier
    access: "all";
  } @else if $role == "admin" {
    access: "write";
  } @else if $role == "editor" {
    access: "edit";
  } @else {
    // Le plus général en dernier
    access: "read";
  }
}
```

---

## 18.9 — Exercices

### Exercice 1 : Système d'alertes

Créez un système d'alertes qui prend une variable `$type-alerte` et affiche un style différent pour chaque type : `succes`, `avertissement`, `erreur`, `info`.

```scss
$type-alerte: "avertissement";

// Votre code ici
```

### Exercice 2 : Thème adaptatif

Créez un thème qui s'adapte selon la variable `$mode-utilisateur` qui peut être `"debutant"`, `"intermediaire"` ou `"expert"`. Le mode expert affiche plus d'options, le mode débutant masque les fonctionnalités avancées.

```scss
$mode-utilisateur: "expert";

// Votre code ici
```

### Exercice 3 : Grille responsive

Créez un système de grille avec les variables `$nbre-colonnes` et `$taille-ecran`. La grille doit s'adapter selon le nombre de colonnes (1 à 12) et la taille d'écran.

```scss
$nbre-colonnes: 6;
$taille-ecran: "desktop";

// Votre code ici
```

### Exercice 4 : Prix avec réductions

Créez un système de prix qui applique automatiquement des réductions selon la quantité :
- 1-4 articles : prix normal
- 5-9 articles : 5% de réduction
- 10-19 articles : 10% de réduction
- 20+ articles : 20% de réduction

```scss
$quantite: 12;
$prix-unitaire: 50px;

// Votre code ici
```

### Exercice 5 : Mode d'accessibilité

Créez un mode d'accessibilité qui, quand `$mode-accessible` est `true`, augmente les contrastes, ajoute des bordures visibles, et agrandit la police. Sinon, utilisez les styles par défaut.

```scss
$mode-accessible: true;

// Votre code ici
```

---

## 18.10 — Résumé

| Concept | Syntaxe | Description |
|---------|---------|-------------|
| `@if` | `@if condition { }` | Exécute le bloc si la condition est vraie |
| `@else` | `@else { }` | Exécute le bloc si la condition précédente est fausse |
| `@else if` | `@else if condition { }` | Teste une nouvelle condition |
| `==` | `$a == $b` | Teste l'égalité |
| `!=` | `$a != $b` | Teste la différence |
| `>` | `$a > $b` | Teste la supériorité |
| `<` | `$a < $b` | Teste l'infériorité |
| `>=` | `$a >= $b` | Teste la supériorité ou égalité |
| `<=` | `$a <= $b` | Teste l'infériorité ou égalité |
| `and` | `$a and $b` | Combinaison logique ET |
| `or` | `$a or $b` | Combinaison logique OU |
| `not` | `not $a` | Négation logique |

---

**Prochain chapitre :** Nous verrons le module `sass:color` qui permet de manipuler les couleurs de manière avancée.
