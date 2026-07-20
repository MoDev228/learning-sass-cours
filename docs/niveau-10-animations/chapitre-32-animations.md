# Chapitre 32 — Animations en SCSS

## Introduction

Les animations rendent une interface vivante et interactive. SCSS permet de créer des systèmes d'animation robustes avec des variables, des mixins et des bibliothèques réutilisables. Ce chapitre couvre les transitions, les keyframes, et la création d'une bibliothèque d'animation complète.

---

## Variables d'animation

### Configuration de base

```scss
// tokens/_animation.scss

// === Durées ===
$duration-instant: 0ms !default;
$duration-fast: 150ms !default;
$duration-normal: 300ms !default;
$duration-base: 400ms !default;
$duration-slow: 600ms !default;
$duration-slower: 800ms !default;
$duration-slowest: 1200ms !default;

$duration-map: (
  instant: $duration-instant,
  fast: $duration-fast,
  normal: $duration-normal,
  base: $duration-base,
  slow: $duration-slow,
  slower: $duration-slower,
  slowest: $duration-slowest,
) !default;

// === Courbes d'accélération ===
$ease-default: cubic-bezier(0.25, 0.1, 0.25, 1) !default;
$ease-in: cubic-bezier(0.42, 0, 1, 1) !default;
$ease-out: cubic-bezier(0, 0, 0.58, 1) !default;
$ease-in-out: cubic-bezier(0.42, 0, 0.58, 1) !default;
$ease-bounce: cubic-bezier(0.68, -0.55, 0.265, 1.55) !default;
$ease-elastic: cubic-bezier(0.175, 0.885, 0.32, 1.275) !default;
$ease-snap: cubic-bezier(0.55, 0.085, 0.68, 0.53) !default;
$ease-smooth: cubic-bezier(0.23, 1, 0.32, 1) !default;

$ease-map: (
  default: $ease-default,
  in: $ease-in,
  out: $ease-out,
  'in-out': $ease-in-out,
  bounce: $ease-bounce,
  elastic: $ease-elastic,
  snap: $ease-snap,
  smooth: $ease-smooth,
) !default;

// === Délais ===
$delay-none: 0ms !default;
$delay-short: 50ms !default;
$delay-medium: 100ms !default;
$delay-long: 200ms !default;

// === Propriétés animées ===
$animated-properties: (
  colors: color, background-color, border-color, box-shadow,
  transform: transform,
  opacity: opacity,
  dimensions: width, height, max-width, max-height,
  spacing: padding, margin, gap,
  typography: font-size, line-height, letter-spacing,
  shadow: box-shadow,
  border: border, border-radius,
  outline: outline, outline-offset,
  visibility: visibility,
  filter: filter, backdrop-filter,
  all: all,
) !default;
```

---

## Mixins de transition

### Mixin principal

```scss
// mixins/_transition.scss

@mixin transition(
  $properties: $animated-properties.all,
  $duration: $duration-base,
  $easing: $ease-in-out,
  $delay: $delay-none
) {
  transition-property: $properties;
  transition-duration: $duration;
  transition-timing-function: $easing;
  transition-delay: $delay;
}

// Variantes rapides
@mixin transition-fast($properties: $animated-properties.all) {
  @include transition($properties, $duration-fast, $ease-out);
}

@mixin transition-colors {
  @include transition(
    $animated-properties.colors,
    $duration-normal,
    $ease-in-out
  );
}

@mixin transition-transform($duration: $duration-normal, $easing: $ease-out) {
  @include transition($animated-properties.transform, $duration, $easing);
}

@mixin transition-shadow {
  @include transition($animated-properties.shadow, $duration-normal, $ease-in-out);
}

@mixin transition-opacity($duration: $duration-normal) {
  @include transition($animated-properties.opacity, $duration, $ease-in-out);
}
```

### Mixin multi-propriétés

```scss
@mixin transitions($transitions...) {
  $result: ();
  $length: length($transitions);

  @for $i from 1 through $length {
    $transition: nth($transitions, $i);
    $prop: nth($transition, 1);
    $dur: if(length($transition) > 1, nth($transition, 2), $duration-base);
    $ease: if(length($transition) > 2, nth($transition, 3), $ease-in-out);
    $del: if(length($transition) > 4, nth($transition, 5), $delay-none);

    $result: append($result, $prop $dur $ease $del, comma);
  }

  transition: $result;
}

// Utilisation
.card {
  @include transitions(
    (transform, $duration-normal, $ease-out),
    (box-shadow, $duration-slow, $ease-in-out),
    (opacity, $duration-fast)
  );
}
```

### Résultat CSS

```css
.card {
  transition-property: transform, box-shadow, opacity;
  transition-duration: 400ms, 600ms, 150ms;
  transition-timing-function: cubic-bezier(0, 0, 0.58, 1), cubic-bezier(0.42, 0, 0.58, 1), cubic-bezier(0.42, 0, 0.58, 1);
  transition-delay: 0ms, 0ms, 0ms;
}
```

---

## @keyframes avec SCSS

### Système de keyframes configurable

```scss
// mixins/_keyframes.scss

@mixin keyframes($name) {
  @-webkit-keyframes #{$name} {
    @content;
  }
  @keyframes #{$name} {
    @content;
  }
}

// Animation générique
@mixin animate($name, $duration: $duration-base, $easing: $ease-in-out, $fill-mode: both) {
  animation-name: $name;
  animation-duration: $duration;
  animation-timing-function: $easing;
  animation-fill-mode: $fill-mode;
  animation-iteration-count: 1;
}

@mixin animate-infinite($name, $duration: $duration-base, $easing: $ease-in-out) {
  @include animate($name, $duration, $easing, none);
  animation-iteration-count: infinite;
}

@mixin animate-paused {
  animation-play-state: paused;
}

@mixin animate-running {
  animation-play-state: running;
}
```

---

## Bibliothèque d'animations

### 1. Fade (fondu)

```scss
// animations/_fade.scss

@include keyframes(fade-in) {
  from { opacity: 0; }
  to { opacity: 1; }
}

@include keyframes(fade-out) {
  from { opacity: 1; }
  to { opacity: 0; }
}

@include keyframes(fade-in-up) {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@include keyframes(fade-in-down) {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@include keyframes(fade-in-left) {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@include keyframes(fade-in-right) {
  from {
    opacity: 0;
    transform: translateX(20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

// Classes utilitaires
.animate-fade-in { @include animate(fade-in); }
.animate-fade-in-up { @include animate(fade-in-up); }
.animate-fade-in-down { @include animate(fade-in-down); }
.animate-fade-in-left { @include animate(fade-in-left); }
.animate-fade-in-right { @include animate(fade-in-right); }
```

### 2. Slide (glissement)

```scss
// animations/_slide.scss

@include keyframes(slide-in-up) {
  from { transform: translateY(100%); }
  to { transform: translateY(0); }
}

@include keyframes(slide-in-down) {
  from { transform: translateY(-100%); }
  to { transform: translateY(0); }
}

@include keyframes(slide-in-left) {
  from { transform: translateX(-100%); }
  to { transform: translateX(0); }
}

@include keyframes(slide-in-right) {
  from { transform: translateX(100%); }
  to { transform: translateX(0); }
}

@include keyframes(slide-out-up) {
  from { transform: translateY(0); }
  to { transform: translateY(-100%); }
}

@include keyframes(slide-out-down) {
  from { transform: translateY(0); }
  to { transform: translateY(100%); }
}

// Classes
.animate-slide-in-up { @include animate(slide-in-up); }
.animate-slide-in-down { @include animate(slide-in-down); }
.animate-slide-in-left { @include animate(slide-in-left); }
.animate-slide-in-right { @include animate(slide-in-right); }
```

### 3. Scale (zoom)

```scss
// animations/_scale.scss

@include keyframes(scale-in) {
  from {
    opacity: 0;
    transform: scale(0.9);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

@include keyframes(scale-out) {
  from {
    opacity: 1;
    transform: scale(1);
  }
  to {
    opacity: 0;
    transform: scale(0.9);
  }
}

@include keyframes(pulse) {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

@include keyframes(ping) {
  0% {
    transform: scale(1);
    opacity: 1;
  }
  75%, 100% {
    transform: scale(2);
    opacity: 0;
  }
}

// Classes
.animate-scale-in { @include animate(scale-in); }
.animate-scale-out { @include animate(scale-out); }
.animate-pulse { @include animate-infinite(pulse, 2s); }
```

### 4. Bounce (rebond)

```scss
// animations/_bounce.scss

@include keyframes(bounce) {
  0%, 20%, 53%, 80%, 100% {
    animation-timing-function: $ease-in-out;
    transform: translateY(0);
  }
  40%, 43% {
    animation-timing-function: $ease-in-out;
    transform: translateY(-30px);
  }
  70% {
    animation-timing-function: $ease-in-out;
    transform: translateY(-15px);
  }
  90% {
    animation-timing-function: $ease-in-out;
    transform: translateY(-4px);
  }
}

@include keyframes(bounce-in) {
  0% {
    opacity: 0;
    transform: scale(0.3);
  }
  50% {
    opacity: 1;
    transform: scale(1.05);
  }
  70% {
    transform: scale(0.9);
  }
  100% {
    transform: scale(1);
  }
}

@include keyframes(bounce-out) {
  0% { transform: scale(1); }
  25% { transform: scale(0.95); }
  50% { opacity: 1; transform: scale(1.1); }
  100% { opacity: 0; transform: scale(0.3); }
}

// Classes
.animate-bounce { @include animate-infinite(bounce, 1s); }
.animate-bounce-in { @include animate(bounce-in); }
```

### 5. Spin (rotation)

```scss
// animations/_spin.scss

@include keyframes(spin) {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

@include keyframes(spin-reverse) {
  from { transform: rotate(360deg); }
  to { transform: rotate(0deg); }
}

@include keyframes(flip) {
  0% { transform: perspective(400px) rotateY(0); }
  40% { transform: perspective(400px) rotateY(-180deg); }
  100% { transform: perspective(400px) rotateY(-360deg); }
}

@include keyframes(wiggle) {
  0%, 100% { transform: rotate(0deg); }
  25% { transform: rotate(-5deg); }
  75% { transform: rotate(5deg); }
}

// Classes
.animate-spin { @include animate-infinite(spin, 1s, $ease-linear); }
.animate-spin-reverse { @include animate-infinite(spin-reverse, 1s, $ease-linear); }
.animate-flip { @include animate(flip, 0.6s); }
.animate-wiggle { @include animate-infinite(wiggle, 0.5s); }
```

### 6. Shake (secousse)

```scss
// animations/_shake.scss

@include keyframes(shake) {
  0%, 100% { transform: translateX(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
  20%, 40%, 60%, 80% { transform: translateX(5px); }
}

@include keyframes(shake-y) {
  0%, 100% { transform: translateY(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateY(-5px); }
  20%, 40%, 60%, 80% { transform: translateY(5px); }
}

@include keyframes(shake-hard) {
  0%, 100% { transform: translateX(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateX(-10px); }
  20%, 40%, 60%, 80% { transform: translateX(10px); }
}

// Classes
.animate-shake { @include animate(shake, 0.5s); }
.animate-shake-y { @include animate(shake-y, 0.5s); }
```

### 7. Flash & Glow

```scss
// animations/_flash.scss

@include keyframes(flash) {
  0%, 50%, 100% { opacity: 1; }
  25%, 75% { opacity: 0; }
}

@include keyframes(glow) {
  0%, 100% { box-shadow: 0 0 5px rgba($color-primary, 0.5); }
  50% { box-shadow: 0 0 20px rgba($color-primary, 0.8), 0 0 40px rgba($color-primary, 0.4); }
}

@include keyframes(shimmer) {
  0% { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}

// Skeleton loading
.skeleton {
  background: linear-gradient(
    90deg,
    $color-gray-200 25%,
    $color-gray-100 50%,
    $color-gray-200 75%
  );
  background-size: 200% 100%;
  @include animate-infinite(shimmer, 1.5s, $ease-in-out);
}

// Classes
.animate-flash { @include animate-infinite(flash, 1.5s); }
.animate-glow { @include animate-infinite(glow, 2s); }
```

### 8. Entrance combinées

```scss
// animations/_entrance.scss

@include keyframes(zoom-in) {
  from {
    opacity: 0;
    transform: scale(0.5);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

@include keyframes(slide-up-fade) {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@include keyframes(blur-in) {
  from {
    opacity: 0;
    filter: blur(10px);
  }
  to {
    opacity: 1;
    filter: blur(0);
  }
}

@include keyframes(flip-in-x) {
  from {
    opacity: 0;
    transform: perspective(400px) rotateX(90deg);
  }
  to {
    opacity: 1;
    transform: perspective(400px) rotateX(0);
  }
}

@include keyframes(flip-in-y) {
  from {
    opacity: 0;
    transform: perspective(400px) rotateY(90deg);
  }
  to {
    opacity: 1;
    transform: perspective(400px) rotateY(0);
  }
}

// Classes
.animate-zoom-in { @include animate(zoom-in); }
.animate-slide-up-fade { @include animate(slide-up-fade, $duration-slow); }
.animate-blur-in { @include animate(blur-in, $duration-slower); }
.animate-flip-in-x { @include animate(flip-in-x); }
.animate-flip-in-y { @include animate(flip-in-y); }
```

---

## Système de delays pour animations séquentielles

```scss
// mixins/_stagger.scss

@mixin stagger($count: 10, $delay: 50ms, $base-delay: 0ms) {
  @for $i from 1 through $count {
    &:nth-child(#{$i}) {
      animation-delay: $base-delay + (($i - 1) * $delay);
    }
  }
}

// Alternative : delai par data-attribute
@mixin stagger-by-attr($attr: 'data-delay', $multiplier: 50ms) {
  [style*="#{$attr}"] {
    animation-delay: calc(var(#{$attr}) * #{$multiplier / 1ms}ms);
  }
}

// Classes de delay
@for $i from 0 through 10 {
  .delay-#{$i} {
    animation-delay: #{$i * 50}ms;
  }
}

// Utilisation
.stagger-list {
  .stagger-item {
    @include animate(fade-in-up);
    opacity: 0;
    animation-fill-mode: forwards;
    @include stagger(10, 100ms);
  }
}
```

---

## Animations au scroll (Intersection Observer)

```scss
// animations/_scroll.scss

// État initial avant animation
.animate-on-scroll {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity $duration-slow $ease-out,
              transform $duration-slow $ease-out;

  // Quand l'élément est visible
  &.is-visible {
    opacity: 1;
    transform: translateY(0);
  }

  // Variante depuis la gauche
  &--left {
    transform: translateX(-30px);

    &.is-visible {
      transform: translateX(0);
    }
  }

  // Variante depuis la droite
  &--right {
    transform: translateX(30px);

    &.is-visible {
      transform: translateX(0);
    }
  }

  // Variante scale
  &--scale {
    transform: scale(0.9);

    &.is-visible {
      transform: scale(1);
    }
  }

  // Variante rotate
  &--rotate {
    transform: rotate(-5deg) scale(0.95);

    &.is-visible {
      transform: rotate(0) scale(1);
    }
  }
}

// Délais pour le stagger
@for $i from 1 through 12 {
  .animate-delay-#{$i} {
    transition-delay: #{$i * 100}ms;
  }
}
```

### JavaScript associé

```javascript
// Permet de déclencher les animations au scroll
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('is-visible');
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll('.animate-on-scroll').forEach(el => {
  observer.observe(el);
});
```

---

## Animations pour le dark mode

```scss
// animations/_theme-transition.scss

.theme-transition,
.theme-transition *,
.theme-transition *::before,
.theme-transition *::after {
  transition:
    background-color $duration-base $ease-in-out,
    color $duration-base $ease-in-out,
    border-color $duration-base $ease-in-out,
    box-shadow $duration-base $ease-in-out,
    fill $duration-base $ease-in-out !important;
}
```

---

## Animations de chargement (Loading)

```scss
// animations/_loading.scss

// Spinner
@include keyframes(spinner) {
  to { transform: rotate(360deg); }
}

.loader-spinner {
  width: 40px;
  height: 40px;
  border: 4px solid $color-gray-200;
  border-top-color: $color-primary;
  border-radius: 50%;
  @include animate-infinite(spinner, 0.8s, $ease-linear);
}

// Dots
@include keyframes(dot-pulse) {
  0%, 80%, 100% { transform: scale(0.6); opacity: 0.5; }
  40% { transform: scale(1); opacity: 1; }
}

.loader-dots {
  display: flex;
  gap: $spacing-xs;

  span {
    width: 10px;
    height: 10px;
    background-color: $color-primary;
    border-radius: 50%;
    @include animate-infinite(dot-pulse, 1.4s, $ease-in-out, infinite);
    animation-fill-mode: both;

    &:nth-child(1) { animation-delay: -0.32s; }
    &:nth-child(2) { animation-delay: -0.16s; }
    &:nth-child(3) { animation-delay: 0s; }
  }
}

// Bar
@include keyframes(bar-loading) {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}

.loader-bar {
  width: 200px;
  height: 4px;
  background-color: $color-gray-200;
  border-radius: $border-radius-full;
  overflow: hidden;

  &__fill {
    width: 100%;
    height: 100%;
    background-color: $color-primary;
    @include animate-infinite(bar-loading, 1.5s, $ease-in-out);
  }
}
```

---

## Animations accessibles

```scss
// animations/_accessibility.scss

// Respect de prefers-reduced-motion
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}

// Classe pour désactiver les animations
.no-animations,
.no-animations *,
.no-animations *::before,
.no-animations *::after {
  animation: none !important;
  transition: none !important;
}
```

---

## Organisation des fichiers

```
sass/
├── tokens/
│   └── _animation.scss
├── mixins/
│   ├── _transition.scss
│   ├── _keyframes.scss
│   └── _stagger.scss
├── animations/
│   ├── _fade.scss
│   ├── _slide.scss
│   ├── _scale.scss
│   ├── _bounce.scss
│   ├── _spin.scss
│   ├── _shake.scss
│   ├── _flash.scss
│   ├── _entrance.scss
│   ├── _scroll.scss
│   ├── _loading.scss
│   └── _accessibility.scss
└── main.scss
```

---

## Résumé

Les animations en SCSS deviennent un système maintenable grâce aux variables (durées, easings), aux mixins (transition, keyframes) et à une bibliothèque structurée. En combinant des animations CSS pures avec des patterns comme le scroll-trigger et le stagger, on crée des interfaces dynamiques et performantes. Toujours respecter `prefers-reduced-motion` pour l'accessibilité.
