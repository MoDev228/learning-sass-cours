# Chapitre 15 : Les Listes

## Introduction

Les **listes** sont l'une des trois structures de données principales de Sass (avec les maps et les nombres). Une liste est un **ensemble ordonné de valeurs** séparées par des espaces ou des virgules. Elles sont essentielles pour manipuler des collections de données, itérer sur des ensembles de valeurs, et créer des systèmes de design dynamiques.

Pensez aux listes Sass comme à des **tableaux** dans d'autres langages de programmation : elles stockent plusieurs valeurs sous un seul nom de variable et permettent d'accéder, de modifier et d'itérer sur ces valeurs.

---

## 1. Créer des listes

### Listes séparées par des espaces

```scss
$couleurs: #3498db #2ecc71 #e74c3c #f39c12;
$tailles: 1rem 1.25rem 1.5rem 2rem;
$polices: "Roboto", "Helvetica", "Arial";
```

### Listes séparées par des virgules

```scss
$couleurs-virgule: #3498db, #2ecc71, #e74c3c, #f39c12;
$dimensions: 100%, 200px, auto;
```

### Liste à un seul élément

```scss
$single: (10px);  // Attention : les parenthèses créent une liste
$single2: 10px;   // Ceci est un nombre, pas une liste
```

### Différence entre liste avec espaces et avec virgules

```scss
$avec-espaces: 10px 20px 30px;     // 3 éléments : 10px, 20px, 30px
$avec-virgules: 10px, 20px, 30px;  // 3 éléments : 10px, 20px, 30px
$avec-parentheses: (10px 20px 30px); // 3 éléments : 10px, 20px, 30px
$avec-parentheses-virgules: (10px, 20px, 30px); // 3 éléments : 10px, 20px, 30px
```

### Listes imbriquées

```scss
$matrice: (
  (10px, 20px),
  (30px, 40px),
  (50px, 60px)
);
// Contient 3 listes, chacune contenant 2 valeurs
```

### Listes avec des types différents

```scss
$mixte: 10px "hello" #3498db true;  // Nombre, chaîne, couleur, booléen
```

---

## 2. Accéder aux éléments avec `nth()`

La fonction `nth()` permet d'accéder à un élément spécifique d'une liste. **L'index commence à 1** (contrairement à beaucoup d'autres langages qui commencent à 0).

### Syntaxe

```scss
nth($liste, $index)
```

### Exemples

```scss
$couleurs: #3498db #2ecc71 #e74c3c #f39c12;

$bleu: nth($couleurs, 1);    // #3498db
$vert: nth($couleurs, 2);    // #2ecc71
$rouge: nth($couleurs, 3);   // #e74c3c
$orange: nth($couleurs, 4);  // #f39c12
```

### Utilisation dans un sélecteur

```scss
$colonnes: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12;

@each $col in $colonnes {
  .col-#{$col} {
    width: percentage($col / 12);
    float: left;
    padding: 0 0.5rem;
  }
}
```

### Accéder aux listes imbriquées

```scss
$points: (
  (0, 0),
  (100, 50),
  (200, 100)
);

$x1: nth(nth($points, 1), 1); // 0
$y1: nth(nth($points, 1), 2); // 0
$x2: nth(nth($points, 2), 1); // 100
$y2: nth(nth($points, 2), 2); // 50
```

---

## 3. Itérer avec `@each`

La boucle `@each` est la façon la plus courante de parcourir une liste Sass.

### Syntaxe de base

```scss
@each $element in $liste {
  // styles avec $element
}
```

### Exemple : classes de couleur

```scss
$couleurs: (
  primaire: #3498db,
  secondaire: #95a5a6,
  succes: #27ae60,
  danger: #e74c3c,
  avertissement: #f39c12,
  info: #17a2b8
);

@each $nom, $couleur in $couleurs {
  .texte-#{$nom} {
    color: $couleur;
  }
  .fond-#{$nom} {
    background-color: $couleur;
  }
  .bordure-#{$nom} {
    border-color: $couleur;
  }
}
```

**CSS généré :**

```css
.texte-primaire { color: #3498db; }
.fond-primaire { background-color: #3498db; }
.bordure-primaire { border-color: #3498db; }

.texte-secondaire { color: #95a5a6; }
.fond-secondaire { background-color: #95a5a6; }
.bordure-secondaire { border-color: #95a5a6; }
/* ... etc */
```

### Exemple : espacement

```scss
$niveaux: 1, 2, 3, 4, 5, 6, 7, 8;

@each $niveau in $niveaux {
  .mt-#{$niveau} { margin-top: $niveau * 0.5rem; }
  .mb-#{$niveau} { margin-bottom: $niveau * 0.5rem; }
  .ml-#{$niveau} { margin-left: $niveau * 0.5rem; }
  .mr-#{$niveau} { margin-right: $niveau * 0.5rem; }
  .pt-#{$niveau} { padding-top: $niveau * 0.5rem; }
  .pb-#{$niveau} { padding-bottom: $niveau * 0.5rem; }
  .pl-#{$niveau} { padding-left: $niveau * 0.5rem; }
  .pr-#{$niveau} { padding-right: $niveau * 0.5rem; }
}
```

### Exemple : grilles de colonnes

```scss
$colonnes: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12;

@each $col in $colonnes {
  .col-#{$col} {
    width: percentage($col / 12);
  }

  .offset-#{$col} {
    margin-left: percentage($col / 12);
  }
}
```

---

## 4. Fonctions sur les listes

### `length()` — Nombre d'éléments

```scss
$liste: 10px 20px 30px 40px;
length($liste);  // 4

$vide: ();
length($vide);   // 0
```

**Exemple d'utilisation :**

```scss
$breakpoints: 576px, 768px, 992px, 1200px;

@for $i from 1 through length($breakpoints) {
  @media (min-width: nth($breakpoints, $i)) {
    .container {
      max-width: nth($breakpoints, $i);
    }
  }
}
```

### `append()` — Ajouter un élément

```scss
$liste: 10px 20px 30px;
$resultat: append($liste, 40px);
// Résultat : 10px 20px 30px 40px
```

**Option de séparation :**

```scss
$liste: 10px 20px 30px;

// Avec espace (par défaut)
$resultat1: append($liste, 40px);
// 10px 20px 30px 40px

// Avec virgule
$resultat2: append($liste, 40px, comma);
// 10px, 20px, 30px, 40px
```

**Exemple pratique :**

```scss
$polices-base: "Helvetica";
$polices: append($polices-base, "Arial", comma);
$polices: append($polices, "sans-serif", comma);
// Résultat : "Helvetica", "Arial", sans-serif

body {
  font-family: $polices;
}
```

### `index()` — Trouver la position d'un élément

```scss
$liste: rouge, vert, bleu, jaune;
index($liste, vert);  // 2
index($liste, bleu);  // 3
index($liste, rose);  // null (non trouvé)
```

**Exemple :**

```scss
$alertes: info, succes, avertissement, danger;

@each $type in $alertes {
  .alerte-#{$type} {
    @if index($alertes, $type) == 1 {
      border-left: 4px solid #17a2b8;
    } @else if index($alertes, $type) == 2 {
      border-left: 4px solid #28a745;
    } @else if index($alertes, $type) == 3 {
      border-left: 4px solid #ffc107;
    } @else if index($alertes, $type) == 4 {
      border-left: 4px solid #dc3545;
    }
  }
}
```

### `join()` — Fusionner deux listes

```scss
$liste1: 10px 20px;
$liste2: 30px 40px;

$resultat: join($liste1, $liste2);
// 10px 20px 30px 40px
```

**Avec séparation :**

```scss
$liste1: a, b, c;
$liste2: d, e, f;

// Avec espace
join($liste1, $liste2, space);
// a b c d e f

// Avec virgule
join($liste1, $liste2, comma);
// a, b, c, d, e, f
```

### `zip()` — Combiner des listes

```scss
$noms: alice, bob, charlie;
$ages: 25, 30, 35;
$villes: paris, lyon, marseille;

$combinaison: zip($noms, $ages, $villes);
// (alice 25 paris), (bob 30 lyon), (charlie 35 marseille)
```

### `reverse()` — Inverser une liste

```scss
$liste: 1, 2, 3, 4, 5;
$resultat: reverse($liste);
// 5, 4, 3, 2, 1
```

### `flatten()` — Aplatir les listes imbriquées

```scss
$matrice: (1, 2), (3, 4), (5, 6);
$resultat: flatten($matrice);
// 1, 2, 3, 4, 5, 6
```

---

## 5. Manipulation des listes

### Vérifier si un élément existe

```scss
$liste: rouge, vert, bleu;

@if index($liste, vert) {
  .element {
    color: green;
  }
}
```

### Supprimer un élément

```scss
@function supprimer-element($liste, $valeur) {
  $resultat: ();
  @each $item in $liste {
    @if $item != $valeur {
      $resultat: append($resultat, $item);
    }
  }
  @return $resultat;
}

$couleurs: rouge, vert, bleu, jaune;
$nouveaux: supprimer-element($couleurs, vert);
// rouge, bleu, jaune
```

### Vérifier si une liste est vide

```scss
$liste: ();

@if length($liste) == 0 {
  .element {
    display: none;
  }
}
```

### Trier une liste

```scss
@function trier($liste) {
  $resultat: $liste;
  $longueur: length($resultat);

  @for $i from 1 through $longueur - 1 {
    @for $j from $i + 1 through $longueur {
      @if nth($resultat, $i) > nth($resultat, $j) {
        $temp: nth($resultat, $i);
        $resultat: set-nth($resultat, $i, nth($resultat, $j));
        $resultat: set-nth($resultat, $j, $temp);
      }
    }
  }

  @return $resultat;
}

$nombres: 5, 2, 8, 1, 9, 3;
$tries: trier($nombres);
// 1, 2, 3, 5, 8, 9
```

---

## 6. Exemples pratiques

### 6.1 Système de polices

```scss
$familles-polices: (
  "serif": ("Georgia", "Times New Roman", serif),
  "sans-serif": ("Helvetica", "Arial", sans-serif),
  "monospace": ("Courier New", monospace)
);

@each $nom, $familles in $familles-polices {
  .font-#{$nom} {
    font-family: $familles;
  }
}
```

### 6.2 Système d'espacement

```scss
$espace-base: 4px;
$niveaux-espacement: 0, 1, 2, 3, 4, 5, 6, 8, 10, 12, 16, 20;

@each $niveau in $niveaux-espacement {
  @if $niveau == 0 {
    .m-0 { margin: 0 !important; }
    .p-0 { padding: 0 !important; }
  } @else {
    $valeur: $espace-base * $niveau;
    .m-#{$niveau} { margin: $valeur !important; }
    .mt-#{$niveau} { margin-top: $valeur !important; }
    .mb-#{$niveau} { margin-bottom: $valeur !important; }
    .ml-#{$niveau} { margin-left: $valeur !important; }
    .mr-#{$niveau} { margin-right: $valeur !important; }
    .mx-#{$niveau} { margin-left: $valeur !important; margin-right: $valeur !important; }
    .my-#{$niveau} { margin-top: $valeur !important; margin-bottom: $valeur !important; }
    .p-#{$niveau} { padding: $valeur !important; }
    .pt-#{$niveau} { padding-top: $valeur !important; }
    .pb-#{$niveau} { padding-bottom: $valeur !important; }
    .pl-#{$niveau} { padding-left: $valeur !important; }
    .pr-#{$niveau} { padding-right: $valeur !important; }
    .px-#{$niveau} { padding-left: $valeur !important; padding-right: $valeur !important; }
    .py-#{$niveau} { padding-top: $valeur !important; padding-bottom: $valeur !important; }
  }
}
```

### 6.3 Système de breakpoints

```scss
$breakpoints: (
  "xs": 0,
  "sm": 576px,
  "md": 768px,
  "lg": 992px,
  "xl": 1200px,
  "xxl": 1400px
);

$breakpoints-liste: 576px, 768px, 992px, 1200px, 1400px;

@each $nom, $valeur in $breakpoints {
  @media (min-width: $valeur) {
    .d-#{$nom}-none { display: none !important; }
    .d-#{$nom}-block { display: block !important; }
    .d-#{$nom}-flex { display: flex !important; }
    .d-#{$nom}-grid { display: grid !important; }
    .d-#{$nom}-inline { display: inline !important; }
    .d-#{$nom}-inline-block { display: inline-block !important; }
  }
}
```

### 6.4 Système de typographie

```scss
$niveaux-typographie: (
  1: (2.5rem, 700, 1.2),
  2: (2rem, 700, 1.25),
  3: (1.75rem, 600, 1.3),
  4: (1.5rem, 600, 1.35),
  5: (1.25rem, 600, 1.4),
  6: (1rem, 600, 1.45)
);

@each $niveau, $params in $niveaux-typographie {
  $taille: nth($params, 1);
  $poids: nth($params, 2);
  $hauteur: nth($params, 3);

  h#{$niveau}, .titre-#{$niveau} {
    font-size: $taille;
    font-weight: $poids;
    line-height: $hauteur;
    margin-top: 0;
    margin-bottom: 0.5rem;
  }
}
```

### 6.5 Système de couleurs avec opacités

```scss
$couleurs-base: (
  primaire: #3498db,
  secondaire: #95a5a6,
  succes: #27ae60,
  danger: #e74c3c,
  avertissement: #f39c12
);

$opacites: 10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90%;

@each $nom, $couleur in $couleurs-base {
  @each $opacite in $opacites {
    $alpha: $opacite / 100;
    .#{$nom}-#{$opacite} {
      color: rgba($couleur, $alpha);
    }
  }
}
```

---

## 7. Exercices

### Exercice 1 : Manipulation de base

Créez une liste `$animaux: chien, chat, oiseau, poisson, lapin` et :
1. Affichez le nombre d'éléments
2. Accédez au 3ème élément
3. Ajoutez "tortue" à la fin
4. Trouvez la position de "oiseau"

```scss
// Votre réponse ici
```

### Exercice 2 : Génération de classes de taille

Avec la liste `$tailles: xs, sm, md, lg, xl, xxl`, générez des classes :
- `.texte-xs`, `.texte-sm`, etc. avec des tailles croissantes
- `.fond-xs`, `.fond-sm`, etc. avec des padding croissants

```scss
// Votre réponse ici
```

### Exercice 3 : Système de grille responsive

Créez un système de grille avec 12 colonnes en utilisant `@each` et `nth()`.

```scss
// Votre réponse ici
```

### Exercice 4 : Bibliothèque de couleurs

Créez une bibliothèque de couleurs avec des variations de 5 couleurs de base, chacune avec 10 niveaux de luminosité (10% à 100%).

```scss
$couleurs-base: (
  rouge: #e74c3c,
  bleu: #3498db,
  vert: #27ae60,
  orange: #f39c12,
  violet: #9b59b6
);

// Votre réponse ici
```

### Exercice 5 : Système d'alertes

Avec la liste `$types-alertes: info, succes, avertissement, danger`, créez un système d'alertes complet :
- Chaque type a sa propre couleur
- Chaque type a une icône (via content)
- Chaque type a un style de bordure
- Utilisez `@each` et `index()` pour générer les classes

```scss
// Votre réponse ici
```

---

## Résumé

| Fonction | Description | Exemple |
|---|---|---|
| `nth($liste, $n)` | Accède au n-ème élément | `nth(10px 20px, 1)` → `10px` |
| `length($liste)` | Nombre d'éléments | `length(10px 20px 30px)` → `3` |
| `append($liste, $val)` | Ajoute un élément | `append(10px, 20px)` → `10px 20px` |
| `index($liste, $val)` | Position d'un élément | `index(a b c, b)` → `2` |
| `join($l1, $l2)` | Fusionne deux listes | `join(10px, 20px)` → `10px 20px` |
| `zip($l1, $l2)` | Combine des listes | `zip(a b, 1 2)` → `(a 1) (b 2)` |
| `reverse($liste)` | Inverse une liste | `reverse(1 2 3)` → `3 2 1` |
| `@each $x in $liste` | Itère sur les éléments | Boucle sur chaque élément |

---

## Prochaine étape

Au chapitre suivant, nous verrons les **Maps** (dictionnaires), une structure de données qui associe des **clés** à des **valeurs**, idéale pour créer des systèmes de design configurables et maintenables.
