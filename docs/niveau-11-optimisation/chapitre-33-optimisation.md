# Chapitre 33 — Optimisation SCSS

## Introduction

L'optimisation du code SCSS passe par la compilation, la compression, la minification, l'analyse des performances et la bonne gestion des dépendances. Ce chapitre couvre toutes les techniques pour produire un CSS final performant.

---

## Options de compilation

### Compilation standard

```bash
# Compilation basique
sass input.scss output.css

# Compilation avec style compressé
sass --style=compressed input.scss output.css

# Compilation watch (surveillance)
sass --watch input.scss:output.css

# Compilation du dossier entier
sass --watch src/:dist/
```

### Options CLI

```bash
# Toutes les options
sass --help

# Options principales :
# --style=expanded|compressed|compact
# --source-map
# --no-source-map
# --watch
# --update
# --load-path=PATH
# --no-charset
# --error-css
# --quiet-deps
# --silence-deprecation=DEP
```

---

## Styles de sortie

### Comparaison des styles

```scss
// input.scss
$color: #3498db;
$size: 16px;

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 24px;

  &__title {
    color: $color;
    font-size: $size * 2;
    font-weight: 700;
  }

  &__text {
    color: darken($color, 20%);
    line-height: 1.5;
  }
}
```

### Expanded (développé)

```css
/* output: expanded */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 24px;
}

.container__title {
  color: #3498db;
  font-size: 32px;
  font-weight: 700;
}

.container__text {
  color: #2980b9;
  line-height: 1.5;
}
```

### Compressed (compressé)

```css
/* output: compressed */
.container{max-width:1200px;margin:0 auto;padding:24px}.container__title{color:#3498db;font-size:32px;font-weight:700}.container__text{color:#2980b9;line-height:1.5}
```

### Compact

```css
/* output: compact */
.container { max-width: 1200px; margin: 0 auto; padding: 24px; }
.container__title { color: #3498db; font-size: 32px; font-weight: 700; }
.container__text { color: #2980b9; line-height: 1.5; }
```

---

## Compression avancée

### Suppression des caractères inutiles

```scss
// Les espaces et sauts de ligne inutiles
$color: #3498db;

// Avant compilation :
.element {
  margin: 0 px;
  padding: 16 px;
  background-color: #3498db;
  background-color: rgba(52, 152, 219, 1);
  font-weight: 400;
  opacity: 1;
  display: block;
}

// Après compression :
.element {
  margin: 0px;
  padding: 16px;
  background-color: #3498db;
  font-weight: 400;
  opacity: 1;
  display: block;
}

// Optimisation manuelle :
.element {
  margin: 0;        // 0px → 0
  padding: 1rem;    // 16px → 1rem (si design system)
  background-color: #3498db;  // rgba inutile quand alpha=1
  font-weight: 400;          // supprimable si c'est la valeur par défaut
  opacity: 1;                // supprimable si c'est la valeur par défaut
  display: block;            // supprimable si c'est la seule option
}
```

### Règles d'optimisation

```scss
// 1. Éviter les valeurs redondantes
.bad {
  font-weight: bold;           // OK
  font-weight: 700;            // OK (identique)
  font-weight: bolder;         // Différent ! Attention
}

// 2. Utiliser les raccourcis
.bad {
  margin-top: 10px;
  margin-right: 10px;
  margin-bottom: 10px;
  margin-left: 10px;
}

.good {
  margin: 10px;               // Raccourci
}

// 3. Éviter les couleurs calculées inutilement
.bad {
  color: darken(#3498db, 0%);  // Inutile, retourne la même couleur
}

.good {
  color: #3498db;
}

// 4. Éviter les opérations inutiles
.bad {
  width: 100% * 1;            // Inutile
  font-size: 16px + 0px;     // Inutile
}

.good {
  width: 100%;
  font-size: 16px;
}
```

---

## Source Maps

### Qu'est-ce qu'un source map ?

Un source map est un fichier `.map` qui permet de mapper le CSS compilé vers le SCSS source. Indispensable pour le debugging.

### Génération

```bash
# Avec source map (par défaut)
sass input.scss output.css --source-map

# Sans source map
sass input.scss output.css --no-source-map

# Source map intégré (inline)
sass input.scss output.css --source-map=inline

# Source map avec chemin personnalisé
sass input.scss output.css --source-map=maps/output.css.map
```

### Structure du source map

```json
{
  "version": 3,
  "file": "output.css",
  "sourceRoot": "",
  "sources": [
    "../src/scss/abstracts/_variables.scss",
    "../src/scss/base/_reset.scss",
    "../src/scss/components/_buttons.scss"
  ],
  "names": [],
  "mappings": "AAAA,SAAS,..."
}
```

### Configuration dans un projet

```scss
// gulpfile.js (Gulp)
const sass = require('gulp-sass')(require('sass'));

gulp.task('sass', function () {
  return gulp.src('src/scss/**/*.scss')
    .pipe(sass({
      outputStyle: 'compressed',
      sourceMap: true,
      sourceMapContents: true,
      sourceMapEmbed: false,
    }).on('error', sass.logError))
    .pipe(gulp.dest('dist/css'));
});

// webpack.config.js
module.exports = {
  module: {
    rules: [{
      test: /\.scss$/,
      use: [
        'style-loader',
        'css-loader',
        {
          loader: 'sass-loader',
          options: {
            sourceMap: true,
          },
        },
      ],
    }],
  },
};
```

### Visualiser les source maps

```bash
# Ouvrir dans Chrome DevTools
# 1. Ouvrir l'onglet Sources
# 2. Chercher le fichier SCSS dans le tree
# 3. Cliquer sur une ligne pour voir le CSS correspondant
```

---

## Minification avancée

### PostCSS + CSSNano

```javascript
// postcss.config.js
module.exports = {
  plugins: [
    require('cssnano')({
      preset: ['advanced', {
        discardComments: {
          removeAll: true,
        },
        normalizeUrl: true,
        minifyFontValues: true,
        minifyGradients: true,
        mergeRules: true,
        mergeLonghand: true,
        reduceIdents: true,
        colormin: true,
      }],
    }),
  ],
};
```

### PurgeCSS

```javascript
// purgecss.config.js
module.exports = {
  content: [
    './src/**/*.html',
    './src/**/*.js',
    './src/**/*.jsx',
    './src/**/*.vue',
    './src/**/*.svelte',
  ],
  css: ['./dist/css/**/*.css'],
  defaultExtractor: content => content.match(/[\w-/:]+(?<!:)/g) || [],
  safelist: [
    'active',
    'show',
    'hidden',
    /^modal-/,
    /^tooltip-/,
    /^dropdown-/,
  ],
};
```

### Configuration SCSS pour PurgeCSS

```scss
// Les classes dynamiques doivent être dans des commentaires spéciaux
// ou dans un fichier séparé pour PurgeCSS

// classes-purge.scss
// @layer utilities {
//   .dynamic-class-1 { color: red; }
//   .dynamic-class-2 { color: blue; }
// }
```

---

## Performance SCSS

### Éviter la récursion profonde

```scss
// MAUVAIS : récursion profonde
@for $i from 1 through 100 {
  .item-#{$i} {
    width: $i * 1%;
    @for $j from 1 through 50 {
      .sub-#{$j} {
        margin-left: $j * 2px;
      }
    }
  }
}
// Résultat : potentiellement des milliers de règles

// BON : limiter la complexité
@for $i from 1 through 12 {
  .col-#{$i} {
    grid-column: span $i;
  }
}
```

### Limiter les opérations coûteuses

```scss
// MAUVAIS : opérations dans des boucles
@for $i from 1 through 1000 {
  .item-#{$i} {
    color: darken($color-primary, $i * 0.1%);
  }
}

// BON : valeurs pré-calculées ou map
$color-shades: (
  100: darken($color-primary, 5%),
  200: darken($color-primary, 10%),
  300: darken($color-primary, 15%),
  400: darken($color-primary, 20%),
  500: darken($color-primary, 25%),
);

@each $shade, $color in $color-shades {
  .text-primary-#{$shade} {
    color: $color;
  }
}
```

### Éviter les @import en boucle

```scss
// MAUVAIS : importer dans une boucle
@for $i from 1 through 10 {
  @import 'components/component-#{$i}';
}

// BON : importer chaque fichier séparément
@import 'components/component-1';
@import 'components/component-2';
// ...
@import 'components/component-10';
```

### Optimiser les maps

```scss
// MAUVAIS : maps imbriquées profondément
$map: (
  level1: (
    level2: (
      level3: (
        level4: (
          value: #333,
        ),
      ),
    ),
  ),
);

// BON : maps plats quand possible
$colors: (
  primary: #3498db,
  secondary: #2ecc71,
  danger: #e74c3c,
);

// Si nécessaire, max 2 niveaux d'imbrication
$theme-light: (
  bg: #ffffff,
  text: #333333,
  border: #e0e0e0,
);
```

---

## Analyse du CSS généré

### Outils d'analyse

```bash
# Avec stylestats
npx stylestats dist/css/main.css

# Avec pack-size
npx pack-size dist/css/main.css

# Avec CSS Analyzer
npx css-analyzer dist/css/main.css
```

### Analyse manuelle

```scss
// Ajouter des commentaires de debug
// # DEBUG: start - total rules count
// Le CSS généré aura ces commentaires
// # DEBUG: end - total rules count

// ou en SCSS :
// stylelint-disable-next-line
```

### Métriques à surveiller

```bash
# Taille totale
wc -c dist/css/main.css

# Nombre de lignes
wc -l dist/css/main.css

# Nombre de sélecteurs
grep -o '[^{]*{' dist/css/main.css | wc -l

# Nombre de media queries
grep -c '@media' dist/css/main.css

# Couleurs uniques
grep -oE '#[0-9a-fA-F]{3,6}' dist/css/main.css | sort -u | wc -l
```

---

## Arbre de dépendances

### Gestion des @use et @forward

```scss
// SCSS moderne : @use et @forward

// abstracts/_index.scss
@forward 'variables';
@forward 'functions';
@forward 'mixins';

// components/_index.scss
@forward 'buttons';
@forward 'cards';
@forward 'forms';

// main.scss
@use 'abstracts';
@use 'components';

// ou avec alias
@use 'abstracts' as a;
@use 'components' as c;

.container {
  @include a.flex-center;
  color: a.$color-primary;
}

.btn {
  @include c.button-styles;
}
```

### Éviter les conflits de noms

```scss
// Si deux modules exportent la même variable
@use 'abstracts/variables';
@use 'theme/variables';

// Conflit ! Solution : alias
@use 'abstracts/variables' as abs;
@use 'theme/variables' as theme;

.element {
  color: abs.$color-primary;
  background: theme.$bg-color;
}
```

### Arbre de dépendances propre

```
sass/
├── abstracts/
│   ├── _index.scss          ← forward tout
│   ├── _variables.scss
│   ├── _functions.scss
│   └── _mixins.scss
│
├── base/
│   ├── _index.scss
│   ├── _reset.scss
│   └── _typography.scss
│
├── components/
│   ├── _index.scss
│   ├── _buttons.scss        ← @use '../abstracts'
│   ├── _cards.scss          ← @use '../abstracts'
│   └── _forms.scss          ← @use '../abstracts'
│
└── main.scss                ← @use 'abstracts', 'base', 'components'
```

---

## Bundle CSS analysis

### Rapport de compilation

```bash
# Compiler avec verbose
sass --verbose input.scss output.css

# Compiler avec stats
sass --style=compressed input.scss output.css 2>&1 | grep -i "time\|rules\|size"
```

### Visualiser la taille par section

```scss
// Ajouter des commentaires de section pour l'analyse
// === ABSTRACTS (0 CSS) ===
// === BASE (X rules) ===
// === COMPONENTS (Y rules) ===
// === UTILITIES (Z rules) ===
```

### Outil d'audit

```scss
// audit.scss - fichier d'audit à compiler séparément
@debug "Nombre de variables: #{length($all-variables)}";
@debug "Nombre de mixins: #{length($all-mixins)}";
@debug "Nombre de composants: #{length($all-components)}";

// Compter les sélecteurs générés
$selector-count: 0;
@each $component in $components {
  // ...
}
```

---

## Bonnes pratiques d'optimisation

### 1. Compilation

```bash
# Production : toujours compressé
sass --style=compressed src/main.scss dist/css/main.css

# Développement : expanded avec source maps
sass --watch --style=expanded --source-map src/:dist/
```

### 2. Structure des imports

```scss
// Éviter les imports profonds
@import 'a/b/c/d/variable';    // MAUVAIS

// Préférer les imports plats avec index
@use 'abstracts';               // BON
```

### 3. Variables

```scss
// Éviter les variables inutilisées
$unused-color: red;    // Supprimez-la
$used-color: blue;     // Gardez-la

// Utiliser !default pour la configurabilité
$color-primary: #3498db !default;
```

### 4. Mixins

```scss
// Éviter les mixins qui génèrent beaucoup de CSS
@mixin massive-mixin {
  @for $i from 1 through 100 {
    .item-#{$i} { width: $i * 1%; }
  }
}

// Préférer des mixins ciblés
@mixin responsive-grid($cols) {
  display: grid;
  grid-template-columns: repeat($cols, 1fr);
}
```

### 5. Sélecteurs

```scss
// Éviter les sélecteurs trop spécifiques
.parent .child .grandchild .item { }  // MAUVAIS

.item { }  // BON (avec BEM)

// ou
.card__item { }  // BEM
```

---

## Arbre de fichiers optimisé

```
src/
├── abstracts/
│   ├── _index.scss
│   ├── _variables.scss
│   ├── _functions.scss
│   └── _mixins.scss
├── base/
│   ├── _index.scss
│   ├── _reset.scss
│   └── _typography.scss
├── components/
│   ├── _index.scss
│   ├── _buttons.scss
│   ├── _cards.scss
│   └── _forms.scss
├── utilities/
│   ├── _index.scss
│   └── _generated.scss
└── main.scss

dist/
├── css/
│   ├── main.css           ← Compressé
│   ├── main.css.map       ← Source map
│   └── main.min.css       ← Minifié (PostCSS)
└── maps/
    └── main.css.map
```

---

## Pipeline de build complet

```javascript
// gulpfile.js
const { src, dest, watch, series } = require('gulp');
const sass = require('gulp-sass')(require('sass'));
const postcss = require('gulp-postcss');
const cssnano = require('cssnano');
const autoprefixer = require('autoprefixer');
const purgecss = require('@fullhuman/postcss-purgecss');

function compileSass() {
  return src('src/scss/main.scss')
    .pipe(sass({ outputStyle: 'compressed' }).on('error', sass.logError))
    .pipe(dest('dist/css'));
}

function optimizeCss() {
  return src('dist/css/main.css')
    .pipe(postcss([
      autoprefixer(),
      cssnano({ preset: 'advanced' }),
    ]))
    .pipe(dest('dist/css'));
}

function purgeUnused() {
  return src('dist/css/main.css')
    .pipe(postcss([
      purgecss({
        content: ['src/**/*.{html,js,jsx,ts,tsx,vue}'],
      }),
    ]))
    .pipe(dest('dist/css'));
}

exports.build = series(compileSass, optimizeCss, purgeUnused);
exports.dev = () => watch('src/scss/**/*.scss', compileSass);
```

---

## Résumé

L'optimisation SCSS consiste à produire le CSS le plus petit et le plus performant possible. Utilisez `--style=compressed` en production, activez les source maps en développement, purgez le CSS inutilisé, et surveillez la taille de votre bundle. Une bonne architecture (7-1 ou ITCSS) facilite naturellement l'optimisation.
