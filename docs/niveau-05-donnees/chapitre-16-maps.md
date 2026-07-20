# Chapitre 16 : Les Maps

## Introduction

Les **maps** (ou dictionnaires / tableaux associatifs) sont l'une des structures de données les plus puissantes de Sass. Une map associe des **clés** (noms) à des **valeurs** (propriétés), exactement comme un objet en JavaScript ou un dictionnaire en Python.

Les maps sont essentielles pour créer des **systèmes de design configurables** : au lieu de définir des dizaines de variables séparées, vous pouvez regrouper toutes les couleurs, espacements, tailles, etc. dans une map unique et les manipuler de manière programmatique.

```scss
// Sans map (répétitif)
$couleur-primaire: #3498db;
$couleur-secondaire: #2ecc71;
$couleur-danger: #e74c3c;
$couleur-warning: #f39c12;

// Avec map (organisé et manipulable)
$couleurs: (
  primaire: #3498db,
  secondaire: #2ecc71,
  danger: #e74c3c,
  warning: #f39c12
);
```

---

## 1. Créer des maps

### Syntaxe de base

```scss
$nom-de-la-map: (
  cle1: valeur1,
  cle2: valeur2,
  cle3: valeur3
);
```

### Exemple : couleurs

```scss
$couleurs: (
  primaire: #3498db,
  secondaire: #95a5a6,
  succes: #27ae60,
  danger: #e74c3c,
  avertissement: #f39c12,
  info: #17a2b8,
  sombre: #2c3e50,
  clair: #ecf0f1
);
```

### Exemple : espacement

```scss
$espacement: (
  xs: 0.25rem,
  sm: 0.5rem,
  md: 1rem,
  lg: 1.5rem,
  xl: 2rem,
  xxl: 3rem,
  xxxl: 4rem
);
```

### Exemple : breakpoints

```scss
$breakpoints: (
  mobile: 576px,
  tablette: 768px,
  desktop: 992px,
  large: 1200px,
  xlarge: 1400px
);
```

### Exemple : ombres

```scss
$ombres: (
  none: none,
  legere: 0 1px 3px rgba(0, 0, 0, 0.12),
  moyenne: 0 4px 6px rgba(0, 0, 0, 0.15),
  forte: 0 10px 20px rgba(0, 0, 0, 0.15),
  elevee: 0 15px 25px rgba(0, 0, 0, 0.15),
  dramatique: 0 20px 40px rgba(0, 0, 0, 0.2)
);
```

### Exemple : polices

```scss
$polices: (
  famille-primaire: "Helvetica Neue", Helvetica, Arial, sans-serif,
  famille-secondaire: Georgia, "Times New Roman", Times, serif,
  famille-mono: "SFMono-Regular", Consolas, Menlo, monospace,
  poids-light: 300,
  poids-normal: 400,
  poids-medium: 500,
  poids-semi-bold: 600,
  poids-bold: 700
);
```

### Map vide

```scss
$vide: ();
```

### Map imbriquée

```scss
$theme: (
  clair: (
    fond: #ffffff,
    texte: #333333,
    primaire: #3498db,
    secondaire: #95a5a6,
    bordure: #dee2e6
  ),
  sombre: (
    fond: #1a1a2e,
    texte: #e0e0e0,
    primaire: #4da6ff,
    secondaire: #7f8c8d,
    bordure: #2c3e50
  )
);
```

### Map avec différents types de valeurs

```scss
$configuration: (
  nom: "Mon Site",
  version: 2.5,
  responsive: true,
  max-width: 1200px,
  colonnes: 12,
  gap: 1rem,
  border-radius: 8px
);
```

---

## 2. Lire des valeurs avec `map-get()`

La fonction `map-get()` permet de récupérer la valeur associée à une clé.

### Syntaxe

```scss
map-get($map, $cle)
```

### Exemples

```scss
$couleurs: (
  primaire: #3498db,
  secondaire: #2ecc71,
  danger: #e74c3c
);

.bouton-primaire {
  background-color: map-get($couleurs, primaire);
  // background-color: #3498db;
}

.bouton-success {
  background-color: map-get($couleurs, secondaire);
  // background-color: #2ecc71;
}

.bouton-danger {
  background-color: map-get($couleurs, danger);
  // background-color: #e74c3c;
}
```

### Vérifier si une clé existe avec `map-has-key()`

```scss
$couleurs: (
  primaire: #3498db,
  secondaire: #2ecc71
);

@warn map-has-key($couleurs, primaire);    // true
@warn map-has-key($couleurs, danger);      // false
```

**Exemple pratique :**

```scss
@mixin couleur-sure($map, $cle, $fallback: #000000) {
  @if map-has-key($map, $cle) {
    color: map-get($map, $cle);
  } @else {
    @warn "La cle '#{$cle}' n'existe pas dans la map.";
    color: $fallback;
  }
}
```

### Lire dans des maps imbriquées

```scss
$theme: (
  clair: (
    fond: #ffffff,
    texte: #333333
  ),
  sombre: (
    fond: #1a1a2e,
    texte: #e0e0e0
  )
);

$theme-clair: map-get($theme, clair);
$fond-clair: map-get($theme-clair, fond);
// #ffffff
```

### Utiliser `map-get` avec une fonction de fallback

```scss
$ombres: (
  legere: 0 1px 3px rgba(0, 0, 0, 0.12),
  moyenne: 0 4px 6px rgba(0, 0, 0, 0.15)
);

@mixin ombre($niveau: moyenne) {
  $valeur: map-get($ombres, $niveau);

  @if $valeur {
    box-shadow: $valeur;
  } @else {
    @warn "Niveau d'ombre '#{$niveau}' non trouve.";
    box-shadow: none;
  }
}

.carte {
  @include ombre(legere);
}

.carte-elevee {
  @include ombre(moyenne);
}
```

---

## 3. Modifier des maps avec `map-merge()`

La fonction `map-merge()` permet de fusionner deux maps ou d'ajouter de nouvelles cles-valeurs.

### Syntaxe

```scss
map-merge($map1, $map2)
```

Si des cles existent dans les deux maps, les valeurs de `$map2` ecrasent celles de `$map1`.

### Exemple

```scss
$couleurs-base: (
  primaire: #3498db,
  secondaire: #95a5a6,
  succes: #27ae60
);

$couleurs-customisees: (
  primaire: #8e44ad,
  danger: #e74c3c
);

$couleurs-finales: map-merge($couleurs-base, $couleurs-customisees);
// Resultat :
// (
//   primaire: #8e44ad,   <- ecrase par la map2
//   secondaire: #95a5a6,
//   succes: #27ae60,
//   danger: #e74c3c      <- ajoute
// )
```

### Chaîner les merges

```scss
$theme-base: (
  fond: #ffffff,
  texte: #333333
);

$theme-secondaire: (
  primaire: #3498db,
  secondaire: #95a5a6
);

$theme-succes: (
  succes: #27ae60,
  danger: #e74c3c
);

$theme-final: map-merge(
  map-merge($theme-base, $theme-secondaire),
  $theme-succes
);
```

### Utilisation dans un systeme de design

```scss
$design-system: (
  espace-base: 1rem,
  rayon-base: 4px,
  transition-base: 0.3s ease
);

$projet-config: (
  espace-base: 1.5rem,
  rayon-base: 8px,
  police-base: "Poppins"
);

$config: map-merge($design-system, $projet-config);

.container {
  padding: map-get($config, espace-base);
  border-radius: map-get($config, rayon-base);
}
```

---

## 4. Iterer sur les maps

### `@each` avec les maps

Lorsqu'on itere sur une map, on recoit **deux variables** : la cle et la valeur.

```scss
@each $cle, $valeur in $map {
  // $cle est la cle
  // $valeur est la valeur
}
```

### Exemple : classes de couleur

```scss
$couleurs: (
  primaire: #3498db,
  secondaire: #95a5a6,
  succes: #27ae60,
  danger: #e74c3c,
  avertissement: #f39c12
);

@each $nom, $couleur in $couleurs {
  .texte-#{$nom} {
    color: $couleur;
  }

  .fond-#{$nom} {
    background-color: $couleur;
  }

  .bordure-#{$nom} {
    border: 2px solid $couleur;
  }

  .ombre-#{$nom} {
    box-shadow: 0 4px 12px rgba($couleur, 0.3);
  }
}
```

### Exemple : ombres personnalisees

```scss
$ombres: (
  none: none,
  legere: 0 1px 3px rgba(0, 0, 0, 0.12),
  moyenne: 0 4px 6px rgba(0, 0, 0, 0.15),
  forte: 0 10px 20px rgba(0, 0, 0, 0.15)
);

@each $nom, $valeur in $ombres {
  .ombre-#{$nom} {
    box-shadow: $valeur;
  }
}
```

### Exemple : espacement

```scss
$espacement: (
  0: 0,
  xs: 0.25rem,
  sm: 0.5rem,
  md: 1rem,
  lg: 1.5rem,
  xl: 2rem,
  xxl: 3rem
);

@each $nom, $valeur in $espacement {
  .m-#{$nom} { margin: $valeur; }
  .p-#{$nom} { padding: $valeur; }
  .mt-#{$nom} { margin-top: $valeur; }
  .mb-#{$nom} { margin-bottom: $valeur; }
  .ml-#{$nom} { margin-left: $valeur; }
  .mr-#{$nom} { margin-right: $valeur; }
  .pt-#{$nom} { padding-top: $valeur; }
  .pb-#{$nom} { padding-bottom: $valeur; }
  .pl-#{$nom} { padding-left: $valeur; }
  .pr-#{$nom} { padding-right: $valeur; }
}
```

---

## 5. `map-keys()` et `map-values()`

### `map-keys()` — Extraire les cles

```scss
$couleurs: (
  primaire: #3498db,
  secondaire: #2ecc71,
  danger: #e74c3c
);

$cles: map-keys($couleurs);
// (primaire, secondaire, danger)
```

**Utilisation pratique :**

```scss
@if index(map-keys($couleurs), danger) {
  .alerte-danger {
    background-color: map-get($couleurs, danger);
  }
}
```

### `map-values()` — Extraire les valeurs

```scss
$couleurs: (
  primaire: #3498db,
  secondaire: #2ecc71,
  danger: #e74c3c
);

$valeurs: map-values($couleurs);
// (#3498db, #2ecc71, #e74c3c)
```

### Exemple : generer un selecteur pour chaque valeur

```scss
$tailles: (
  xs: 12px,
  sm: 14px,
  md: 16px,
  lg: 20px,
  xl: 24px
);

@each $nom in map-keys($tailles) {
  .texte-#{$nom} {
    font-size: map-get($tailles, $nom);
  }
}
```

---

## 6. Fonctions utilitaires pour les maps

### `map-remove()` — Supprimer une cle

```scss
$couleurs: (
  primaire: #3498db,
  secondaire: #2ecc71,
  danger: #e74c3c
);

$sans-danger: map-remove($couleurs, danger);
// (primaire: #3498db, secondaire: #2ecc71)
```

### `map-deep-merge()` — Fusion profonde

```scss
$theme1: (
  bouton: (
    primaire: (
      fond: #3498db,
      texte: #ffffff
    )
  )
);

$theme2: (
  bouton: (
    primaire: (
      fond: #8e44ad
    )
  )
);

// map-merge ecraserait tout le bouton primaire
$resultat-shallow: map-merge($theme1, $theme2);
// bouton: (primaire: (fond: #8e44ad)) <- texte est perdu !

// map-deep-merge fusionne recursivement
$resultat-deep: map-deep-merge($theme1, $theme2);
// bouton: (primaire: (fond: #8e44ad, texte: #ffffff)) <- texte conserve !
```

### Vérifier si la map est vide

```scss
$vide: ();

@if length($vide) == 0 {
  @warn "La map est vide";
}
```

---

## 7. Exemples pratiques avances

### 7.1 Systeme de design complet

```scss
// ==========================================
// CONFIGURATION DU DESIGN SYSTEM
// ==========================================

$couleurs: (
  blanc: #ffffff,
  noir: #000000,
  gris-100: #f7fafc,
  gris-200: #edf2f7,
  gris-300: #e2e8f0,
  gris-400: #cbd5e0,
  gris-500: #a0aec0,
  gris-600: #718096,
  gris-700: #4a5568,
  gris-800: #2d3748,
  gris-900: #1a202c,
  primaire: #3182ce,
  secondaire: #718096,
  succes: #38a169,
  danger: #e53e3e,
  avertissement: #d69e2e,
  info: #3182ce
);

$espacement: (
  0: 0,
  1: 0.25rem,
  2: 0.5rem,
  3: 0.75rem,
  4: 1rem,
  5: 1.25rem,
  6: 1.5rem,
  8: 2rem,
  10: 2.5rem,
  12: 3rem,
  16: 4rem,
  20: 5rem,
  24: 6rem,
  32: 8rem
);

$tailles-police: (
  xs: 0.75rem,
  sm: 0.875rem,
  base: 1rem,
  lg: 1.125rem,
  xl: 1.25rem,
  "2xl": 1.5rem,
  "3xl": 1.875rem,
  "4xl": 2.25rem
);

$poids-police: (
  light: 300,
  normal: 400,
  medium: 500,
  semi-bold: 600,
  bold: 700,
  extra-bold: 800
);

$ombres: (
  none: none,
  sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05),
  base: 0 1px 3px 0 rgba(0, 0, 0, 0.1),
  md: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
  lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
  xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1)
);

$rayons: (
  none: 0,
  sm: 0.125rem,
  base: 0.25rem,
  md: 0.375rem,
  lg: 0.5rem,
  xl: 0.75rem,
  "2xl": 1rem,
  plein: 9999px
);

$transitions: (
  rapide: 150ms ease,
  normale: 300ms ease,
  lente: 500ms ease,
  spring: 500ms cubic-bezier(0.175, 0.885, 0.32, 1.275)
);

// ==========================================
// GENERATION DES UTILITAIRES
// ==========================================

@each $nom, $valeur in $couleurs {
  .texte-#{$nom} {
    color: $valeur;
  }
  .fond-#{$nom} {
    background-color: $valeur;
  }
}

@each $nom, $valeur in $espacement {
  .m-#{$nom} { margin: $valeur; }
  .p-#{$nom} { padding: $valeur; }
  .mt-#{$nom} { margin-top: $valeur; }
  .mb-#{$nom} { margin-bottom: $valeur; }
  .ml-#{$nom} { margin-left: $valeur; }
  .mr-#{$nom} { margin-right: $valeur; }
  .mx-#{$nom} { margin-left: $valeur; margin-right: $valeur; }
  .my-#{$nom} { margin-top: $valeur; margin-bottom: $valeur; }
  .pt-#{$nom} { padding-top: $valeur; }
  .pb-#{$nom} { padding-bottom: $valeur; }
  .pl-#{$nom} { padding-left: $valeur; }
  .pr-#{$nom} { padding-right: $valeur; }
  .px-#{$nom} { padding-left: $valeur; padding-right: $valeur; }
  .py-#{$nom} { padding-top: $valeur; padding-bottom: $valeur; }
}

@each $nom, $valeur in $tailles-police {
  .texte-#{$nom} {
    font-size: $valeur;
  }
}

@each $nom, $valeur in $ombres {
  .ombre-#{$nom} {
    box-shadow: $valeur;
  }
}

@each $nom, $valeur in $rayons {
  .arrondi-#{$nom} {
    border-radius: $valeur;
  }
}
```

### 7.2 Systeme de themes

```scss
$themes: (
  clair: (
    fond-principal: #ffffff,
    fond-secondaire: #f8f9fa,
    fond-tertiaire: #e9ecef,
    texte-primaire: #212529,
    texte-secondaire: #6c757d,
    texte-tertiaire: #adb5bd,
    primaire: #007bff,
    secondaire: #6c757d,
    succes: #28a745,
    danger: #dc3545,
    avertissement: #ffc107,
    info: #17a2b8,
    bordure: #dee2e6,
    ombre: rgba(0, 0, 0, 0.1)
  ),
  sombre: (
    fond-principal: #121212,
    fond-secondaire: #1e1e1e,
    fond-tertiaire: #2d2d2d,
    texte-primaire: #e0e0e0,
    texte-secondaire: #a0a0a0,
    texte-tertiaire: #666666,
    primaire: #4da6ff,
    secondaire: #7f8c8d,
    succes: #2ecc71,
    danger: #e74c3c,
    avertissement: #f39c12,
    info: #3498db,
    bordure: #404040,
    ombre: rgba(0, 0, 0, 0.3)
  )
);

@mixin appliquer-theme($nom-theme) {
  $theme: map-get($themes, $nom-theme);

  @if $theme {
    @each $propriete, $valeur in $theme {
      --#{$propriete}: #{$valeur};
    }
  }
}

@each $nom-theme, $valeurs in $themes {
  .theme-#{$nom-theme} {
    @include appliquer-theme($nom-theme);
  }
}

body {
  background-color: var(--fond-principal);
  color: var(--texte-primaire);
}

.carte {
  background-color: var(--fond-secondaire);
  border: 1px solid var(--bordure);
  box-shadow: 0 4px 6px var(--ombre);
}
```

### 7.3 Systeme responsive avec maps

```scss
$breakpoints: (
  mobile: 576px,
  tablette: 768px,
  desktop: 992px,
  large: 1200px,
  xlarge: 1400px
);

@mixin responsive($nom) {
  $valeur: map-get($breakpoints, $nom);

  @if $valeur {
    @media (max-width: $valeur) {
      @content;
    }
  } @else {
    @warn "Breakpoint '#{$nom}' non trouve.";
  }
}

@mixin min-responsive($nom) {
  $valeur: map-get($breakpoints, $nom);

  @if $valeur {
    @media (min-width: $valeur + 1px) {
      @content;
    }
  }
}

.sidebar {
  width: 250px;

  @include responsive("tablette") {
    width: 100%;
  }
}

.grille {
  display: grid;
  grid-template-columns: repeat(4, 1fr);

  @include responsive("desktop") {
    grid-template-columns: repeat(2, 1fr);
  }

  @include responsive("mobile") {
    grid-template-columns: 1fr;
  }
}
```

### 7.4 Map de configuration de composant

```scss
$button-config: (
  sizes: (
    sm: (
      padding: 6px 12px,
      font-size: 0.875rem,
      border-radius: 4px
    ),
    md: (
      padding: 10px 20px,
      font-size: 1rem,
      border-radius: 6px
    ),
    lg: (
      padding: 14px 28px,
      font-size: 1.125rem,
      border-radius: 8px
    )
  ),
  variants: (
    primaire: (
      bg: #3498db,
      text: #ffffff,
      border: transparent
    ),
    secondaire: (
      bg: #95a5a6,
      text: #ffffff,
      border: transparent
    ),
    outline: (
      bg: transparent,
      text: #3498db,
      border: #3498db
    ),
    ghost: (
      bg: transparent,
      text: #3498db,
      border: transparent
    )
  )
);

@mixin btn($size: md, $variant: primaire) {
  $sizes: map-get($button-config, sizes);
  $size-params: map-get($sizes, $size);
  $variants: map-get($button-config, variants);
  $variant-params: map-get($variants, $variant);

  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  cursor: pointer;
  border-style: solid;
  border-width: 1px;
  transition: all 0.2s ease;

  @if $size-params {
    padding: map-get($size-params, padding);
    font-size: map-get($size-params, font-size);
    border-radius: map-get($size-params, border-radius);
  }

  @if $variant-params {
    background-color: map-get($variant-params, bg);
    color: map-get($variant-params, text);
    border-color: map-get($variant-params, border);
  }

  &:hover {
    opacity: 0.9;
    transform: translateY(-1px);
  }

  &:active {
    transform: translateY(0);
  }
}

.btn-sm-primaire { @include btn(sm, primaire); }
.btn-md-primaire { @include btn(md, primaire); }
.btn-lg-primaire { @include btn(lg, primaire); }
.btn-md-outline { @include btn(md, outline); }
.btn-md-ghost { @include btn(md, ghost); }
```

---

## 8. Exercices

### Exercice 1 : Creer et lire une map

Creez une map `$animaux` avec les cles-valeurs suivantes :
- chien: "Canis lupus familiaris"
- chat: "Felis catus"
- oiseau: "Aves"
- poisson: "Osteichthyes"

Puis affichez le nom scientifique du chat.

```scss
// Votre reponse ici
```

### Exercice 2 : Iteration sur une map

Avec la map `$tailles` definie ci-dessous, generer des classes `.texte-{nom}` avec la bonne taille de police :

```scss
$tailles: (
  xs: 12px,
  sm: 14px,
  md: 16px,
  lg: 20px,
  xl: 24px,
  "2xl": 30px,
  "3xl": 36px
);

// Votre reponse ici
```

### Exercice 3 : Fusion de maps

Creez trois maps de configuration et fusionnez-les en une seule map `$config` :

```scss
$base: (espace: 1rem, rayon: 4px);
$override: (espace: 1.5rem, police: "Poppins");
$supplementaire: (transition: 0.3s ease, ombre: 0 2px 4px rgba(0,0,0,0.1));

// Votre reponse ici
```

### Exercice 4 : Systeme de themes

Creez un systeme de themes avec deux themes (clair et sombre) qui genere des variables CSS customisable.

```scss
// Votre reponse ici
```

### Exercice 5 : Map imbriquee pour un composant

Creez une map `$alerts` contenant les configurations pour 4 types d'alertes (info, succes, warning, danger), chacune avec : `bg`, `text`, `border`, `icon`. Generer les classes correspondantes.

```scss
// Votre reponse ici
```

---

## Resume

| Fonction | Description | Exemple |
|---|---|---|
| `map-get($map, $cle)` | Recupere une valeur | `map-get($c, primaire)` |
| `map-has-key($map, $cle)` | Verifie l'existence d'une cle | `map-has-key($c, danger)` |
| `map-keys($map)` | Extrait toutes les cles | `map-keys($c)` |
| `map-values($map)` | Extrait toutes les valeurs | `map-values($c)` |
| `map-merge($m1, $m2)` | Fusionne deux maps | `map-merge($m1, $m2)` |
| `map-remove($map, $cle)` | Supprime une cle | `map-remove($c, danger)` |
| `map-deep-merge($m1, $m2)` | Fusion recursive | `map-deep-merge($m1, $m2)` |
| `@each $k, $v in $map` | Itere sur la map | Boucle cle/valeur |

---

## Prochaine etape

Au chapitre suivant, nous verrons les **boucles Sass** (`@for`, `@while`, `@each`) en profondeur, qui permettent de generer du CSS de maniere programmatique et de creer des systemes de design dynamiques et maintenables.
