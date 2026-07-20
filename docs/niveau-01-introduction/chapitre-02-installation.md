# Chapitre 2 — Installation

> **Niveau** : Débutant | **Durée estimée** : 30 minutes

---

## Objectifs du chapitre

À l'issue de ce chapitre, vous serez capable de :

- Installer Node.js sur votre machine
- Installer SASS avec npm ou pnpm
- Comprendre la différence entre installation globale et locale
- Compiler un fichier SCSS en CSS
- Utiliser le mode surveillance (watch)
- Générer du CSS minifié et des source maps

---

## 1. Installer Node.js

SASS (la version actuelle, Dart Sass) est distribué via **npm**, le gestionnaire de paquets de Node.js. Il est donc nécessaire d'avoir Node.js installé.

### Vérifier si Node.js est déjà installé

Ouvrez votre terminal et tapez :

```bash
node --version
```

Si vous voyez un numéro de version (ex: `v20.11.0`), Node.js est déjà installé. Sinon, vous devez l'installer.

### Installation de Node.js

**Sur Linux (Ubuntu/Debian) :**

```bash
# Méthode 1 : Via le dépôt officiel
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Méthode 2 : Via nvm (recommandé)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install 20
nvm use 20
```

**Sur macOS :**

```bash
# Via Homebrew
brew install node

# Ou téléchargez l'installeur sur https://nodejs.org
```

**Sur Windows :**

```bash
# Téléchargez l'installeur MSI depuis https://nodejs.org
# Ou utilisez winget
winget install OpenJS.NodeJS.LTS
```

### Vérifier l'installation

```bash
node --version    # Affiche la version de Node.js
npm --version     # Affiche la version de npm
```

Sortie attendue :
```
v20.11.0
10.2.4
```

---

## 2. Installer SASS avec npm

**npm** (Node Package Manager) est le gestionnaire de paquets inclus avec Node.js.

### Installation globale

L'installation globale permet d'utiliser la commande `sass` n'importe où sur votre machine :

```bash
npm install -g sass
```

> **Note** : Sur certaines configurations Linux, vous devrez utiliser `sudo` :
> ```bash
> sudo npm install -g sass
> ```

### Vérifier l'installation

```bash
sass --version
```

Sortie attendue :
```
Sass 1.77.8
```

---

## 3. Installer SASS avec pnpm

**pnpm** est un gestionnaire de paquets alternatif à npm, plus rapide et qui économise de l'espace disque.

### Installer pnpm d'abord

```bash
npm install -g pnpm
```

### Installer SASS avec pnpm

```bash
# Installation globale
pnpm add -g sass
```

### Vérifier l'installation

```bash
sass --version
```

---

## 4. Installation globale vs locale

Il existe deux façons d'installer SASS, et le choix a des implications importantes.

### Installation globale

```bash
npm install -g sass
```

- La commande `sass` est disponible **partout** dans votre terminal
- Utile pour les projets personnels et l'apprentissage
- **Inconvénient** : la version de SASS peut différer entre les développeurs d'une équipe

### Installation locale (recommandée pour les projets)

```bash
# Dans le dossier de votre projet
npm init -y          # Crée un fichier package.json
npm install --save-dev sass
```

- SASS est installé uniquement dans le projet courant
- La version est **verrouillée** dans `package.json` et `package-lock.json`
- Toute l'équipe utilise la **même version**

### Utilisation après installation locale

Après une installation locale, vous pouvez exécuter SASS de deux façons :

**Option 1 — Via npx :**
```bash
npx sass input.scss output.css
```

**Option 2 — Via un script npm :**

Ajoutez dans `package.json` :
```json
{
  "scripts": {
    "build": "sass input.scss output.css",
    "watch": "sass --watch input.scss:output.css"
  }
```

Puis utilisez :
```bash
npm run build    # Compilation
npm run watch    # Mode surveillance
```

---

## 5. Vérifier l'installation

Exécutez ces commandes pour tout vérifier :

```bash
# Vérifier Node.js
node --version

# Vérifier npm
npm --version

# Vérifier SASS
sass --version

# Vérifier que la commande fonctionne
sass --help
```

Si toutes les commandes fonctionnent, vous êtes prêt !

---

## 6. Première compilation

Créez un fichier de test pour vérifier que tout fonctionne.

### Étape 1 : Créer le fichier SCSS

Créez un dossier pour votre projet, puis un fichier `style.scss` :

```bash
mkdir mon-projet-sass
cd mon-projet-sass
```

Créez le fichier `style.scss` avec ce contenu :

```scss
$primary-color: #3498db;
$font-stack: 'Helvetica', sans-serif;

body {
  font-family: $font-stack;
  color: #333;
}

h1 {
  color: $primary-color;
  font-size: 2em;
}

.button {
  background-color: $primary-color;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;

  &:hover {
    background-color: darken($primary-color, 10%);
  }
}
```

### Étape 2 : Compiler

```bash
sass style.scss style.css
```

### Étape 3 : Vérifier le résultat

Ouvrez le fichier `style.css` généré :

```css
body {
  font-family: "Helvetica", sans-serif;
  color: #333;
}

h1 {
  color: #3498db;
  font-size: 2em;
}

.button {
  background-color: #3498db;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.button:hover {
  background-color: #2980b9;
}

/*# sourceMappingURL=style.css.map */
```

**Félicitations !** Votre premier fichier SCSS a été compilé avec succès.

---

## 7. Mode surveillance (watch)

Le mode surveillance surveille votre fichier SCSS et recompile automatiquement à chaque modification.

### Syntaxe

```bash
sass --watch input.scss:output.css
```

### Exemple concret

```bash
sass --watch style.scss:style.css
```

Sortie :
```
Compiled style.scss to style.css.
Sass is watching for changes. Press Ctrl-C to stop.
```

Désormains, chaque fois que vous modifiez `style.scss`, le fichier `style.css` est automatiquement régénéré. Plus besoin de recompiler manuellement !

### Arrêter la surveillance

Appuyez sur `Ctrl + C` dans le terminal.

### Surveillance de dossiers entiers

Vous pouvez surveiller un dossier complet :

```bash
sass --watch src/scss:dist/css
```

Cela compilerait tous les fichiers `.scss` du dossier `src/scss/` vers le dossier `dist/css/`.

---

## 8. CSS minifié (mode compressed)

En production, vous voulez un CSS le plus petit possible pour optimiser les performances de chargement.

### Générer du CSS minifié

```bash
sass --style=compressed style.scss style.min.css
```

### Comparaison

**CSS normal (expanded) :**
```css
.button {
  background-color: #3498db;
  color: white;
  padding: 10px 20px;
}
```

**CSS minifié (compressed) :**
```css
.button{background-color:#3498db;color:#fff;padding:10px 20px}
```

### Les différents styles de sortie

| Style | Commande | Description |
|-------|----------|-------------|
| `expanded` (défaut) | `--style=expanded` | CSS lisible, un bloc par ligne |
| `compressed` | `--style=compressed` | CSS minifié, tout sur une ligne |
| `compact` | `--style=compact` | Une propriété par ligne |
| `crunched` | `--style=crunched` | Sans espaces ni sauts de ligne |

### Recommandation

- **Développement** : utilisez `expanded` (défaut) pour la lisibilité
- **Production** : utilisez `compressed` pour la performance

---

## 9. Génération de Source Maps

Les **source maps** sont des fichiers qui permettent de tracer le CSS généré vers le code SCSS original. Ils sont essentiels pour le débogage.

### Qu'est-ce qu'un source map ?

Quand vous ouvrez les outils de développement du navigateur et inspectez un élément, le source map vous permet de voir le **fichier SCSS original** plutôt que le CSS compilé.

```
CSS généré          Source Map           SCSS original
.button {     ←──  style.css.map  ──→  .button {
  padding: 10px;                         padding: $spacing;
}                                      }
```

### Générer un source map

Par défaut, SASS génère un source map :

```bash
sass style.scss style.css
```

Cela crée automatiquement :
- `style.css` — le CSS compilé
- `style.css.map` — le source map

### Désactiver les source maps

```bash
sass --no-source-map style.scss style.css
```

### Intégration avec les navigateurs

1. Ouvrez les **Outils de développement** (F12 ou Ctrl+Shift+I)
2. Onglet **Sources** ou **Elements**
3. Survolez ou inspectez un élément CSS
4. Le navigateur affichera le fichier `.scss` original grâce au source map

> **Important** : En production, désactivez les source maps pour ne pas exposer votre code source.

---

## 10. Commandes utiles — Récapitulatif

```bash
# Compilation de base
sass input.scss output.css

# Mode surveillance
sass --watch input.scss:output.css

# CSS minifié
sass --style=compressed input.scss output.min.css

# Sans source map
sass --no-source-map input.scss output.css

# Avec source map (par défaut)
sass input.scss output.css --source-map

# Surveillance de dossiers
sass --watch src/scss:dist/css

# Aide
sass --help

# Version
sass --version
```

---

## 11. Dépannage

### Erreur : "command not found: sass"

**Cause** : SASS n'est pas installé globalement.

**Solution** :
```bash
npm install -g sass
# ou
npx sass --version
```

### Erreur : "Permission denied"

**Cause** : Droits insuffisants pour l'installation globale.

**Solution** :
```bash
sudo npm install -g sass
# ou utilisez nvm (recommandé)
```

### Erreur : "File to read not found"

**Cause** : Le fichier SCSS spécifié n'existe pas ou le chemin est incorrect.

**Solution** : Vérifiez le chemin et que le fichier existe :
```bash
ls -la style.scss
```

### Erreur : "Invalid CSS"

**Cause** : Erreur de syntaxe dans le fichier SCSS.

**Solution** : SASS affiche le numéro de ligne et l'erreur. Corrigez la syntaxe et recompilez.

---

## 12. Exercices

### Exercice 1 — Installation et vérification

1. Installez Node.js si ce n'est pas déjà fait
2. Installez SASS globalement avec npm
3. Vérifiez l'installation avec `sass --version`
4. Affichez l'aide avec `sass --help`

**Capture d'écran** les résultats et vérifiez que tout fonctionne.

---

### Exercice 2 — Première compilation

1. Créez un dossier `exercice-sass`
2. Créez un fichier `style.scss` avec le contenu suivant :

```scss
// Variables
$bg-color: #f0f0f0;
$text-color: #333;
$heading-color: #2c3e50;

// Styles de base
body {
  background-color: $bg-color;
  color: $text-color;
  font-family: Arial, sans-serif;
  line-height: 1.6;
}

h1, h2, h3 {
  color: $heading-color;
}

h1 {
  font-size: 2.5em;
  margin-bottom: 0.5em;
}

h2 {
  font-size: 1.8em;
  margin-bottom: 0.4em;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}
```

3. Compilez le fichier en CSS standard
4. Compilez le fichier en CSS minifié
5. Ouvrez les deux fichiers et comparez-les

**Question** : Quelle est la différence en taille entre les deux fichiers ?

---

### Exercice 3 — Mode surveillance

1. Avec le projet de l'exercice 2, lancez le mode surveillance :
   ```bash
   sass --watch style.scss:style.css
   ```
2. Modifiez la valeur de `$bg-color` dans `style.scss`
3. Sauvegardez le fichier
4. Observez que `style.css` est automatiquement régénéré
5. Arrêtez la surveillance avec `Ctrl+C`

**Question** : Pourquoi le mode surveillance est-il utile en développement ?

---

## Résumé

| Concept | Commande | Description |
|---------|----------|-------------|
| Installation | `npm install -g sass` | Installe SASS globalement |
| Compilation | `sass input.scss output.css` | Transforme SCSS en CSS |
| Surveillance | `sass --watch in:out` | Recompile automatiquement |
| Minification | `--style=compressed` | CSS compact pour la production |
| Source map | `--source-map` | Génère un fichier .map pour le débogage |

---

**Prochaine étape** : [Chapitre 3 — Premier fichier SCSS](chapitre-03-premier-fichier.md)
