# Chapitre 5 : Imbrication (Nesting) en Sass/SCSS

## Objectifs pédagogiques

À l'issue de ce chapitre, vous serez capable de :
- Utiliser l'imbrication pour structurer votre code Sass
- Comprendre les avantages du nesting pour la lisibilité
- Éviter les pièges de l'imbrication excessive
- Maîtriser l'impact sur la spécificité CSS
- Appliquer les bonnes pratiques du nesting

---

## Introduction

L'imbrication (ou **nesting**) est l'une des fonctionnalités les plus puissantes et les plus intuitives de Sass. Elle vous permet d'imbriquer les sélecteurs CSS à l'intérieur les uns des autres, reflétant ainsi la **structure hiérarchique HTML** de votre page.

```html
<!-- La structure HTML que nous voulons cibler -->
<nav>
  <ul>
    <li>
      <a href="#">Accueil</a>
    </li>
  </ul>
</nav>
```

En CSS traditionnel, vous devriez écrire :

```css
/* CSS classique */
nav ul {
  list-style: none;
}
nav ul li {
  display: inline-block;
}
nav ul li a {
  text-decoration: none;
  color: white;
}
```

En Sass, grâce à l'imbrication :

```scss
// Sass avec imbrication
nav {
  ul {
    list-style: none;

    li {
      display: inline-block;

      a {
        text-decoration: none;
        color: white;
      }
    }
  }
}
```

---

## 1. Imbrication simple

### 1.1 Principes de base

L'imbrication fonctionne en plaçant un sélecteur **à l'intérieur** d'un autre sélecteur. Le sélecteur intérieur sera combiné avec le sélecteur extérieur.

```scss
// Syntaxe générale
父 {
  子 {
    propriété: valeur;
  }
}

// Équivaut en CSS à :
// 父 子 { propriété: valeur; }
```

### 1.2 Exemple : Navigation

```scss
// Sass
.nav {
  background-color: #333;
  padding: 10px 0;

  ul {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
  }

  li {
    margin-right: 20px;
  }

  a {
    color: white;
    text-decoration: none;
    font-weight: bold;

    &:hover {
      color: #3498db;
    }
  }
}
```

**CSS compilé :**

```css
.nav {
  background-color: #333;
  padding: 10px 0;
}

.nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
}

.nav li {
  margin-right: 20px;
}

.nav a {
  color: white;
  text-decoration: none;
  font-weight: bold;
}

.nav a:hover {
  color: #3498db;
}
```

> **Note :** Observez que chaque sélecteur imbriqué est combiné avec son parent avec un espace, ce qui correspond à un **descendant selector** en CSS.

---

## 2. Exemples d'imbrication en profondeur

### 2.1 Carte (Card) complète

```scss
.card {
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.3s ease;

  &:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
  }

  &__header {
    padding: 20px;
    border-bottom: 1px solid #eee;

    h2 {
      margin: 0;
      font-size: 1.5rem;
      color: #2c3e50;
    }

    .subtitle {
      margin: 5px 0 0;
      font-size: 0.875rem;
      color: #7f8c8d;
    }
  }

  &__body {
    padding: 20px;

    p {
      line-height: 1.6;
      color: #555;

      &:last-child {
        margin-bottom: 0;
      }
    }
  }

  &__footer {
    padding: 15px 20px;
    background-color: #f8f9fa;
    display: flex;
    justify-content: space-between;
    align-items: center;

    .btn {
      padding: 8px 16px;
      border-radius: 4px;
      border: none;
      cursor: pointer;
      font-weight: 600;

      &--primary {
        background-color: #3498db;
        color: white;

        &:hover {
          background-color: #2980b9;
        }
      }

      &--secondary {
        background-color: #ecf0f1;
        color: #333;

        &:hover {
          background-color: #ddd;
        }
      }
    }

    .meta {
      font-size: 0.75rem;
      color: #95a5a6;
    }
  }
}
```

**CSS compilé (extrait) :**

```css
.card {
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.3s ease;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

.card__header {
  padding: 20px;
  border-bottom: 1px solid #eee;
}

.card__header h2 {
  margin: 0;
  font-size: 1.5rem;
  color: #2c3e50;
}

.card__header .subtitle {
  margin: 5px 0 0;
  font-size: 0.875rem;
  color: #7f8c8d;
}

.card__body {
  padding: 20px;
}

.card__body p {
  line-height: 1.6;
  color: #555;
}

.card__body p:last-child {
  margin-bottom: 0;
}

.card__footer {
  padding: 15px 20px;
  background-color: #f8f9fa;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.card__footer .btn {
  padding: 8px 16px;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  font-weight: 600;
}

.card__footer .btn--primary {
  background-color: #3498db;
  color: white;
}

.card__footer .btn--primary:hover {
  background-color: #2980b9;
}

.card__footer .btn--secondary {
  background-color: #ecf0f1;
  color: #333;
}

.card__footer .btn--secondary:hover {
  background-color: #ddd;
}

.card__footer .meta {
  font-size: 0.75rem;
  color: #95a5a6;
}
```

### 2.2 Formulaire complet

```scss
.form {
  max-width: 600px;
  margin: 0 auto;
  padding: 30px;

  &__group {
    margin-bottom: 20px;

    label {
      display: block;
      margin-bottom: 8px;
      font-weight: 600;
      color: #333;
      font-size: 0.9rem;

      .required {
        color: #e74c3c;
        margin-left: 3px;
      }
    }

    input,
    textarea,
    select {
      width: 100%;
      padding: 12px 16px;
      border: 2px solid #e0e0e0;
      border-radius: 6px;
      font-size: 1rem;
      font-family: inherit;
      transition: border-color 0.3s ease, box-shadow 0.3s ease;
      box-sizing: border-box;

      &:focus {
        outline: none;
        border-color: #3498db;
        box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
      }

      &::placeholder {
        color: #aaa;
      }

      &.is-error {
        border-color: #e74c3c;

        &:focus {
          box-shadow: 0 0 0 3px rgba(231, 76, 60, 0.2);
        }
      }

      &.is-success {
        border-color: #2ecc71;

        &:focus {
          box-shadow: 0 0 0 3px rgba(46, 204, 113, 0.2);
        }
      }
    }

    textarea {
      min-height: 120px;
      resize: vertical;
    }

    select {
      appearance: none;
      background-image: url("data:image/svg+xml,...");
      background-repeat: no-repeat;
      background-position: right 12px center;
      cursor: pointer;
    }
  }

  &__error {
    margin-top: 6px;
    font-size: 0.8rem;
    color: #e74c3c;
    display: flex;
    align-items: center;

    &::before {
      content: "⚠ ";
      margin-right: 5px;
    }
  }

  &__hint {
    margin-top: 6px;
    font-size: 0.8rem;
    color: #7f8c8d;
  }

  &__actions {
    display: flex;
    justify-content: flex-end;
    gap: 12px;
    margin-top: 30px;
    padding-top: 20px;
    border-top: 1px solid #eee;
  }

  &--inline {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;

    .form__group {
      flex: 1;
      min-width: 200px;
    }
  }
}
```

### 2.3 Grille responsive

```scss
.grid {
  display: grid;
  gap: 20px;

  &--2 {
    grid-template-columns: repeat(2, 1fr);
  }

  &--3 {
    grid-template-columns: repeat(3, 1fr);
  }

  &--4 {
    grid-template-columns: repeat(4, 1fr);
  }

  @media (max-width: 768px) {
    &--2,
    &--3,
    &--4 {
      grid-template-columns: 1fr;
    }
  }

  &__item {
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

    &--featured {
      grid-column: span 2;
      border: 2px solid #3498db;
    }

    &--full {
      grid-column: 1 / -1;
    }
  }
}
```

---

## 3. Bonnes pratiques pour l'imbrication

### 3.1 Règle des 3 niveaux maximum

```scss
// ✅ BON : Imbrication raisonnable (2-3 niveaux)
.sidebar {
  padding: 20px;

  .widget {
    margin-bottom: 30px;

    &__title {
      font-size: 1.2rem;
      font-weight: bold;
      margin-bottom: 10px;
    }

    &__content {
      color: #555;
    }
  }
}

// ❌ MAUVAIS : Imbrication excessive (5+ niveaux)
// Cela génère une spécificité énorme et un CSS difficile à maintenir
.page {
  .content {
    .section {
      .article {
        .paragraph {
          color: red;  // Spécificité : .page .content .section .article .paragraph
        }
      }
    }
  }
}
```

### 3.2 Profondeur d'imbrication recommandée

```scss
// ✅ 1 niveau - Très bon
.container {
  max-width: 1200px;
  margin: 0 auto;

  .inner {
    padding: 20px;
  }
}

// ✅ 2 niveaux - Bon
.nav {
  background: #333;

  ul {
    display: flex;

    li {
      margin-right: 10px;
    }
  }
}

// ⚠️ 3 niveaux - Acceptable avec parcimonie
.form {
  padding: 20px;

  .group {
    margin-bottom: 15px;

    label {
      font-weight: bold;
    }
  }
}

// ❌ 4+ niveaux - À éviter
// Cherchez plutôt à restructurer votre code
```

### 3.3 Séparation des responsabilités

```scss
// ✅ BON : Chaque composant a sa propre structure
.button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;

  &--primary {
    background: #3498db;
    color: white;
  }

  &--danger {
    background: #e74c3c;
    color: white;
  }

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}

.alert {
  padding: 15px;
  border-radius: 4px;
  margin-bottom: 10px;

  &--success {
    background: #d4edda;
    color: #155724;
    border: 1px solid #c3e6cb;
  }

  &--error {
    background: #f8d7da;
    color: #721c24;
    border: 1px solid #f5c6cb;
  }
}
```

---

## 4. L'imbrication et la spécificité CSS

### 4.1 Comprendre la spécificité

La **spécificité** détermine quel style s'applique quand plusieurs règles matchent le même élément. Plus la spécificité est élevée, plus le style a de poids.

```scss
// Chaque niveau d'imbrication AJOUTE de la spécificité

// Spécificité : 0-1-0 (un seul sélecteur de classe)
.card { }

// Spécificité : 0-2-0 (deux sélecteurs de classe)
.card .title { }

// Spécificité : 0-3-0 (trois sélecteurs de classe)
.card .title span { }

// Spécificité : 0-4-0 (quatre sélecteurs de classe)
.card .title span strong { }

// Exemple concret de problème
.card {
  .title {
    color: blue;  // Spécificité : 0-2-0
  }
}

// Plus tard, on veut écraser cette couleur
.title {
  color: red;  // Spécificité : 0-1-0 → NE FONCTIONNE PAS !
  // Le style .card .title a plus de poids
}
```

### 4.2 Problèmes concrets de spécificité

```scss
// Scénario problématique
.modal {
  .header {
    padding: 20px;

    h2 {
      color: #333;  // Spécificité : 0-3-0
    }
  }
}

// On veut réutiliser h2 sans imbrication
// Impossible d'écraser sans utiliser !important ou plus d'imbrication

// Solution : Limiter l'imbrication
.modal {
  // Properties directes du modal...
}

.modal__header {
  padding: 20px;
}

.modal__title {
  color: #333;  // Facile à écraser
}

// Ou utiliser BEM pour éviter les problèmes
.card {
  &__title {
    color: #333;  // .card__title → Spécificité : 0-1-0
  }
}
```

### 4.3 Impact sur la maintenance

```scss
// ❌ Code difficile à maintenir (spécificité élevée)
.page-wrapper {
  .main-content {
    .article-list {
      .article-item {
        .article-title {
          font-size: 1.5rem;  // Spécificité : 0-5-0
        }
      }
    }
  }
}

// Pour changer cette taille, il faudra :
// 1. Soit créer un sélecteur encore plus spécifique
// 2. Soit utiliser !important (mauvaise pratique)
// 3. Soit refactoriser tout le CSS

// ✅ Code facile à maintenir
.article-title {
  font-size: 1.5rem;  // Spécificité : 0-1-0
}

// Facile à écraser quand nécessaire
.article-title--large {
  font-size: 2rem;
}
```

---

## 5. Cas d'utilisation avancés

### 5.1 Thème sombre avec variables

```scss
$bg-primary: #ffffff;
$text-primary: #333333;
$bg-secondary: #f5f5f5;

.theme-dark {
  $bg-primary: #1a1a2e;
  $text-primary: #e0e0e0;
  $bg-secondary: #16213e;

  background-color: $bg-primary;
  color: $text-primary;

  .header {
    background-color: $bg-secondary;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);

    .logo {
      filter: invert(1);
    }
  }

  .sidebar {
    background-color: $bg-secondary;

    .link {
      color: $text-primary;

      &:hover {
        background-color: rgba(255, 255, 255, 0.1);
      }
    }
  }
}
```

### 5.2 Composant avec états multiples

```scss
.notification {
  padding: 16px 20px;
  border-radius: 8px;
  margin-bottom: 12px;
  display: flex;
  align-items: flex-start;
  gap: 12px;

  &--info {
    background-color: #e8f4fd;
    border-left: 4px solid #2196f3;
    color: #0d47a1;
  }

  &--success {
    background-color: #e8f5e9;
    border-left: 4px solid #4caf50;
    color: #1b5e20;
  }

  &--warning {
    background-color: #fff3e0;
    border-left: 4px solid #ff9800;
    color: #e65100;
  }

  &--error {
    background-color: #ffebee;
    border-left: 4px solid #f44336;
    color: #b71c1c;
  }

  &__icon {
    font-size: 1.5rem;
    line-height: 1;
    flex-shrink: 0;
  }

  &__content {
    flex: 1;

    &-title {
      font-weight: 600;
      margin-bottom: 4px;
    }

    &-message {
      font-size: 0.9rem;
      opacity: 0.9;
    }
  }

  &__close {
    background: none;
    border: none;
    font-size: 1.2rem;
    cursor: pointer;
    opacity: 0.6;
    transition: opacity 0.2s;

    &:hover {
      opacity: 1;
    }
  }

  // État d'animation
  &.is-entering {
    animation: slideIn 0.3s ease-out;
  }

  &.is-leaving {
    animation: slideOut 0.3s ease-in forwards;
  }
}

@keyframes slideIn {
  from {
    transform: translateX(-100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

@keyframes slideOut {
  from {
    transform: translateX(0);
    opacity: 1;
  }
  to {
    transform: translateX(-100%);
    opacity: 0;
  }
}
```

### 5.3 Layout complexe

```scss
.layout {
  display: grid;
  grid-template-areas:
    "header header header"
    "sidebar main aside"
    "footer footer footer";
  grid-template-columns: 250px 1fr 200px;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;

  @media (max-width: 768px) {
    grid-template-areas:
      "header"
      "main"
      "footer";
    grid-template-columns: 1fr;
  }

  &__header {
    grid-area: header;
    background: #333;
    color: white;
    padding: 16px 24px;
    display: flex;
    justify-content: space-between;
    align-items: center;

    .logo {
      font-size: 1.5rem;
      font-weight: bold;
    }

    .nav {
      display: flex;
      gap: 20px;

      a {
        color: white;
        text-decoration: none;

        &:hover {
          color: #3498db;
        }
      }
    }

    @media (max-width: 768px) {
      flex-direction: column;
      gap: 12px;

      .nav {
        flex-wrap: wrap;
        justify-content: center;
      }
    }
  }

  &__sidebar {
    grid-area: sidebar;
    background: #f8f9fa;
    padding: 20px;
    border-right: 1px solid #dee2e6;

    @media (max-width: 768px) {
      display: none;
    }
  }

  &__main {
    grid-area: main;
    padding: 30px;
  }

  &__aside {
    grid-area: aside;
    background: #f8f9fa;
    padding: 20px;
    border-left: 1px solid #dee2e6;

    @media (max-width: 768px) {
      display: none;
    }
  }

  &__footer {
    grid-area: footer;
    background: #333;
    color: white;
    padding: 20px;
    text-align: center;
  }
}
```

---

## 6. Différence entre imbrication et sélecteurs combinés

### 6.1 Descendant vs Child

```scss
// Descendant selector (par défaut avec l'imbrication)
.nav {
  a {
    color: white;
  }
}
// Équivaut à : .nav a { color: white; }
// Cible TOUS les <a> descendants de .nav

// Pour le child selector (>), on utilise &
.nav {
  > a {
    color: white;
  }
}
// Équivaut à : .nav > a { color: white; }
// Cible uniquement les <a> enfants directs de .nav
```

### 6.2 Adjacent sibling

```scss
// Pour le sélecteur adjacent (+)
.content {
  & + & {
    margin-top: 20px;
  }
}
// Équivaut à : .content + .content { margin-top: 20px; }
```

### 6.3 General sibling

```scss
// Pour le sélecteur sibling (~)
.section {
  & ~ & {
    margin-top: 10px;
  }
}
// Équivaut à : .section ~ .section { margin-top: 10px; }
```

---

## 7. Alternatives à l'imbrication profonde

### 7.1 Utiliser BEM pour réduire l'imbrication

```scss
// ❌ Imbrication profonde
.page {
  .content {
    .sidebar {
      .widget {
        .title {
          font-weight: bold;
        }
      }
    }
  }
}

// ✅ Avec BEM - pas d'imbrication nécessaire
.page-content { }
.sidebar { }
.widget { }
.widget__title {
  font-weight: bold;
}
```

### 7.2 Utiliser des classes utilitaires

```scss
// Au lieu d'imbriquer pour cibler des cas spécifiques
// Utilisez des classes utilitaires

.text-center { text-align: center; }
.text-bold { font-weight: bold; }
.mt-1 { margin-top: 8px; }
.mt-2 { margin-top: 16px; }
.p-1 { padding: 8px; }

// Puis dans votre HTML :
// <h2 class="text-center text-bold mt-2">Titre</h2>
```

### 7.3 Utiliser des mixins pour factoriser

```scss
// Mixin pour les media queries
@mixin respond-to($breakpoint) {
  @if $breakpoint == 'sm' {
    @media (max-width: 576px) { @content; }
  } @else if $breakpoint == 'md' {
    @media (max-width: 768px) { @content; }
  } @else if $breakpoint == 'lg' {
    @media (max-width: 992px) { @content; }
  }
}

// Utilisation sans imbrication profonde
.card {
  padding: 20px;
}

@include respond-to('md') {
  .card {
    padding: 10px;
  }
}
```

---

## 8. Exercices pratiques

### Exercice 1 : Navigation basique

Créez une navigation avec imbrication :

```scss
// Structure HTML :
// <nav class="main-nav">
//   <ul>
//     <li><a href="#">Accueil</a></li>
//     <li><a href="#">Services</a></li>
//     <li><a href="#">Contact</a></li>
//   </ul>
// </nav>

// Style souhaité :
// - nav : fond noir, padding
// - ul : display flex, pas de marge/padding
// - li : margin right
// - a : blanc, pas de soulignement
// - a:hover : couleur bleue
```

### Exercice 2 : Carte produit

```scss
// Créez un composant .product-card avec :
// - Un conteneur avec ombre et bordure arrondie
// - .product-image (hauteur固定, overflow hidden)
// - .product-info avec :
//   - .product-name (gras, 1.2rem)
//   - .product-price (couleur verte, 1.5rem)
//   - .product-rating (étoiles)
// - .product-actions avec un bouton
// Limitez-vous à 3 niveaux d'imbrication maximum
```

### Exercice 3 : Formulaire de contact

```scss
// Créez un formulaire avec imbrication :
// - .contact-form (max-width: 500px, margin auto)
// - .form-row (flex, gap)
// - .form-field (margin-bottom)
//   - label (display block, margin-bottom)
//   - input (width 100%, padding, border)
//   - input:focus (border-color bleu)
//   - .error-message (petit, rouge)
// - .submit-btn (pleine largeur, fond bleu)
```

### Exercice 4 : Layout page

```scss
// Créez un layout complet :
// - .page-header (flex, space-between, padding)
//   - .logo
//   - .main-nav > ul > li > a
// - .page-content (flex, gap)
//   - .sidebar (width: 250px)
//   - .main-content (flex: 1)
//     - .article > h2 + p + .read-more
// - .page-footer (background sombre, text-center)
//     - .footer-links > a (margin)
//     - .copyright
```

### Exercice 5 : Composant avec états

```scss
// Créez un bouton .toggle-switch avec :
// - Conteneur (relative, width, height)
// - .toggle-track (absolute, background gris)
// - .toggle-thumb (absolute, blanc, border-radius)
// - .toggle-track.is-active (background bleu)
// - .toggle-track.is-active + .toggle-thumb (transform translate)
// - États disabled et focus
// - Animation de transition
```

### Exercice 6 : Analyse de spécificité

```scss
// Analysez cette imbrication et calculez la spécificité de chaque sélecteur :
.page {
  .content {
    .article {
      .title { font-size: 2rem; }
      .meta { color: gray; }
    }
  }
}

// Puis refactorisez pour réduire la spécificité
// tout en gardant le même rendu visuel
```

---

## Résumé

| Concept | Description |
|---------|-------------|
| Imbrication simple | Sélecteur dans sélecteur |
| Profondeur max | 2-3 niveaux recommandés |
| Spécificité | Augmente avec chaque niveau |
| Descendant | `parent { child {} }` |
| Child | `parent { > child {} }` |
| Bonne pratique | Utiliser BEM pour réduire l'imbrication |
| Alternative | Classes utilitaires, mixins |

---

## Chapitre suivant

Dans le [Chapitre 6 - Le parent selector (&)](chapitre-06-parent-selector.md), nous verrons comment utiliser le sélecteur parent `&` pour créer des variantes, des pseudo-classes et des combinaisons avancées.
