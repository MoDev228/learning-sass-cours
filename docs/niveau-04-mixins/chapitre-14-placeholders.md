# Chapitre 14 : Les Placeholders (Extend)

## Introduction

Les **placeholders** (aussi appelés "extend") sont un mécanisme de Sass qui permet de **réutiliser des blocs de styles** sans dupliquer le CSS généré. Contrairement aux mixins qui insèrent le code CSS à chaque `@include`, les placeholders **combinent les sélecteurs** qui les utilisent en un seul bloc CSS.

Pensez aux placeholders comme à un **template partagé** : vous définissez un modèle de styles, et plusieurs classes peuvent "étendre" ce modèle. Le résultat est un CSS optimisé sans répétition.

### Le problème que résolvent les placeholders

Sans Sass :

```css
/* Code CSS répété */
.btn { padding: 10px 20px; border-radius: 4px; font-weight: 600; }
.btn-primary { padding: 10px 20px; border-radius: 4px; font-weight: 600; background: #3498db; }
.btn-secondary { padding: 10px 20px; border-radius: 4px; font-weight: 600; background: #95a5a6; }
```

Avec les placeholders :

```scss
%btn-base {
  padding: 10px 20px;
  border-radius: 4px;
  font-weight: 600;
}

.btn { @extend %btn-base; }
.btn-primary { @extend %btn-base; background: #3498db; }
.btn-secondary { @extend %btn-base; background: #95a5a6; }
```

**CSS généré (sans duplication) :**

```css
.btn, .btn-primary, .btn-secondary {
  padding: 10px 20px;
  border-radius: 4px;
  font-weight: 600;
}

.btn-primary { background: #3498db; }
.btn-secondary { background: #95a5a6; }
```

---

## 1. Créer un placeholder avec `%`

La syntaxe pour définir un placeholder est identique à une règle normale, mais avec `%` au lieu de `.` ou `#` :

```scss
%nom-du-placeholder {
  // propriétés CSS
}
```

Le `%` indique à Sass que ce bloc de styles **n'est pas directement rendu en CSS** — il n'existera dans le CSS final que s'il est utilisé par un `@extend`.

### Exemple de base

```scss
%clearfix {
  &::after {
    content: "";
    display: table;
    clear: both;
  }
}

%texte-centre {
  text-align: center;
}

%centre-absolu {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

**Sans `@extend`, aucun CSS n'est généré pour ces placeholders.**

---

## 2. Utiliser `@extend`

La directive `@extend` permet à une règle CSS d'hériter de toutes les propriétés d'un placeholder :

```scss
%nom-du-placeholder {
  // propriétés
}

.classe-qui-etend {
  @extend %nom-du-placeholder;
  // propriétés supplémentaires
}
```

### Exemple complet

```scss
// Définition des placeholders
%bouton-base {
  display: inline-block;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  text-decoration: none;
  transition: all 0.3s ease;
}

%texte-tronque {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

%ombre-carte {
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
}

// Utilisation avec @extend
.btn {
  @extend %bouton-base;
}

.btn-primary {
  @extend %bouton-base;
  background-color: #3498db;
  color: #ffffff;

  &:hover {
    background-color: darken(#3498db, 10%);
  }
}

.btn-danger {
  @extend %bouton-base;
  background-color: #e74c3c;
  color: #ffffff;

  &:hover {
    background-color: darken(#e74c3c, 10%);
  }
}

.titre-long {
  @extend %texte-tronque;
  font-size: 1.5rem;
  font-weight: 700;
}

.carte {
  @extend %ombre-carte;
  padding: 1.5rem;
  background-color: #ffffff;
}
```

**CSS généré :**

```css
.btn, .btn-primary, .btn-danger {
  display: inline-block;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  text-decoration: none;
  transition: all 0.3s ease;
}

.btn-primary {
  background-color: #3498db;
  color: #ffffff;
}

.btn-primary:hover {
  background-color: #2980b9;
}

.btn-danger {
  background-color: #e74c3c;
  color: #ffffff;
}

.btn-danger:hover {
  background-color: #c0392b;
}

.titre-long {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  font-size: 1.5rem;
  font-weight: 700;
}

.carte {
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 1.5rem;
  background-color: #ffffff;
}
```

---

## 3. `@extend` dans des sélecteurs imbriqués

Les placeholders fonctionnent avec l'imbrication Sass, ce qui les rend très puissants.

### Exemple avec des sélecteurs parent

```scss
%liste-base {
  list-style: none;
  padding: 0;
  margin: 0;

  li {
    padding: 0.5rem 0;
    border-bottom: 1px solid #eee;
  }

  a {
    color: inherit;
    text-decoration: none;

    &:hover {
      color: #3498db;
    }
  }
}

.nav-list {
  @extend %liste-base;

  li {
    display: inline-block;
    margin-right: 1rem;
  }
}

.dropdown-menu {
  @extend %liste-base;

  position: absolute;
  background: #ffffff;
  border-radius: 4px;

  li {
    padding: 0.75rem 1rem;
  }
}
```

**CSS généré :**

```css
.nav-list, .dropdown-menu {
  list-style: none;
  padding: 0;
  margin: 0;
}

.nav-list li, .dropdown-menu li {
  padding: 0.5rem 0;
  border-bottom: 1px solid #eee;
}

.nav-list a, .dropdown-menu a {
  color: inherit;
  text-decoration: none;
}

.nav-list a:hover, .dropdown-menu a:hover {
  color: #3498db;
}

.nav-list li {
  display: inline-block;
  margin-right: 1rem;
}

.dropdown-menu {
  position: absolute;
  background: #ffffff;
  border-radius: 4px;
}

.dropdown-menu li {
  padding: 0.75rem 1rem;
}
```

---

## 4. Quand utiliser `@extend` vs `@mixin`

C'est l'une des décisions les plus importantes en Sass. Voici un guide détaillé.

### Utilisez `@extend` (placeholders) quand :

1. **Les styles partagés sont de simples propriétés CSS** sans paramètres
2. **Vous voulez minimiser la taille du CSS** généré
3. **Vous avez plusieurs classes qui partagent les mêmes styles**
4. **Les styles ne dépendent pas de variables ou d'arguments**

```scss
// EXCELLENT pour @extend
%flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

%texte-tronque {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.hero { @extend %flex-center; }
.modal-content { @extend %flex-center; }
.loading-spinner { @extend %flex-center; }
```

### Utilisez `@mixin` quand :

1. **Vous avez besoin de paramètres** (valeurs d'entrée)
2. **Vous avez besoin de `@content`** pour passer un bloc de styles
3. **Vous avez besoin de `@if`/`@else`** avec des conditions complexes
4. **Vous générez des Media Queries** (les mixins fonctionnent mieux avec `@media`)
5. **Vous voulez inclure des vendor prefixes** conditionnels

```scss
// EXCELLENT pour @mixin
@mixin flex($direction: row, $justify: flex-start, $align: stretch) {
  display: flex;
  flex-direction: $direction;
  justify-content: $justify;
  align-items: $align;
}

@mixin responsive($breakpoint) {
  @media (max-width: $breakpoint) {
    @content;
  }
}

@mixin tronque($lignes: 1) {
  overflow: hidden;
  @if $lignes == 1 {
    white-space: nowrap;
    text-overflow: ellipsis;
  } @else {
    display: -webkit-box;
    -webkit-line-clamp: $lignes;
    -webkit-box-orient: vertical;
  }
}
```

### Tableau comparatif

| Critère | `@extend` (Placeholder) | `@mixin` |
|---|---|---|
| Paramètres | Non | Oui |
| Taille CSS | Optimisée (pas de duplication) | Peut dupliquer le code |
| Imbrication | Fonctionne | Fonctionne |
| Media Queries | Ne fonctionne pas | Fonctionne |
| Vendor Prefixes | Difficile | Facile |
| Logique conditionnelle | Limitée | Complète |
| `@content` | Non | Oui |
| Sélecteurs combinés | Oui (regroupe) | Non (insère tel quel) |

---

## 5. Les avantages des placeholders : pas de duplication de CSS

C'est le principal avantage des placeholders. Comparons :

### Avec des mixins (duplication)

```scss
@mixin bouton-base {
  padding: 10px 20px;
  border-radius: 4px;
  font-weight: 600;
}

.btn { @include bouton-base; }
.btn-primary { @include bouton-base; background: blue; }
.btn-secondary { @include bouton-base; background: gray; }
```

**CSS généré (dupliqué) :**

```css
.btn {
  padding: 10px 20px;
  border-radius: 4px;
  font-weight: 600;
}

.btn-primary {
  padding: 10px 20px;
  border-radius: 4px;
  font-weight: 600;
  background: blue;
}

.btn-secondary {
  padding: 10px 20px;
  border-radius: 4px;
  font-weight: 600;
  background: gray;
}
```

Le bloc de 3 propriétés est répété 3 fois = 9 déclarations CSS.

### Avec les placeholders (regroupé)

```scss
%bouton-base {
  padding: 10px 20px;
  border-radius: 4px;
  font-weight: 600;
}

.btn { @extend %bouton-base; }
.btn-primary { @extend %bouton-base; background: blue; }
.btn-secondary { @extend %bouton-base; background: gray; }
```

**CSS généré (regroupé) :**

```css
.btn, .btn-primary, .btn-secondary {
  padding: 10px 20px;
  border-radius: 4px;
  font-weight: 600;
}

.btn-primary { background: blue; }
.btn-secondary { background: gray; }
```

Le bloc de 3 propriétés apparaît **une seule fois** = 3 déclarations CSS.

---

## 6. Les limitations de `@extend`

### 6.1 Ne fonctionne pas avec les Media Queries

```scss
// Ceci NE FONCTIONNE PAS correctement
%responsive-texte {
  font-size: 1rem;

  @media (max-width: 768px) {
    font-size: 0.875rem;
  }
}

.titre {
  @extend %responsive-texte; // ERREUR !
}
```

**Raison** : Sass ne peut pas sortir un sélecteur d'une media query. Les `@extend` doivent être utilisés au même niveau de complexité de sélecteur.

**Solution** : utilisez un mixin à la place :

```scss
@mixin responsive-texte {
  font-size: 1rem;

  @media (max-width: 768px) {
    font-size: 0.875rem;
  }
}

.titre {
  @include responsive-texte;
}
```

### 6.2 Peut générer des sélecteurs complexes et inutiles

```scss
%base { color: blue; }

.foo .bar {
  @extend %base;
}

.baz .qux {
  @extend %base;
}
```

**CSS généré :**

```css
.foo .bar, .baz .qux {
  color: blue;
}
```

C'est propre ici, mais avec des sélecteurs imbriqués complexes, le résultat peut devenir lourd.

### 6.3 Peut modifier l'ordre du CSS

Les `@extend` regroupent les styles, ce qui peut modifier l'ordre dans lequel les propriétés apparaissent dans le CSS final. Si votre CSS dépend de l'ordre (ce qui ne devrait pas être le cas avec des classes bien isolées), c'est un problème potentiel.

### 6.4 Conflits avec des sélecteurs différents

```scss
%base { color: blue; }

.titre {
  @extend %base;
  color: red; // Conflit !
}

.texte {
  @extend %base;
}
```

**CSS généré :**

```css
.titre, .texte {
  color: blue;
}

.titre {
  color: red;
}
```

La règle de cascade s'applique — le dernier `color` gagne pour `.titre`. Mais `.texte` aura `color: blue` comme prévu.

---

## 7. Bonnes pratiques

### 7.1 Nommer les placeholders avec un préfixe

Utilisez un préfixe pour distinguer les placeholders des classes normales :

```scss
// Préfixe courant : %base-, %btn-, %card-, etc.
%btn-base { ... }
%btn-primair { ... }
%card-base { ... }
%card-elevee { ... }
%texte-centre { ... }
%ombre-legere { ... }
```

### 7.2 Garder les placeholders simples

Un placeholder ne devrait contenir qu'un **bloc de styles simple et réutilisable**. S'il contient de la logique complexe, utilisez une mixin.

```scss
// BON : simple et réutilisable
%centre-flex {
  display: flex;
  justify-content: center;
  align-items: center;
}

// MAUVAIS : trop complexe pour un placeholder
%complexe {
  display: flex;
  @if $condition { justify-content: center; }
  @for $i from 1 through 12 { /* ... */ }
}
```

### 7.3 Ne pas abuser de l'extension multiple

```scss
%base1 { color: blue; }
%base2 { font-weight: bold; }

// Fonctionne, mais peut devenir confus
.classe {
  @extend %base1;
  @extend %base2;
}

// Préférez la composition
.classe {
  @extend %base1;
  @extend %base2;
  // ou utilisez une mixin
}
```

### 7.4 Utiliser les placeholders pour les variantes d'un même composant

```scss
// BON : variantes du même composant
%bouton-base {
  padding: 10px 20px;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
}

.btn {
  @extend %bouton-base;
  background: #3498db;
  color: white;
}

.btn-success {
  @extend %bouton-base;
  background: #27ae60;
  color: white;
}

.btn-warning {
  @extend %bouton-base;
  background: #f39c12;
  color: white;
}
```

---

## 8. Placeholders vs Mixins : quand utiliser quoi ?

### Scénarios concrets

#### Scénario 1 : Styles partagés sans paramètres

```scss
// UTILISER @extend
%centre-flex {
  display: flex;
  justify-content: center;
  align-items: center;
}

.hero { @include centre-flex; }     // ❌ Ceci est une mixin, pas ce qu'on veut
.hero { @extend %centre-flex; }     // ✅ Utilise le placeholder
```

#### Scénario 2 : Styles avec paramètres

```scss
// UTILISER @mixin
@mixin flex($direction, $gap: 0) {
  display: flex;
  flex-direction: $direction;
  gap: $gap;
}

.navbar {
  @include flex(row, 1rem);
}

.sidebar {
  @include flex(column, 2rem);
}
```

#### Scénario 3 : Media Queries

```scss
// UTILISER @mixin
@mixin responsive($bp) {
  @media (max-width: $bp) { @content; }
}

.element {
  width: 100%;

  @include responsive(768px) {
    width: 50%;
  }
}
```

#### Scénario 4 : Vendor prefixes

```scss
// UTILISER @mixin
@mixin prefix($propriete, $valeur) {
  -webkit-#{$propriete}: $valeur;
  -moz-#{$propriete}: $valeur;
  -ms-#{$propriete}: $valeur;
  #{$propriete}: $valeur;
}

.element {
  @include prefix(display, flex);
}
```

---

## 9. Exemples pratiques avancés

### 9.1 Système de design avec placeholders

```scss
// ==========================================
// BASES DE COMPOSANTS (Placeholders)
// ==========================================

%card {
  background: #ffffff;
  border-radius: 12px;
  padding: 1.5rem;
  margin-bottom: 1rem;
}

%card-ombre {
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

%card-elevee {
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
              0 4px 6px -2px rgba(0, 0, 0, 0.05);
}

%card-transition {
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

%badge {
  display: inline-block;
  padding: 0.25rem 0.75rem;
  border-radius: 9999px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

%badge-success {
  background: #d4edda;
  color: #155724;
}

%badge-danger {
  background: #f8d7da;
  color: #721c24;
}

%badge-warning {
  background: #fff3cd;
  color: #856404;
}

// ==========================================
// COMPOSANTS CONCRETS
// ==========================================

// Carte simple
.carte {
  @extend %card;
}

// Carte avec ombre
.carte-ombre {
  @extend %card;
  @extend %card-ombre;
}

// Carte interactive
.carte-interactive {
  @extend %card;
  @extend %card-ombre;
  @extend %card-transition;
  cursor: pointer;

  &:hover {
    transform: translateY(-4px);
    box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1),
                0 10px 10px -5px rgba(0, 0, 0, 0.04);
  }
}

// Badges
.badge { @extend %badge; }
.badge-success { @extend %badge; @extend %badge-success; }
.badge-danger { @extend %badge; @extend %badge-danger; }
.badge-warning { @extend %badge; @extend %badge-warning; }
```

**CSS généré (optimisé) :**

```css
.carte, .carte-ombre, .carte-interactive {
  background: #ffffff;
  border-radius: 12px;
  padding: 1.5rem;
  margin-bottom: 1rem;
}

.carte-ombre, .carte-interactive {
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.carte-interactive {
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  cursor: pointer;
}

.carte-interactive:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1),
              0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

.badge, .badge-success, .badge-danger, .badge-warning {
  display: inline-block;
  padding: 0.25rem 0.75rem;
  border-radius: 9999px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.badge-success { background: #d4edda; color: #155724; }
.badge-danger { background: #f8d7da; color: #721c24; }
.badge-warning { background: #fff3cd; color: #856404; }
```

### 9.2 Système de grilles avec placeholders

```scss
// Placeholders de base pour la grille
%grid {
  display: grid;
  gap: 1rem;
}

%grid-colonnes-2 { grid-template-columns: repeat(2, 1fr); }
%grid-colonnes-3 { grid-template-columns: repeat(3, 1fr); }
%grid-colonnes-4 { grid-template-columns: repeat(4, 1fr); }

%flex-wrap {
  display: flex;
  flex-wrap: wrap;
}

// Utilisation
.grille-2 {
  @extend %grid;
  @extend %grid-colonnes-2;
}

.grille-3 {
  @extend %grid;
  @extend %grid-colonnes-3;
}

.grille-4 {
  @extend %grid;
  @extend %grid-colonnes-4;
}
```

### 9.3 Styles de formulaire

```scss
// Placeholders pour les formulaires
%input-base {
  display: block;
  width: 100%;
  padding: 0.625rem 0.875rem;
  font-size: 1rem;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  border: 1px solid #ced4da;
  border-radius: 4px;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;

  &::placeholder {
    color: #6c757d;
    opacity: 1;
  }

  &:focus {
    outline: 0;
    border-color: #80bdff;
    box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
  }

  &:disabled {
    background-color: #e9ecef;
    opacity: 1;
  }
}

%input-valide {
  border-color: #28a745;

  &:focus {
    border-color: #28a745;
    box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.25);
  }
}

%input-invalide {
  border-color: #dc3545;

  &:focus {
    border-color: #dc3545;
    box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.25);
  }
}

%label-base {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 600;
  color: #212529;
}

// Utilisation
.champ {
  @extend %input-base;
}

.champ-valide {
  @extend %input-base;
  @extend %input-valide;
}

.champ-invalide {
  @extend %input-base;
  @extend %input-invalide;
}

.label {
  @extend %label-base;
}
```

---

## 10. Exercices

### Exercice 1 : Créer des placeholders de base

Créez les placeholders suivants :
- `%centre-flex` : centre un élément avec flexbox
- `%texte-centre` : centre le texte
- `%ombre-legere` : ombre subtile
- `%bordure-tournee` : border-radius de 8px

Puis créez 3 classes qui utilisent ces placeholders.

```scss
// Votre réponse ici
```

### Exercice 2 : Système de boutons

Créez un système de boutons complet avec les placeholders :
- `%btn-base` : styles de base du bouton
- `%btn-plein` : version pleine (background-color coloré, texte blanc)
- `%btn-outline` : version contour (fond transparent, bordure colorée)

Puis créez les classes `.btn-primaire`, `.btn-success`, `.btn-danger`, `.btn-outline-primaire`, `.btn-outline-danger`.

```scss
// Votre réponse ici
```

### Exercice 3 : Comparer mixin et placeholder

Créez les deux versions suivantes et comparez le CSS généré :

**Version A avec mixin :**
```scss
@mixin card-styles {
  background: white;
  border-radius: 8px;
  padding: 1.5rem;
}

.card-1 { @include card-styles; }
.card-2 { @include card-styles; }
.card-3 { @include card-styles; }
```

**Version B avec placeholder :**
```scss
// Votre réponse ici
```

### Exercice 4 : Système d'alertes

Créez un système d'alertes avec placeholders :
- `%alerte-base` : styles de base (padding, border-radius, border)
- `%alerte-success` : vert
- `%alerte-warning` : orange
- `%alerte-danger` : rouge
- `%alerte-info` : bleu

Ajoutez aussi `%alerte-fermable` qui ajoute un style pour le bouton de fermeture.

```scss
// Votre réponse ici
```

### Exercice 5 : Formulaires avec placeholders

Créez un système de formulaires :
- `%input-base` : styles de base
- `%input-erreur` : état d'erreur
- `%input-succes` : état de succès
- `%label-base` : styles de label
- `%groupe-formulaire` : groupe label + input
- `%textarea-base` : extension de `%input-base` pour textarea

Puis créez les classes `.input`, `.input-erreur`, `.input-succes`, `.label`, `.groupe-formulaire`, `.textarea`.

```scss
// Votre réponse ici
```

---

## Résumé

| Concept | Syntaxe | Description |
|---|---|---|
| Définir un placeholder | `%nom { ... }` | Crée un bloc de styles réutilisable non rendu en CSS |
| Étendre un placeholder | `@extend %nom;` | Hérite des styles du placeholder |
| Avantage principal | Pas de duplication CSS | Les sélecteurs sont regroupés |
| Inconvénient principal | Pas de paramètres | Utilisez un mixin si vous avez besoin d'arguments |
| Media Queries | Ne fonctionne pas | Utilisez un mixin à la place |
| Sélecteurs imbriqués | Fonctionne | Attention aux sélecteurs complexes |

---

## Prochaine étape

Au chapitre suivant, nous verrons les **listes Sass**, une structure de données qui permet de stocker et manipuler des collections de valeurs, un outil indispensable pour créer des systèmes de design dynamiques.
