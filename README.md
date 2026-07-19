# Cours Complet SASS/SCSS — Bonnes Pratiques

---

## Bienvenue

Ce cours est un guide complet et structuré pour apprendre **SASS/SCSS**, depuis les bases jusqu'à la maîtrise avancée. Chaque niveau est divisé en chapitres clairs, avec des exemples de code concrets et des exercices pratiques.

Que vous soyez débutant ou développeur expérimenté cherchant à formaliser vos connaissances, ce cours vous fournira les outils et les méthodes pour écrire du CSS professionnel, maintenable et performant.

---

## Prérequis

Avant de commencer ce cours, vous devez maîtriser les bases suivantes :

- **HTML** :结构, sémantique, balisage de base
- **CSS** : sélecteurs, propriétés, valeurs, box model, positionnement
- **Un éditeur de code** : VS Code (recommandé), Sublime Text, ou tout autre éditeur moderne
- **Un terminal** : notions de base de ligne de commande (navigation, exécution de commandes)
- **Node.js** (optionnel mais recommandé) : pour l'installation et la compilation

Si vous débutez avec le CSS, je vous recommande de suivre un cours de base en CSS avant de vous lancer dans SASS.

---

## Comment utiliser ce cours

1. **Suivez l'ordre des niveaux** : chaque niveau builds sur le précédent
2. **Lisez chaque chapitre** attentivement et étudiez les exemples
3. **Faites les exercices** : c'est en pratiquant qu'on apprend le mieux
4. **Créez vos propres projets** pour tester ce que vous apprenez
5. **Revoyez les chapitres** si nécessaire — la répétition est la clé

### Convention de notation

- 🔹 NiveauFacile
- 🔸 Niveau Moyen
- 🔴 NiveauAvancé

### Structure des fichiers

Chaque niveau possède son propre dossier contenant les chapitres associés. Les fichiers sont nommés selon le schéma `chapitre-XX-nom-du-chapitre.md`.

---

## Table des matières

### Niveau 1 — Introduction à SASS
| Chapitre | Titre |
|----------|-------|
| 1 | [Découvrir SASS](niveau-01-introduction/chapitre-01-decouvrir-sass.md) |
| 2 | [Installation](niveau-01-introduction/chapitre-02-installation.md) |
| 3 | [Premier fichier SCSS](niveau-01-introduction/chapitre-03-premier-fichier.md) |

### Niveau 2 — Les bases
| Chapitre | Titre |
|----------|-------|
| 4 | Variables |
| 5 | Imbrication (Nesting) |
| 6 | Opérateurs arithmétiques |
| 7 | Partials et imports |

### Niveau 3 — Organisation
| Chapitre | Titre |
|----------|-------|
| 8 | Structure de dossiers |
| 9 | Fichiers de configuration |
| 10 | Convention de noms |
| 11 | Le système 7-1 |

### Niveau 4 — Mixins
| Chapitre | Titre |
|----------|-------|
| 12 | Définir des mixins |
| 13 | Arguments et valeurs par défaut |
| 14 | Mixins avancés et cas d'usage |

### Niveau 5 — Structures de données
| Chapitre | Titre |
|----------|-------|
| 15 | Listes |
| 16 | Maps |
| 17 | Itérations (boucles) |

### Niveau 6 — Conditions
| Chapitre | Titre |
|----------|-------|
| 18 | `@if`, `@else if`, `@else` |

### Niveau 7 — Modules intégrés
| Chapitre | Titre |
|----------|-------|
| 19 | `sass:color` |
| 20 | `sass:math` |
| 21 | `sass:meta` |
| 22 | `sass:selector` |
| 23 | `sass:string` |
| 24 | `sass:list` |
| 25 | `sass:map` |

### Niveau 8 — Architecture
| Chapitre | Titre |
|----------|-------|
| 26 | Principes d'architecture |
| 27 | Méthodologies (BEM, SMACSS, OOCSS) |
| 28 | Bonnes pratiques d'organisation |

### Niveau 9 — Responsive
| Chapitre | Titre |
|----------|-------|
| 29 | Media queries avec SASS |
| 30 | Breakpoints et mixins |
| 31 | Stratégies responsive avancées |

### Niveau 10 — Animations
| Chapitre | Titre |
|----------|-------|
| 32 | Keyframes avec SASS |
| 33 | Transitions dynamiques |
| 34 | Animations complexes |

### Niveau 11 — Optimisation
| Chapitre | Titre |
|----------|-------|
| 35 | Minification et compression |
| 36 | Réduction du CSS généré |
| 37 | Performance et loading |

### Niveau 12 — Bonnes pratiques
| Chapitre | Titre |
|----------|-------|
| 38 | Conventions de codage |
| 39 | Documentation |
| 40 | Réutilisabilité |

### Niveau 13 — Sécurité et qualité du code
| Chapitre | Titre |
|----------|-------|
| 41 | Validation et linting |
| 42 | Tests de CSS |
| 43 | CI/CD pour le CSS |

### Niveau 14 — Projet final
| Chapitre | Titre |
|----------|-------|
| 44 | Cahier des charges |
| 45 | Réalisation |
| 46 | Révision et optimisation |

---

## Structure des fichiers du cours

```
cours-sass/
├── README.md
├── niveau-01-introduction/
│   ├── chapitre-01-decouvrir-sass.md
│   ├── chapitre-02-installation.md
│   └── chapitre-03-premier-fichier.md
├── niveau-02-les-bases/
│   ├── chapitre-04-variables.md
│   ├── chapitre-05-imbrication.md
│   ├── chapitre-06-operateurs.md
│   └── chapitre-07-partials-imports.md
├── niveau-03-organisation/
│   ├── chapitre-08-structure-dossiers.md
│   ├── chapitre-09-fichiers-configuration.md
│   ├── chapitre-10-convention-noms.md
│   └── chapitre-11-systeme-7-1.md
├── niveau-04-mixins/
│   ├── chapitre-12-definir-mixins.md
│   ├── chapitre-13-arguments.md
│   └── chapitre-14-mixins-avances.md
├── niveau-05-structures-donnees/
│   ├── chapitre-15-listes.md
│   ├── chapitre-16-maps.md
│   └── chapitre-17-iterations.md
├── niveau-06-conditions/
│   └── chapitre-18-conditions.md
├── niveau-07-modules-integres/
│   ├── chapitre-19-sass-color.md
│   ├── chapitre-20-sass-math.md
│   ├── chapitre-21-sass-meta.md
│   ├── chapitre-22-sass-selector.md
│   ├── chapitre-23-sass-string.md
│   ├── chapitre-24-sass-list.md
│   └── chapitre-25-sass-map.md
├── niveau-08-architecture/
│   ├── chapitre-26-principes-architecture.md
│   ├── chapitre-27-methodologies.md
│   └── chapitre-28-bonnes-pratiques-organisation.md
├── niveau-09-responsive/
│   ├── chapitre-29-media-queries.md
│   ├── chapitre-30-breakpoints.md
│   └── chapitre-31-strategies-responsive.md
├── niveau-10-animations/
│   ├── chapitre-32-keyframes.md
│   ├── chapitre-33-transitions.md
│   └── chapitre-34-animations-complexes.md
├── niveau-11-optimisation/
│   ├── chapitre-35-minification.md
│   ├── chapitre-36-reduction-css.md
│   └── chapitre-37-performance.md
├── niveau-12-bonnes-pratiques/
│   ├── chapitre-38-conventions-codage.md
│   ├── chapitre-39-documentation.md
│   └── chapitre-40-reutilisabilite.md
├── niveau-13-securite-qualite/
│   ├── chapitre-41-validation-linting.md
│   ├── chapitre-42-tests-css.md
│   └── chapitre-43-cicd-css.md
└── niveau-14-projet-final/
    ├── chapitre-44-cahier-des-charges.md
    ├── chapitre-45-realisation.md
    └── chapitre-46-revision-optimisation.md
```

---

## Ressources complémentaires

- [Documentation officielle SASS](https://sass-lang.com/documentation/)
- [Sass Guidelines](https://sass-guidelin.es/fr/)
- [Angelmanstyle CSS](https://www.angrytools.com/css/)

---

## Licence

Ce cours est distribué à des fins éducatives. Vous êtes libre de le partager et de le modifier selon vos besoins.

---

*Bon apprentissage ! 🎨*
