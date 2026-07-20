# Liste des Exercices — Cours SCSS/SASS

## Niveau 01 — Introduction à SCSS

### Exercice 1.1 : Premier fichier SCSS
**Difficulté :** Facile

**Consignes :**
1. Créer un fichier `style.scss`
2. Définir une variable `$main-color: #3498db`
3. Définir une variable `$font-stack: 'Arial', sans-serif`
4. Appliquer ces variables à un sélecteur `body`
5. Ajouter un sélecteur `h1` qui utilise la même couleur
6. Compiler le fichier en CSS

**Résultat attendu :**
```css
body {
  color: #3498db;
  font-family: 'Arial', sans-serif;
}

h1 {
  color: #3498db;
}
```

---

### Exercice 1.2 : Variables et opérations
**Difficulté :** Facile

**Consignes :**
1. Définir `$base-spacing: 16px`
2. Créer des variables `$spacing-xs`, `$spacing-sm`, `$spacing-md`, `$spacing-lg` en multipliant `$base-spacing` par 0.5, 1, 2 et 4
3. Créer une variable `$base-font-size: 16px`
4. Créer `$font-size-lg: $base-font-size * 1.5`
5. Utiliser ces variables dans un style de `header`

**Résultat attendu :**
```css
header {
  padding: 16px;
  font-size: 24px;
}
```

---

### Exercice 1.3 : Compilation
**Difficulté :** Facile

**Consignes :**
1. Installer Sass avec npm
2. Créer un projet avec `package.json`
3. Ajouter les scripts `build` et `dev` (watch)
4. Compiler en mode compressed
5. Générer un source map

---

## Niveau 02 — Variables et Types de données

### Exercice 2.1 : Palette de couleurs
**Difficulté :** Facile

**Consignes :**
1. Créer une map `$colors` avec 5 couleurs (primary, secondary, danger, warning, success)
2. Créer une fonction `@function color($name)` qui retourne la couleur
3. Utiliser la fonction dans 5 sélecteurs différents

**Résultat attendu :**
```css
.alert-primary { border-color: #3498db; }
.alert-secondary { border-color: #2ecc71; }
.alert-danger { border-color: #e74c3c; }
.alert-warning { border-color: #f39c12; }
.alert-success { border-color: #27ae60; }
```

---

### Exercice 2.2 : Map imbriquée
**Difficulté :** Moyen

**Consignes :**
1. Créer une map `$themes` avec deux thèmes (light, dark), chacun contenant bg, text, border
2. Créer une fonction `@function theme($theme, $key)` pour accéder aux valeurs
3. Générer des styles pour chaque thème

**Résultat attendu :**
```css
[data-theme="light"] {
  background-color: #ffffff;
  color: #333333;
  border-color: #e0e0e0;
}

[data-theme="dark"] {
  background-color: #1a1a2e;
  color: #e0e0e0;
  border-color: #444444;
}
```

---

### Exercice 2.3 : Listes et boucles
**Difficulté :** Moyen

**Consignes :**
1. Créer une liste `$breakpoints: (sm: 576px, md: 768px, lg: 992px, xl: 1200px)`
2. Utiliser `@each` pour générer des classes `.visible-{bp}` et `.hidden-{bp}`
3. Utiliser `@media (min-width: ...)` pour chaque breakpoint

---

## Niveau 03 — Imbrication (Nesting)

### Exercice 3.1 : Navigation BEM
**Difficulté :** Facile

**Consignes :**
1. Créer une navigation avec la structure BEM : `.nav`, `.nav__item`, `.nav__link`, `.nav__link--active`
2. Utiliser le nesting SCSS pour organiser le code
3. Ajouter des styles au hover pour `.nav__link`

**Résultat attendu :**
```css
.nav { display: flex; gap: 16px; }
.nav__item { list-style: none; }
.nav__link { color: #333; text-decoration: none; }
.nav__link:hover { color: #3498db; }
.nav__link--active { color: #3498db; font-weight: bold; }
```

---

### Exercice 3.2 : Card responsive
**Difficulté :** Moyen

**Consignes :**
1. Créer un composant `.card` avec nesting
2. Éléments : `__header`, `__body`, `__footer`
3. Modificateurs : `--dark`, `--flat`
4. Ajouter un `@media` dans le nesting pour un layout horizontal sur desktop
5. **Règle** : pas plus de 3 niveaux de nesting

---

### Exercice 3.3 : Formulaire avec états
**Difficulté :** Moyen

**Consignes :**
1. Créer `.form-group`, `.form-label`, `.form-input`
2. Ajouter les états via nesting : `:hover`, `:focus`, `&--error`, `&--success`
3. Ajouter `.form-help` et `.form-error`
4. Limiter le nesting à 2 niveaux

---

## Niveau 4 — Les Mixins

### Exercice 4.1 : Mixin responsive
**Difficulté :** Moyen

**Consignes :**
1. Créer un mixin `@mixin respond-to($breakpoint)` qui prend un nom de breakpoint
2. Créer la map des breakpoints
3. Utiliser le mixin pour rendre `.sidebar` responsive (pleine largeur sur mobile, 300px sur desktop)

---

### Exercice 4.2 : Mixin de bouton
**Difficulté :** Moyen

**Consignes :**
1. Créer un mixin `@mixin button($bg, $color)` qui génère un style de bouton complet
2. Le mixin doit inclure : padding, border-radius, cursor, transition, hover state
3. Créer 3 variantes en utilisant le mixin : primary, secondary, danger
4. Ajouter des tailles : `--sm`, `--lg`

---

### Exercice 4.3 : Mixin de grille
**Difficulté :** Difficile

**Consignes :**
1. Créer un mixin `@mixin grid($columns, $gap)` qui génère une grille CSS
2. Créer un mixin `@mixin grid-item($span)` qui gère le span
3. Utiliser les deux mixins pour créer un layout 3 colonnes avec gap de 24px

---

## Niveau 05 — Les Fonctions

### Exercice 5.1 : Fonction rem()
**Difficulté :** Facile

**Consignes :**
1. Créer une fonction `@function rem($px)` qui convertit des pixels en rem
2. Utiliser la fonction pour définir les tailles de police d'une typographie
3. Créer des classes `.text-sm`, `.text-base`, `.text-lg`, `.text-xl`

---

### Exercice 5.2 : Fonction de couleur dynamique
**Difficulté :** Difficile

**Consignes :**
1. Créer une fonction `@function shade($color, $percent)` qui retourne une version plus foncée
2. Créer une fonction `@function tint($color, $percent)` qui retourne une version plus claire
3. Générer 5 nuances de `$color-primary` en utilisant ces fonctions
4. Créer des classes `.bg-primary-{100-500}`

---

### Exercice 5.3 : Map lookup
**Difficulté :** Moyen

**Consignes :**
1. Créer une map `$spacing: (0: 0, 1: 4px, 2: 8px, 3: 16px, 4: 24px, 5: 32px)`
2. Créer une fonction `@function space($key)` qui retourne la valeur
3. Générer des classes `.m-{n}` et `.p-{n}` pour chaque valeur de la map

---

## Niveau 06 — Les Placeholders

### Exercice 6.1 : Placeholders réutilisables
**Difficulté :** Facile

**Consignes :**
1. Créer un placeholder `%flex-center` qui centre un élément
2. Créer un placeholder `%truncate` qui tronque le texte
3. Appliquer ces placeholders à 3 éléments différents chacun
4. Comparer le CSS généré avec des mixins équivalents

---

### Exercice 6.2 : %clearfix
**Difficulté :** Moyen

**Consignes :**
1. Créer un placeholder `%clearfix` avec la technique du `::after`
2. Créer un placeholder `%list-reset` qui supprime les styles de liste
3. Créer un placeholder `%visually-hidden` pour l'accessibilité
4. Appliquer chaque placeholder à au moins 2 éléments

---

## Niveau 07 — Les Extend et @import

### Exercice 7.1 : @extend vs @mixin
**Difficulté :** Moyen

**Consignes :**
1. Créer un mixin `@mixin button-base` et un placeholder `%button-base` avec les mêmes styles
2. Appliquer le mixin à 3 boutons et le placeholder à 3 autres
3. Compiler et comparer le CSS généré
4. Expliquer la différence en commentaire

---

### Exercice 7.2 : Structure d'imports
**Difficulté :** Moyen

**Consignes :**
1. Créer la structure :
   - `abstracts/_variables.scss`
   - `abstracts/_mixins.scss`
   - `base/_reset.scss`
   - `components/_buttons.scss`
2. Utiliser `@import` pour tout assembler dans `main.scss`
3. Compiler et vérifier que tout fonctionne

---

## Niveau 08 — Architecture

### Exercice 8.1 : Architecture 7-1
**Difficulté :** Difficile

**Consignes :**
1. Créer l'arborescence 7-1 complète (7 dossiers + main.scss)
2. Créer au moins 2 fichiers par dossier
3. Remplir chaque fichier avec du contenu pertinent
4. Créer `main.scss` avec les imports dans le bon ordre
5. Compiler sans erreur

---

### Exercice 8.2 : Design Token System
**Difficulté :** Difficile

**Consignes :**
1. Créer un système de tokens complet avec :
   - Palette de couleurs (5 familles × 5 nuances)
   - Échelle d'espacement (8 tailles)
   - Échelle typographique (6 tailles)
   - Ombres (4 niveaux)
   - Border radius (4 tailles)
2. Créer des fonctions pour accéder à chaque token
3. Générer des classes utilitaires automatiquement

---

### Exercice 8.3 : Architecture ITCSS
**Difficulté :** Difficile

**Consignes :**
1. Créer les 7 couches ITCSS (Settings, Tools, Generic, Elements, Objects, Components, Utilities)
2. Placer le bon type de code dans chaque couche
3. Respecter l'ordre des couches dans le fichier d'entrée
4. S'assurer qu'aucune couche ne revient en arrière

---

## Niveau 09 — Responsive

### Exercice 9.1 : Media Queries Mixin
**Difficulté :** Moyen

**Consignes :**
1. Créer un mixin `@mixin respond-to($bp)` et `@mixin respond-below($bp)`
2. Créer la map des breakpoints
3. Créer un layout avec sidebar qui passe en dessous sur mobile
4. Tester sur 3 breakpoints

---

### Exercice 9.2 : Container Queries
**Difficulté :** Difficile

**Consignes :**
1. Créer un composant `.card` qui s'adapte à son conteneur
2. Utiliser `container-type: inline-size`
3. Créer 3 layout différents selon la taille du conteneur (200px, 400px, 600px)
4. Ajouter un fallback pour les navigateurs non supportés

---

### Exercice 9.3 : Fluid Design
**Difficulté :** Difficile

**Consignes :**
1. Créer un mixin `@mixin fluid-type($min, $max)` utilisant `clamp()`
2. Créer un mixin `@mixin fluid-spacing($prop, $min, $max)`
3. Appliquer à un header, un titre, un paragraphe et un padding de section
4. Vérifier que la taille change continuellement (pas de sauts)

---

## Niveau 10 — Animations

### Exercice 10.1 : Transitions
**Difficulté :** Facile

**Consignes :**
1. Créer des variables pour les durées (fast, normal, slow)
2. Créer des variables pour les easings (ease-in, ease-out, ease-in-out)
3. Créer un mixin `@mixin transition($props, $dur, $ease)`
4. Appliquer une transition de couleur à un lien et une transition de taille à un bouton

---

### Exercice 10.2 : Bibliothèque d'animations
**Difficulté :** Moyen

**Consignes :**
1. Créer les keyframes suivants : `fadeIn`, `slideUp`, `slideDown`, `bounce`, `pulse`
2. Créer un mixin `@mixin animate($name, $dur)` pour les appliquer
3. Créer des classes utilitaires : `.animate-fade-in`, `.animate-slide-up`, etc.
4. Ajouter un mixin pour les delays séquentiels (stagger)

---

### Exercice 10.3 : Skeleton loading
**Difficulté :** Difficile

**Consignes :**
1. Créer une animation `shimmer` qui déplace un dégradé
2. Créer un composant `.skeleton` avec variantes : `--text`, `--circle`, `--rect`
3. Créer un composant `.skeleton-card` qui combine plusieurs skeletons
4. Respecter `prefers-reduced-motion`

---

## Niveau 11 — Optimisation

### Exercice 11.1 : Comparaison de tailles
**Difficulté :** Moyen

**Consignes :**
1. Écrire le même design en SCSS puis en CSS
2. Compiler les deux et comparer les tailles
3. Optimiser le SCSS pour réduire la taille du CSS
4. Mesurer la différence en pourcentage

---

### Exercice 11.2 : Suppression du code mort
**Difficulté :** Moyen

**Consignes :**
1. Créer 5 variables non utilisées
2. Créer 3 mixins non utilisés
3. Installer Stylelint et le configurer
4. Lancer le lint et identifier les variables/mixins inutilisés
5. Supprimer le code mort

---

### Exercice 11.3 : Pipeline de build
**Difficulté :** Difficile

**Consignes :**
1. Configurer un pipeline avec npm scripts pour :
   - Compiler le SCSS
   - Minifier le CSS (PostCSS + cssnano)
   - Générer les source maps
2. Créer un script `build:prod` et un script `dev`
3. Tester que les deux fonctionnent

---

## Niveau 12 — Bonnes Pratiques

### Exercice 12.1 : Code review
**Difficulté :** Moyen

**Consignes :**
1. On vous donne un fichier SCSS mal écrit (nesting à 6 niveaux, noms non descriptifs, !important partout)
2. Réécrire le fichier en suivant les bonnes pratiques BEM
3. Limiter le nesting à 3 niveaux maximum
4. Supprimer tous les !importants

---

### Exercice 12.2 : Convention BEM
**Difficulté :** Facile

**Consignes :**
1. Créer un composant Modal avec BEM :
   - `.modal` (block)
   - `.modal__overlay`, `.modal__content`, `.modal__header`, `.modal__body`, `.modal__footer`
   - `.modal--open`, `.modal--fullscreen`
2. Tout écrire avec du nesting SCSS propre (max 3 niveaux)

---

### Exercice 12.3 : Documentation
**Difficulté :** Moyen

**Consignes :**
1. Créer un composant `.tooltip` complet
2. Ajouter une documentation en commentaires en haut du fichier :
   - Description
   - Utilisation (HTML)
   - Modificateurs disponibles
   - Variables configurables
3. Rendre le code auto-documenté

---

## Niveau 13 — Sécurité et Qualité

### Exercice 13.1 : Sécurité
**Difficulté :** Difficile

**Consignes :**
1. Écrire un mixin qui prend une entrée utilisateur et génère un sélecteur
2. Identifier le risque de sécurité (injection CSS)
3. Réécrire le mixin avec une whitelist de valeurs autorisées
4. Tester avec des valeurs malveillantes

---

### Exercice 13.2 : Linting complet
**Difficulté :** Difficile

**Consignes :**
1. Installer Stylelint avec la config SCSS
2. Configurer les règles : max-nesting-depth, no-important, BEM pattern
3. Écrire 3 fichiers SCSS intentionally bad
4. Lancer le lint et corriger toutes les erreurs
5. Faire passer le lint sans erreur

---

### Exercice 13.3 : @use vs @import
**Difficulté :** Moyen

**Consignes :**
1. Créer 3 modules avec `@forward`
2. Créer un fichier principal qui utilise `@use` pour importer les modules
3. Démontrer qu'il n'y a pas de conflit de noms
4. Comparer avec la même structure en `@import`

---

## Niveau 14 — Projet Final

### Exercice 14.1 : Mini Design System (complet)
**Difficulté :** Difficile

**Consignes :**
Créer un Mini Design System complet avec :

1. **Architecture 7-1** avec `@use`/`@forward`
2. **Tokens** : couleurs, espacement, typographie, ombres
3. **Composants** : boutons (5 variantes), cartes (3 types), formulaires (complets)
4. **Layout** : grille responsive, conteneur, header, footer
5. **Utilities** : spacing, display, text (auto-générées)
6. **Thèmes** : light et dark avec CSS Custom Properties
7. **Documentation** : chaque fichier commenté

**Livrables :**
- Arborescence complète
- Fichiers SCSS fonctionnels
- CSS compilé et minifié
- Page HTML de démonstration

---

### Exercice 14.2 : Système de tokens avancé
**Difficulté :** Difficile

**Consignes :**
Créer un système de tokens qui génère automatiquement :
1. Des variables CSS Custom Properties
2. Des classes utilitaires
3. Des variantes dark mode

Le système doit être entièrement configurable via une map de tokens.

---

### Exercice 14.3 : Bibliothèque de composants
**Difficulté :** Difficile

**Consignes :**
Créer une bibliothèque de composants avec :
1. **Bouton** : 6 variantes × 3 tailles × 2 états = 36 classes
2. **Badge** : 5 couleurs × 2 formes
3. **Alert** : 4 types × 2 tailles
4. **Card** : 3 layouts × 2 styles

Chaque composant doit :
- Être dans son propre fichier
- Utiliser BEM
- Avoir des variables configurables (!default)
- Être documenté
- Passer le lint

---

## Barème de notation

| Niveau | Exercice | Points |
|---|---|---|
| 01 | 1.1, 1.2, 1.3 | 5, 5, 5 |
| 02 | 2.1, 2.2, 2.3 | 5, 10, 10 |
| 03 | 3.1, 3.2, 3.3 | 5, 10, 10 |
| 04 | 4.1, 4.2, 4.3 | 10, 10, 15 |
| 05 | 5.1, 5.2, 5.3 | 5, 15, 10 |
| 06 | 6.1, 6.2 | 5, 10 |
| 07 | 7.1, 7.2 | 10, 10 |
| 08 | 8.1, 8.2, 8.3 | 15, 15, 15 |
| 09 | 9.1, 9.2, 9.3 | 10, 15, 15 |
| 10 | 10.1, 10.2, 10.3 | 5, 10, 15 |
| 11 | 11.1, 11.2, 11.3 | 10, 10, 15 |
| 12 | 12.1, 12.2, 12.3 | 10, 5, 10 |
| 13 | 13.1, 13.2, 13.3 | 15, 15, 10 |
| 14 | 14.1, 14.2, 14.3 | 25, 20, 25 |

**Total : 335 points**
