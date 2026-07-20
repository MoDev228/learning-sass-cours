# Chapitre 22 : Les Listes (Module) — Module `sass:list`

## Introduction

Les listes sont l'un des types de données les plus fondamentaux en Sass. Elles permettent de regrouper des valeurs, d'itérer sur des collections et de créer des systèmes de design flexibles. Le module `sass:list` fournit des fonctions puissantes pour manipuler, transformer et interroger les listes. Ce chapitre explore en détail chaque fonction du module `sass:list` avec de nombreux exemples pratiques.

---

## 22.1 — Introduction aux listes en Sass

### Création de listes

```scss
// Liste simple
$fruits: "pomme", "banane", "cerise";

// Liste avec parenthèses (explicite)
$couleurs: (rouge, vert, bleu);

// Liste vide
$liste-vide: ();

// Liste avec un seul élément
$singleton: "uniquement";
```

### Types de séparateurs

```scss
// Séparateur par espace (défaut)
$liste-espace: 1 2 3 4 5;

// Séparateur par virgule
$liste-virgule: 1, 2, 3, 4, 5;

// Séparateur par trait
$liste-trait: 1 / 2 / 3 / 4 / 5;
```

### Accès aux éléments

```scss
$fruits: "pomme", "banane", "cerise";

$premier: nth($fruits, 1);    // "pomme"
$deuxieme: nth($fruits, 2);   // "banane"
$troisieme: nth($fruits, 3);  // "cerise"

$longueur: length($fruits);   // 3
```

---

## 22.2 — `list.append()` : Ajouter un élément

La fonction `list.append()` ajoute un élément à la fin d'une liste.

### Syntaxe

```scss
list.append($liste, $valeur, $separator: null)
```

Le paramètre `$separator` est optionnel. Si non spécifié, le séparateur est conservé.

### Exemples de base

```scss
$fruits: "pomme", "banane";
$fruits-ajoutes: list.append($fruits, "cerise");
// Résultat: "pomme", "banane", "cerise"

$couleurs: "rouge";
$couleurs-ajoutees: list.append($couleurs, "vert");
// Résultat: "rouge" "vert"

$vide: ();
$listee: list.append($vide, 1);
// Résultat: 1
```

### Changement de séparateur

```scss
$liste-espace: 1 2 3;
$avec-virgule: list.append($liste-espace, 4, comma);
// Résultat: 1, 2, 3, 4

$avec-trait: list.append($liste-espace, 4, slash);
// Résultat: 1 / 2 / 3 / 4
```

### Ajout multiple

```scss
$base: 1 2 3;
$etape1: list.append($base, 4);
$etape2: list.append($etape1, 5);
$etape3: list.append($etape2, 6);
// Résultat: 1 2 3 4 5 6
```

### Utilisation pratique : accumulation de propriétés

```scss
$proprietes-base: "display", "flex";
$nouvelles-proprietes: list.append($proprietes-base, "flex-wrap");
$proprietes-finales: list.append($nouvelles-proprietes, "justify-content");

.flex-container {
  @each $prop in $proprietes-finales {
    @debug $prop;
  }
}
```

### Ajout de variables à une liste

```scss
$prefixes: ();
$classes-generees: ();

@mixin generer-classe($nom) {
  $classes-generees: list.append($classes-generees, $nom);
  .#{$nom} {
    @content;
  }
}

@include generer-classe("btn-primary") {
  background-color: blue;
}

@include generer-classe("btn-secondary") {
  background-color: gray;
}

// $classes-generees contient maintenant "btn-primary" "btn-secondary"
```

---

## 22.3 — `list.index()` : Trouver la position

La fonction `list.index()` retourne la position de la première occurrence d'une valeur dans une liste.

### Syntaxe

```scss
list.index($liste, $valeur)
```

### Exemples de base

```scss
$fruits: "pomme", "banane", "cerise";

$pos-1: list.index($fruits, "banane");   // 2
$pos-2: list.index($fruits, "pomme");   // 1
$pos-3: list.index($fruits, "cerise");  // 3
$pos-4: list.index($fruits, "orange");  // null (non trouvé)
```

### Recherche dans des listes imbriquées

```scss
$liste: (a, b, c), (d, e, f), (g, h, i);

$pos-1: list.index($liste, (a, b, c));  // 1
$pos-2: list.index($liste, (d, e, f));  // 2
$pos-3: list.index($liste, (x, y, z));  // null
```

### Utilisation pratique : vérification d'existence

```scss
$roles-autorises: "admin", "editor", "viewer";
$role-utilisateur: "editor";

@mixin verifier-role($role) {
  $autorise: list.index($roles-autorises, $role);

  @if $autorise {
    @debug "Role '#{$role}' autorisé à la position #{$autorise}";
  } @else {
    @warn "Role '#{$role}' non autorisé";
  }
}

@include verifier-role("admin");
@include verifier-role("superadmin");
```

---

## 22.4 — `list.inspect()` : Inspection d'une liste

La fonction `list.inspect()` retourne une représentation textuelle d'une liste, incluant les séparateurs.

### Syntaxe

```scss
list.inspect($liste)
```

### Exemples

```scss
$liste-virgule: 1, 2, 3;
$liste-espace: 1 2 3;
$liste-vide: ();
$liste-nidifiee: (a, b), (c, d);

$inspect-1: list.inspect($liste-virgule);    // "(1, 2, 3)"
$inspect-2: list.inspect($liste-espace);     // "(1 2 3)"
$inspect-3: list.inspect($liste-vide);       // "()"
$inspect-4: list.inspect($liste-nidifiee);   // "((a, b), (c, d))"
```

### Utilisation pour le débogage

```scss
$ma-liste: "a", "b", "c";

@debug "Contenu de la liste: #{list.inspect($ma-liste)}";
@debug "Séparateur: #{list.list-separator($ma-liste)}";
@debug "Longueur: #{list.length($ma-liste)}";
```

---

## 22.5 — `list.is-bracketed()` : Vérification des crochets

La fonction `list.is-bracketed()` vérifie si une liste est encadrée par des crochets.

### Syntaxe

```scss
list.is-bracketed($liste)
```

### Exemples

```scss
$liste-sans-crochets: 1, 2, 3;
$liste-avec-crochets: [1, 2, 3];

$is-1: list.is-bracketed($liste-sans-crochets);  // false
$is-2: list.is-bracketed($liste-avec-crochets);   // true
```

### Utilisation pratique

```scss
$marge: [10px, 20px, 30px, 40px];

@mixin appliquer-marges($marges) {
  @if list.is-bracketed($marges) {
    // Liste avec crochets — extraire les valeurs
    margin-top: nth($marges, 1);
    margin-right: nth($marges, 2);
    margin-bottom: nth($marges, 3);
    margin-left: nth($marges, 4);
  } @else {
    // Liste sans crochets — appliquer à toutes les côtés
    margin: $marges;
  }
}

.bloc-1 {
  @include appliquer-marges($marge);
}

.bloc-2 {
  @include appliquer-marges(10px);
}
```

---

## 22.6 — `list.join()` : Joindre des listes

La fonction `list.join()` combine deux listes en une seule.

### Syntaxe

```scss
list.join($liste1, $liste2, $bracketed: null, $separator: null)
```

### Exemples de base

```scss
$liste1: 1, 2, 3;
$liste2: 4, 5, 6;

$jointe: list.join($liste1, $liste2);
// Résultat: 1, 2, 3, 4, 5, 6
```

### Changement de séparateur

```scss
$liste1: 1 2 3;
$liste2: 4 5 6;

$avec-virgule: list.join($liste1, $liste2, comma);
// Résultat: 1, 2, 3, 4, 5, 6

$avec-espace: list.join($liste1, $liste2, space);
// Résultat: 1 2 3 4 5 6
```

### Listes vides

```scss
$vide1: ();
$vide2: ();
$base: 1, 2, 3;

$join1: list.join($vide1, $base);    // 1, 2, 3
$join2: list.join($base, $vide2);    // 1, 2, 3
$join3: list.join($vide1, $vide2);   // ()
```

### Utilisation pratique : fusion de grilles

```scss
$colonnes-mobile: 1fr;
$colonnes-tablette: 1fr 1fr;
$colonnes-desktop: 1fr 1fr 1fr;

$colonnes-tablet-fusionnees: list.join($colonnes-tablette, $colonnes-desktop);
// "1fr 1fr" "1fr 1fr 1fr" (liste de listes)
```

---

## 22.7 — `list.length()` : Longueur d'une liste

La fonction `list.length()` retourne le nombre d'éléments dans une liste.

### Syntaxe

```scss
list.length($liste)
```

### Exemples

```scss
$longueur-1: list.length(1, 2, 3);        // 3
$longueur-2: list.length((a, b, c, d));   // 4
$longueur-3: list.length(());             // 0
$longueur-4: list.length(single);          // 1
$longueur-5: list.length(1);              // 1
```

### Utilisation pratique : itération conditionnelle

```scss
$padding-options: 5px, 10px, 15px, 20px;

@for $i from 1 through list.length($padding-options) {
  .padding-#{$i} {
    padding: nth($padding-options, $i);
  }
}

// Vérification avant itération
$options: ();

@if list.length($options) > 0 {
  @each $opt in $options {
    .option {
      value: $opt;
    }
  }
} @else {
  @debug "Aucune option définie";
}
```

---

## 22.8 — `list.list-separator()` : Type de séparateur

La fonction `list.list-separator()` retourne le séparateur utilisé dans une liste.

### Syntaxe

```scss
list.list-separator($liste)
```

### Exemples

```scss
$sep-1: list.list-separator(1, 2, 3);   // comma
$sep-2: list.list-separator(1 2 3);     // space
$sep-3: list.list-separator(());         // space (vide)
$sep-4: list.list-separator((a, b));     // comma
```

### Utilisation pratique : adaptabilité

```scss
$gouttiere: 20px 30px 40px 50px;

@mixin appliquer-gouttiere($g) {
  $sep: list.list-separator($g);

  @if $sep == comma {
    // Liste avec virgules — padding individuel
    padding-top: nth($g, 1);
    padding-right: nth($g, 2);
    padding-bottom: nth($g, 3);
    padding-left: nth($g, 4);
  } @else {
    // Liste avec espaces — padding uniforme
    padding: $g;
  }
}

.bloc {
  @include appliquer-gouttiere($gouttiere);
}
```

---

## 22.9 — `list.nth()` : Accéder à un élément

La fonction `list.nth()` retourne l'élèment à une position donnée dans une liste.

### Syntaxe

```scss
list.nth($liste, $position)
```

### Exemples

```scss
$liste: "a", "b", "c", "d", "e";

$elem-1: list.nth($liste, 1);   // "a"
$elem-3: list.nth($liste, 3);   // "c"
$elem-5: list.nth($liste, 5);   // "e"
$elem-neg: list.nth($liste, -1); // "e" (dernier élément)
```

### Listes imbriquées

```scss
$matrice: (1, 2), (3, 4), (5, 6);

$ligne-1: list.nth($matrice, 1);  // (1, 2)
$ligne-2: list.nth($matrice, 2);  // (3, 4)
$elem-1-1: nth(nth($matrice, 1), 1); // 1
$elem-2-2: nth(nth($matrice, 2), 2); // 4
```

### Utilisation pratique : extraction de tokens

```scss
$tokens: (
  "couleurs": ("primaire", "#3498db"),
  "tailles": ("base", "16px"),
  "espacements": ("marge", "20px")
);

@each $token in $tokens {
  $nom: nth($token, 1);
  $valeur: nth($token, 2);

  .#{$nom} {
    value: $valeur;
  }
}
```

---

## 22.10 — `list.set-nth()` : Modifier un élément

La fonction `list.set-nth()` retourne une nouvelle liste avec l'élément à la position donnée remplacé.

### Syntaxe

```scss
list.set-nth($liste, $position, $valeur)
```

### Exemples

```scss
$liste: 1, 2, 3, 4, 5;

$nouvelle-liste: list.set-nth($liste, 3, 99);
// Résultat: 1, 2, 99, 4, 5

$nouvelle-liste-2: list.set-nth($liste, 1, "a");
// Résultat: "a", 2, 3, 4, 5

$nouvelle-liste-3: list.set-nth($liste, -1, 100);
// Résultat: 1, 2, 3, 4, 100
```

### Modification de plusieurs éléments

```scss
$liste: 1, 2, 3, 4, 5;

$etape1: list.set-nth($liste, 1, 10);
$etape2: list.set-nth($etape1, 3, 30);
$etape3: list.set-nth($etape2, 5, 50);
// Résultat: 10, 2, 30, 4, 50
```

### Utilisation pratique : configuration

```scss
$config-base: (
  "couleur-primaire": "#3498db",
  "taille-texte": "16px",
  "rayon-bordure": "4px"
);

// Surcharge de certaines valeurs
$config-customisee: list.set-nth(
  $config-base,
  list.index($config-base, "#3498db"),
  "#e74c3c"
);
```

---

## 22.11 — `list.reverse()` : Inverser une liste

La fonction `list.reverse()` retourne une liste inversée.

### Syntaxe

```scss
list.reverse($liste, $bracketed: null)
```

### Exemples

```scss
$liste: 1, 2, 3, 4, 5;

$inverse: list.reverse($liste);
// Résultat: 5, 4, 3, 2, 1

// La liste originale n'est pas modifiée
@debug $liste; // 1, 2, 3, 4, 5
```

### Utilisation pratique

```scss
$etapes: "etape1", "etape2", "etape3", "etape4";

// Afficher les étapes dans l'ordre inverse
@each $etape in list.reverse($etapes) {
  .timeline-#{$etape} {
    display: block;
  }
}
```

---

## 22.12 — `list.zip()` : Combiner des listes

La fonction `list.zip()` combine plusieurs listes en une liste de listes, en groupant les éléments par position.

### Syntaxe

```scss
list.zip($liste1, $liste2, ..., $listeN)
```

### Exemples

```scss
$noms: "Alice", "Bob", "Charlie";
$ages: 25, 30, 35;
$villes: "Paris", "Lyon", "Marseille";

$combinee: list.zip($noms, $ages, $villes);
// Résultat: ("Alice", 25, "Paris"), ("Bob", 30, "Lyon"), ("Charlie", 35, "Marseille")
```

### Utilisation pratique : tableaux dynamiques

```scss
$prenoms: "Marie", "Pierre", "Sophie";
$professions: "Designer", "Développeur", "Chef de projet";

$personnes: list.zip($prenoms, $professions);

@each $personne in $personnes {
  $prenom: nth($personne, 1);
  $profession: nth($personne, 2);

  .personne-#{$prenom} {
    &::before {
      content: "#{$prenom} — #{$profession}";
    }
  }
}
```

---

## 22.13 — `list.flatten()` : Aplatir une liste

La fonction `list.flatten()` aplatit une liste imbriquée en une seule liste.

### Syntaxe

```scss
list.flatten($liste, $bracketed: null)
```

### Exemples

```scss
$liste-imbriquee: (1, 2), (3, 4), (5, 6);

$aplatie: list.flatten($liste-imbriquee);
// Résultat: 1, 2, 3, 4, 5, 6

$profondement: (1, (2, 3)), ((4, 5), 6);
$aplatie-profonde: list.flatten($profondement, true);
// Résultat: [1, 2, 3, 4, 5, 6]
```

### Utilisation pratique

```scss
$colonnes-desktop: 1fr, 1fr, 1fr;
$colonnes-tablette: 1fr, 1fr;

$colonnes-aplaties: list.flatten(($colonnes-desktop, $colonnes-tablette));
// "1fr, 1fr, 1fr, 1fr, 1fr"
```

---

## 22.14 — Exemples pratiques avancés

### Système de design tokens

```scss
@use 'sass:list';

// Définition des tokens
$spacing-tokens: (
  (xs, 4px),
  (sm, 8px),
  (md, 16px),
  (lg, 24px),
  (xl, 32px),
  (xxl, 48px)
);

$color-tokens: (
  (primary, #3498db),
  (secondary, #2ecc71),
  (danger, #e74c3c),
  (warning, #f39c12)
);

$font-size-tokens: (
  (xs, 12px),
  (sm, 14px),
  (base, 16px),
  (lg, 18px),
  (xl, 24px),
  (xxl, 32px)
);

// Génération automatique des classes
@each $token in $spacing-tokens {
  .spacing-#{nth($token, 1)} {
    padding: nth($token, 2);
  }
}

@each $token in $color-tokens {
  .text-#{nth($token, 1)} {
    color: nth($token, 2);
  }

  .bg-#{nth($token, 1)} {
    background-color: nth($token, 2);
  }
}

@each $token in $font-size-tokens {
  .font-#{nth($token, 1)} {
    font-size: nth($token, 2);
  }
}
```

### Système de grille

```scss
@use 'sass:list';

$breakpoints: (
  (mobile, 576px),
  (tablet, 768px),
  (desktop, 992px),
  (wide, 1200px)
);

$nbre-colonnes: 12;

@mixin generer-grille($bp-name, $bp-width) {
  @media (min-width: $bp-width) {
    @for $i from 1 through $nbre-colonnes {
      .col-#{$bp-name}-#{$i} {
        width: percentage($i / $nbre-colonnes);
      }
    }
  }
}

@each $breakpoint in $breakpoints {
  @include generer-grille(nth($breakpoint, 1), nth($breakpoint, 2));
}
```

### Système de thème

```scss
@use 'sass:list';

$theme-clair: (
  (bg, #ffffff),
  (text, #333333),
  (primary, #3498db),
  (border, #dee2e6)
);

$theme-sombre: (
  (bg, #1a1a2e),
  (text, #e0e0e0),
  (primary, #6c5ce7),
  (border, #2d2d44)
);

@mixin appliquer-theme($theme) {
  @each $token in $theme {
    $propriete: nth($token, 1);
    $valeur: nth($token, 2);

    --color-#{$propriete}: #{$valeur};
  }
}

body {
  @include appliquer-theme($theme-clair);
}

@media (prefers-color-scheme: dark) {
  body {
    @include appliquer-theme($theme-sombre);
  }
}
```

### Système d'icônes

```scss
@use 'sass:list';

$icons: (
  (home, "\f015"),
  (user, "\f007"),
  (settings, "\f013"),
  (heart, "\f004"),
  (star, "\f005")
);

@each $icon in $icons {
  .icon-#{nth($icon, 1)}::before {
    content: unquote(nto#{nth($icon, 2)}t);
    font-family: "Font Awesome 5 Free";
    font-weight: 900;
  }
}
```

---

## 22.15 — Exercices

### Exercice 1 : Gestionnaire de listes

Créez un système de gestion de listes avec les opérations suivantes : ajouter, supprimer, rechercher, inverser, trier.

### Exercice 2 : Système de pagination

Créez un système de pagination qui génère automatiquement les numéros de page en utilisant les fonctions de listes.

### Exercice 3 : Configuration de thème

Créez un système de configuration de thème qui permet de fusionner des configurations par défaut avec des personnalisations.

### Exercice 4 : Générateur de classes

Créez un générateur de classes utilitaires qui prend une liste de propriétés et de valeurs et génère les classes correspondantes.

### Exercice 5 : Tableau de bord

Créez un système de widgets pour un tableau de bord, en utilisant les listes pour définir la disposition et le contenu.

---

## 22.16 — Résumé

| Fonction | Description | Exemple |
|----------|-------------|---------|
| `list.append()` | Ajouter un élément | `list.append($l, 4)` |
| `list.index()` | Trouver la position | `list.index($l, "a")` |
| `list.inspect()` | Inspection d'une liste | `list.inspect($l)` |
| `list.is-bracketed()` | Vérification des crochets | `list.is-bracketed($l)` |
| `list.join()` | Joindre des listes | `list.join($l1, $l2)` |
| `list.length()` | Longueur d'une liste | `list.length($l)` |
| `list.list-separator()` | Type de séparateur | `list.list-separator($l)` |
| `list.nth()` | Accéder à un élément | `list.nth($l, 1)` |
| `list.set-nth()` | Modifier un élément | `list.set-nth($l, 1, "x")` |
| `list.reverse()` | Inverser une liste | `list.reverse($l)` |
| `list.zip()` | Combiner des listes | `list.zip($l1, $l2)` |
| `list.flatten()` | Aplatir une liste | `list.flatten($l)` |

---

**Prochain chapitre :** Nous explorerons le module `sass:map` pour la manipulation des maps (dictionnaires).
