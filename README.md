# E-Learning App

Une application web d'e-learning développée avec [SvelteKit](https://kit.svelte.dev/).  
Elle propose trois exercices interactifs pour aider à l'apprentissage de concepts variés.

## 📂 Architecture du projet

L'application est structurée en trois modules principaux, chacun correspondant à un exercice :

- **`src/complete-words/`** : Exercice où l'utilisateur doit compléter des mots.  
- **`src/link-elements/`** : Exercice où il faut associer des éléments entre eux.  
- **`src/categories/`** : Exercice où il faut classer des mots dans les bonnes catégories.  

Chaque module contient :
- Une page principale pour l'exercice.
- Un dossier `components/` contenant les composants spécifiques à cet exercice.

## 🚀 Installation et développement

### 1️⃣ Cloner le projet

```bash
git clone <URL_DU_REPO>
cd e-learning-app
```

### 2️⃣ Installer les dépendances

```bash
npm install
```

### 3️⃣ Lancer le serveur de développement

```bash
npm run dev

# ou pour ouvrir directement dans le navigateur :
npm run dev -- --open
```

## 🏗️ Build et déploiement

Pour générer une version optimisée de l'application :

```bash
npm run build
```

Tu peux prévisualiser le build avec :

```bash
npm run preview
```

> Pour le déploiement, installe l'[adapter](https://kit.svelte.dev/docs/adapters) adapté à ton hébergement.

## ✨ Fonctionnalités

- 🌍 Application web interactive et intuitive.
- 🎯 Trois types d'exercices pour renforcer l'apprentissage.
- 📊 Suivi du score et feedback instantané.
- 📱 Responsive et compatible avec les mobiles.
