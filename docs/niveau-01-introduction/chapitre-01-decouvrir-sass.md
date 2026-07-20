# Chapitre 1 — Découvrir SASS

> **Niveau** : Débutant | **Durée estimée** : 45 minutes

---

## Objectifs du chapitre

À l'issue de ce chapitre, vous serez capable de :

- Expliquer ce qu'est SASS et pourquoi il existe
- Comparer le CSS natif avec SASS
- Connaître les avantages et inconvénients de SASS
- Distinguer les deux syntaxes (Sass et SCSS)
- Comprendre comment fonctionne le compilateur

---

## 1. Qu'est-ce que SASS ?

**SASS** signifie **Syntactically Awesome Style Sheets**. C'est un **préprocesseur CSS** : un outil qui étend les fonctionnalités du CSS en ajoutant des features qui n'existent pas dans le langage natif.

Concrètement, SASS vous permet d'écrire du CSS de manière plus puissante et plus organisée, puis un **compilateur** transforme votre code SASS en CSS standard que les navigateurs peuvent interpréter.

```
┌─────────────────┐      ┌──────────────┐      ┌─────────────────┐
│  Fichier .scss   │ ───▶ │  Compilateur │ ───▶ │  Fichier .css    │
│  (votre code)    │      │    SASS      │      │  (navigateur)    │
└─────────────────┘      └──────────────┘      └─────────────────┘
```

SASS a été créé par **Hampton Catlin** en 2006 et est aujourd'hui l'un des préprocesseurs CSS les plus utilisés au monde. Il est maintenu par une communauté active et est支持é par tous les grands frameworks CSS.

---

## 2. Pourquoi SASS existe-t-il ?

### Les limitations du CSS

Le CSS natif, bien que puissant pour le style de base, présente plusieurs limitations quand on travaille sur des projets de grande envergure :

**a) Pas de variables**

En CSS pur, vous ne pouvez pas définir de réutiliser des valeurs :

```css
/* CSS natif — pas de variables ! */
.header {
  color: #333333;
}

.footer {
  color: #333333; /* même valeur, répétée */
}

.sidebar {
  color: #333333; /* encore ! */
}
```

Si vous voulez changer cette couleur, vous devez la modifier **à chaque endroit** où elle apparaît. Sur un gros projet, c'est un cauchemar.

**b) Pas de calculs dynamiques**

Le CSS a un système de `calc()`, mais il est limité. Vous ne pouvez pas facilement calculer des couleurs, des proportions complexes, ou des grilles dynamiques.

**c) Pas d'imbrication (nesting) naturelle**

En HTML, les éléments sont imbriqués. En CSS, vous devez répéter les sélecteurs :

```css
/* CSS natif — répétition de sélecteurs */
.nav { }
.nav ul { }
.nav ul li { }
.nav ul li a { }
.nav ul li a:hover { }
```

**d) Pas de réutilisation de code (DRY)**

Le principe **DRY** (Don't Repeat Yourself) est difficile à appliquer en CSS pur. Vous devez copier-coller des blocs de code similaires.

**e) Pas de fonctions**

Le CSS ne permet pas de créer des fonctions réutilisables pour générer du style.

### La solution : SASS

SASS résout **tous** ces problèmes en ajoutant des fonctionnalités puissantes tout en restant compatible avec le CSS existant.

---

## 3. Comparaison CSS vs SASS

### Exemple 1 : Variables

**CSS (avec variables CSS, limité) :**
```css
:root {
  --primary-color: #3498db;
  --font-size-base: 16px;
}

.button {
  background-color: var(--primary-color);
  font-size: var(--font-size-base);
}
```

**SASS :**
```scss
$primary-color: #3498db;
$font-size-base: 16px;

.button {
  background-color: $primary-color;
  font-size: $font-size-base;
}
```

> **Différence clé** : Les variables CSS (`--var`) sont une bonne chose, mais elles sont limitées aux navigateurs modernes et ne permettent pas de calculs complexes en amont. Les variables SASS sont résolues à la **compilation**, donc elles fonctionnent partout.

### Exemple 2 : Imbrication

**CSS :**
```css
.card { }
.card .card-header { }
.card .card-header .title { }
.card .card-body { }
.card .card-footer { }
```

**SASS :**
```scss
.card {
  .card-header {
    .title {
      // style du titre
    }
  }

  .card-body {
    // style du corps
  }

  .card-footer {
    // style du pied
  }
}
```

### Exemple 3 : Mixins (fonctions de style)

**CSS :**
```css
.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.flex-between {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.flex-start {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
```

**SASS :**
```scss
@mixin flex-align($justify: center, $align: center) {
  display: flex;
  justify-content: $justify;
  align-items: $align;
}

.flex-center  { @include flex-align(); }
.flex-between { @include flex-align(space-between); }
.flex-start   { @include flex-align(flex-start); }
```

### Exemple 4 : Opérations arithmétiques

**CSS :**
```css
.container {
  width: 960px;
}

.sidebar {
  width: 300px;
}

.content {
  width: 660px; /* calculé manuellement ! */
}
```

**SASS :**
```scss
$container-width: 960px;
$sidebar-width: 300px;

.container {
  width: $container-width;
}

.sidebar {
  width: $sidebar-width;
}

.content {
  width: $container-width - $sidebar-width; /* calcul automatique ! */
}
```

### Exemple 5 : Génération de classes avec des boucles

**CSS (impossible sans répétition) :**
```css
.col-1 { width: 8.333%; }
.col-2 { width: 16.666%; }
.col-3 { width: 25%; }
/* ... jusqu'à .col-12 */
```

**SASS :**
```scss
@for $i from 1 through 12 {
  .col-#{$i} {
    width: percentage($i / 12);
  }
}
```

---

## 4. Les avantages de SASS

### ✅ Variables puissantes

Définissez des couleurs, polices, espacements et réutilisez-les partout :

```scss
// Palette de couleurs
$color-primary: #3498db;
$color-secondary: #2ecc71;
$color-danger: #e74c3c;
$color-warning: #f39c12;

// Espacements
$spacing-xs: 4px;
$spacing-sm: 8px;
$spacing-md: 16px;
$spacing-lg: 32px;
$spacing-xl: 64px;

// Polices
$font-family-base: 'Helvetica Neue', Arial, sans-serif;
$font-family-heading: 'Georgia', serif;

// Utilisation
.button {
  padding: $spacing-sm $spacing-md;
  font-family: $font-family-base;
  background-color: $color-primary;
}
```

### ✅ Imbrication (Nesting)

Le code reflète la structure HTML, ce qui le rend plus lisible :

```scss
.navigation {
  display: flex;

  &__item {
    padding: 10px;

    &:hover {
      background-color: rgba(0, 0, 0, 0.1);
    }

    &--active {
      font-weight: bold;
      color: $color-primary;
    }
  }
}
```

Le `&` fait référence au sélecteur parent. Ce code génère :

```css
.navigation { display: flex; }
.navigation__item { padding: 10px; }
.navigation__item:hover { background-color: rgba(0, 0, 0, 0.1); }
.navigation__item--active { font-weight: bold; color: #3498db; }
```

### ✅ Mixins

Créez des blocs de style réutilisables avec des paramètres :

```scss
@mixin responsive($breakpoint) {
  @if $breakpoint == mobile {
    @media (max-width: 576px) { @content; }
  } @else if $breakpoint == tablet {
    @media (max-width: 768px) { @content; }
  } @else if $breakpoint == desktop {
    @media (max-width: 992px) { @content; }
  }
}

// Utilisation
.container {
  width: 100%;

  @include responsive(mobile) {
    padding: 0 16px;
  }

  @include responsive(tablet) {
    padding: 0 32px;
  }
}
```

### ✅ Partials et imports

Organisez votre CSS en fichiers modulaires :

```scss
// styles.scss — fichier principal
@import 'variables';
@import 'mixins';
@import 'base/reset';
@import 'components/buttons';
@import 'components/cards';
@import 'layout/header';
@import 'layout/footer';
```

### ✅ Fonctions intégrées

SASS fournit des dizaines de fonctions utiles :

```scss
// Assombrir une couleur
$dark-primary: darken($color-primary, 10%);

// Éclaircir une couleur
$light-primary: lighten($color-primary, 20%);

// Transparence
$semi-transparent: rgba($color-primary, 0.5);

// Arrondir des nombres
$rounded: round(3.14159); // 3

// Calculer des pourcentages
$half: percentage(1 / 2); // 50%
```

### ✅ Génération automatique

Créez des grilles, des listes de couleurs, et autres patterns CSS de manière programmatique.

---

## 5. Les inconvénients de SASS

### ❌ Complexité supplémentaire

SASS ajoute une couche d'abstraction. Pour les petits projets, cela peut être **overkill**.

### ❌ Temps de compilation

Vous devez compiler votre code avant de l'utiliser. Cela ajoute une étape au processus de développement.

### ❌ Courbe d'apprentissage

Il faut apprendre la syntaxe SASS en plus du CSS. Les mixins, les fonctions, les boucles... ce sont de nouveaux concepts.

### ❌ Débogage possible

Quand le CSS généré ne correspond pas à vos attentes, il faut comprendre ce que le compilateur a fait. Les **source maps** aident, mais c'est un effort supplémentaire.

### ❌ Sur-optimisation

Il est tentant de créer des mixins et des fonctions partout, même quand une simple ligne CSS suffit. C'est le piège du **SASS over-engineering**.

> **Règle d'or** : Utilisez SASS quand il apporte une vraie valeur. Ne complexifiez pas le code pour le plaisir de utiliser des features avancées.

---

## 6. Les deux syntaxes

SASS existe en deux syntaxes différentes. Il est important de les connaître pour ne pas être surpris en rencontrant des projets qui utilisent l'une ou l'autre.

### Syntaxe Sass (indentation)

La syntaxe originale, basée sur l'indentation (comme le Python). Elle n'utilise pas d'accolades `{}` ni de points-virgules `;`.

```sass
$primary-color: #3498db

.button
  background-color: $primary-color
  padding: 10px 20px
  border: none
  border-radius: 4px

  &:hover
    background-color: darken($primary-color, 10%)
```

**Fichiers** : extensions `.sass`

### Syntaxe SCSS (Sassy CSS)

La syntaxe la plus utilisée aujourd'hui. Elle est **100% compatible avec le CSS** : tout fichier CSS valide est aussi un fichier SCSS valide.

```scss
$primary-color: #3498db;

.button {
  background-color: $primary-color;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;

  &:hover {
    background-color: darken($primary-color, 10%);
  }
}
```

**Fichiers** : extensions `.scss`

### Laquelle choisir ?

| Critère | Sass (indentation) | SCSS |
|---------|-------------------|------|
| Lisibilité | Plus propre, moins de bruit | Plus proche du CSS |
| Compatibilité CSS | Non compatible | 100% compatible |
| Adoption | Minoritaire | **Majoritaire** |
| Courbe d'apprentissage | Plus raide | Plus douce |
| Utilisation dans les projets | Rare | **Quasi universelle** |

> **Recommandation** : Apprenez et utilisez **SCSS**. C'est la syntaxe standard utilisée dans l'industrie. La syntaxe Sass est intéressante historiquement mais rarement utilisée en production.

---

## 7. Comment fonctionne le compilateur

### Le principe

Le compilateur SASS prend votre code SCSS et le transforme en CSS standard. C'est un programme qui :

1. **Lit** votre fichier `.scss`
2. **Analyse** la syntaxe (parsing)
3. **Résout** les variables, mixins, imports, opérations
4. **Génère** un fichier `.css` valide

### Exemple concret

**Entrée** (`style.scss`) :
```scss
$primary: #3498db;
$spacing: 16px;

@mixin box-model($padding) {
  padding: $padding;
  box-sizing: border-box;
}

.container {
  @include box-model($spacing);
  background-color: $primary;
  border: 1px solid darken($primary, 15%);
}
```

**Sortie** (`style.css`) :
```css
.container {
  padding: 16px;
  box-sizing: border-box;
  background-color: #3498db;
  border: 1px solid #2980b9;
}
```

### Compilation manuelle vs automatique

```bash
# Compilation manuelle (une fois)
sass input.scss output.css

# Compilation en mode surveillance (automatique)
sass --watch input.scss:output.css
```

---

## 8. Comment les navigateurs lisent le CSS

Point fondamental : **aucun navigateur ne comprend SASS**. Le navigateur ne lit que du CSS standard.

```
Vous écrivez SCSS
        ↓
Le compilateur transforme en CSS
        ↓
Le navigateur applique le CSS
```

C'est pourquoi la compilation est **obligatoire**. Sans étape de compilation, votre site n'aurait aucun style.

### Bonnes pratiques

- Ne **jamais** envoyer un fichier `.scss` au navigateur
- Toujours inclure le fichier `.css` compilé dans votre HTML
- En production, utilisez un CSS **minifié** (compresse les espaces et sauts de ligne)

```html
<!-- Bonne pratique -->
<link rel="stylesheet" href="style.css">

<!-- Mauvaise pratique -->
<link rel="stylesheet" href="style.scss"> <!-- ❌ Ne fonctionnera pas -->
```

---

## 9. Résumé

| Concept | Description |
|---------|-------------|
| SASS | Préprocesseur CSS qui étend les fonctionnalités du CSS |
| SCSS | La syntaxe la plus utilisée, compatible avec le CSS |
| Sass | Syntaxe alternative basée sur l'indentation |
| Compilateur | Programme qui transforme SCSS en CSS |
| Variables | `$variable` — stockent des valeurs réutilisables |
| Nesting | Imbrication des sélecteurs avec `{}` |
| Mixins | `@mixin` — blocs de style réutilisables |
| Partials | Fichiers `_fichier.scss` importés dans un fichier principal |

---

## 10. Exercices

### Exercice 1 — Comparaison CSS/SCSS

Créez deux fichiers :

1. `exercice1.css` — Style natif pour un composant "bouton" avec :
   - padding de `12px 24px`
   - couleur de fond `#3498db`
   - texte blanc
   - border-radius de `4px`
   - un état hover avec couleur `#2980b9`

2. `exercice1.scss` — Le même style en SCSS en utilisant des **variables** pour la couleur, le padding et le border-radius.

**Question** : Quels sont les avantages du fichier SCSS ?

---

### Exercice 2 — Imbrication

En CSS natif, écrivez le style pour une structure HTML de navigation :

```html
<nav class="main-nav">
  <ul>
    <li><a href="#">Accueil</a></li>
    <li><a href="#">Services</a></li>
    <li><a href="#">Contact</a></li>
  </ul>
</nav>
```

Puis refaites le même style en SCSS avec l'imbrication (nesting).

**Question** : Comparez le nombre de lignes et la lisibilité.

---

### Exercice 3 — Recherche

Faites des recherches sur les sites suivants et répondez aux questions :

1. Quelle est la dernière version de SASS en 2024 ?
2. Quel est le differences entre Dart Sass et LibSass ?
3. Pourquoi LibSass est-il déprécié ?

> **Bonus** : Essayez de compiler un fichier SCSS en utilisant l'outil en ligne [Sass Playground](https://sass-lang.com/playground/).

---

## Pour aller plus loin

- [Documentation officielle SASS](https://sass-lang.com/documentation/)
- [Sass Guidelines (FR)](https://sass-guidelin.es/fr/)
- [Sass Playground](https://sass-lang.com/playground/)

---

**Prochaine étape** : [Chapitre 2 — Installation](chapitre-02-installation.md)
