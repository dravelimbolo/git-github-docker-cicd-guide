# Guide Complet : Git, GitHub, Docker & CI/CD
## De Débutant à Expert

---

## Table des Matières

1. [Introduction](#introduction)
2. [Git - Les Fondamentaux](#git---les-fondamentaux)
3. [GitHub - Collaboration en Équipe](#github---collaboration-en-équipe)
4. [Stratégie de Branches Git Flow](#stratégie-de-branches-git-flow)
5. [Docker - Containerisation](#docker---containerisation)
6. [GitHub Actions - CI/CD](#github-actions---cicd)
7. [Bonnes Pratiques](#bonnes-pratiques)
8. [Cas Pratique - Projet E-Commerce](#cas-pratique---projet-e-commerce)
9. [Résolution de Conflits](#résolution-de-conflits)
10. [Checklist de l'Expert](#checklist-de-lexpert)

---

## Introduction

Ce guide vous accompagnera dans la maîtrise complète des outils essentiels du développement moderne. Vous apprendrez à travailler efficacement en équipe, automatiser vos déploiements et éviter les problèmes courants.

**Objectifs du cours :**
- Maîtriser Git et GitHub de A à Z
- Comprendre et implémenter le Git Flow
- Containeriser des applications avec Docker
- Automatiser le CI/CD avec GitHub Actions
- Travailler en équipe sans conflits

---

## Git - Les Fondamentaux

### 9.3 Avant de Créer une Pull Request

```bash
✅ Tous les tests passent localement
✅ Build réussit sans erreurs
✅ Code lint sans warnings
✅ Rebase sur develop récent
✅ Pas de conflits
✅ Branche poussée sur GitHub
✅ Description PR complète et claire
✅ Issue liée avec "Closes #123"
✅ Reviewers assignés
✅ Labels ajoutés
```

### 9.4 Review de Code (Reviewer)

```bash
✅ Code compilé et testé localement
✅ Tests automatiques passent
✅ Logique métier correcte
✅ Pas de failles de sécurité évidentes
✅ Performance acceptable
✅ Code lisible et maintenable
✅ Documentation suffisante
✅ Pas de code dupliqué
✅ Respect des conventions du projet
✅ Feedback constructif donné
```

### 9.5 Après le Merge

```bash
✅ Branche locale supprimée
✅ Branche distante supprimée
✅ Develop mis à jour localement
✅ Issue fermée
✅ Tag de version créé (si release)
✅ Changelog mis à jour (si release)
✅ Équipe notifiée (Slack)
```

### 9.6 CI/CD Pipeline

```bash
✅ Tests unitaires configurés
✅ Tests d'intégration configurés
✅ Linting automatique
✅ Build Docker automatique
✅ Scan de sécurité activé
✅ Déploiement staging automatique
✅ Déploiement production manuel/approuvé
✅ Health checks après déploiement
✅ Rollback plan en place
```

### 9.7 Sécurité

```bash
✅ Pas de secrets dans le code
✅ Variables d'environnement utilisées
✅ .gitignore complet
✅ Dépendances à jour (npm audit)
✅ HTTPS partout
✅ Validation des entrées utilisateur
✅ Protection contre SQL injection
✅ Protection contre XSS
✅ Rate limiting en place
✅ Logs ne contiennent pas de données sensibles
```

---

## Annexes

### A. Commandes Git Avancées

#### A.1 Modifier l'Historique

```bash
# Modifier le dernier commit
git commit --amend -m "Nouveau message"

# Modifier les 3 derniers commits (interactive rebase)
git rebase -i HEAD~3

# Dans l'éditeur :
# pick abc123 commit 1
# reword def456 commit 2  # Changer le message
# squash ghi789 commit 3  # Fusionner avec le précédent

# Annuler un commit public (créer un nouveau commit d'annulation)
git revert abc123

# Annuler un commit privé (supprimer de l'historique)
git reset --hard HEAD~1  # DANGER: Perte de données !
```

#### A.2 Chercher dans l'Historique

```bash
# Trouver qui a modifié une ligne
git blame fichier.js

# Chercher dans tout l'historique
git log --all --grep="bug fix"

# Trouver quand un bug a été introduit (bisect)
git bisect start
git bisect bad                    # Commit actuel est mauvais
git bisect good abc123            # Ce commit était bon
# Git teste automatiquement les commits entre les deux
# À chaque étape : git bisect good ou git bisect bad
git bisect reset                  # Terminer
```

#### A.3 Stash Avancé

```bash
# Sauvegarder temporairement avec message
git stash save "WIP: refactoring user service"

# Lister les stash
git stash list

# Appliquer un stash spécifique
git stash apply stash@{2}

# Appliquer et supprimer
git stash pop

# Créer une branche depuis un stash
git stash branch feature/new-feature stash@{0}

# Supprimer tous les stash
git stash clear
```

#### A.4 Cherry-Pick

```bash
# Appliquer un commit spécifique d'une autre branche
git cherry-pick abc123

# Appliquer plusieurs commits
git cherry-pick abc123 def456 ghi789

# Résoudre les conflits si nécessaire
git add .
git cherry-pick --continue
```

#### A.5 Submodules

```bash
# Ajouter un submodule
git submodule add https://github.com/user/repo.git libs/repo

# Cloner un projet avec submodules
git clone --recursive https://github.com/user/main-repo.git

# Mettre à jour les submodules
git submodule update --remote --merge

# Supprimer un submodule
git submodule deinit libs/repo
git rm libs/repo
```

### B. Configuration Git Optimale

#### B.1 Fichier .gitconfig Global

```bash
# ~/.gitconfig
[user]
    name = Votre Nom
    email = votre.email@example.com

[core]
    editor = code --wait
    autocrlf = input
    ignorecase = false

[init]
    defaultBranch = main

[pull]
    rebase = true

[push]
    default = current
    followTags = true

[fetch]
    prune = true

[rebase]
    autoStash = true

[diff]
    tool = vscode

[merge]
    tool = vscode
    conflictstyle = diff3

[alias]
    # Shortcuts
    co = checkout
    br = branch
    ci = commit
    st = status
    
    # Pretty log
    lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
    
    # Show branches
    branches = branch -a
    
    # Undo last commit but keep changes
    undo = reset HEAD~1 --mixed
    
    # Clean up merged branches
    cleanup = "!git branch --merged | grep -v '\\*\\|main\\|develop' | xargs -n 1 git branch -d"
    
    # Show what changed in a file
    whatchanged = log -p --follow --
    
    # Stash shortcuts
    save = stash save
    pop = stash pop
    
    # Quick commit
    ac = !git add -A && git commit -m
    
    # Amend without editing message
    amend = commit --amend --no-edit

[color]
    ui = auto

[credential]
    helper = cache --timeout=3600
```

#### B.2 Fichier .gitignore Global

```bash
# ~/.gitignore_global

# OS Files
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db
Desktop.ini

# IDE
.vscode/
.idea/
*.swp
*.swo
*~
.project
.settings/

# Logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Environment
.env
.env.local
.env.*.local

# Temporary
*.tmp
*.temp
.cache/

# Activer globalement
git config --global core.excludesfile ~/.gitignore_global
```

### C. Scripts Utiles

#### C.1 Script de Setup Projet

```bash
#!/bin/bash
# setup-project.sh

echo "🚀 Project Setup Script"

# Vérifier Git
if ! command -v git &> /dev/null; then
    echo "❌ Git n'est pas installé"
    exit 1
fi

# Configuration Git
read -p "Nom complet : " git_name
read -p "Email : " git_email

git config --global user.name "$git_name"
git config --global user.email "$git_email"
git config --global pull.rebase true
git config --global init.defaultBranch main

echo "✅ Git configuré"

# Générer clé SSH
if [ ! -f ~/.ssh/id_ed25519 ]; then
    echo "🔑 Génération clé SSH..."
    ssh-keygen -t ed25519 -C "$git_email" -f ~/.ssh/id_ed25519 -N ""
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_ed25519
    echo "✅ Clé SSH créée"
    echo "📋 Ajoutez cette clé à GitHub:"
    cat ~/.ssh/id_ed25519.pub
else
    echo "✅ Clé SSH existe déjà"
fi

# Installer Docker
if ! command -v docker &> /dev/null; then
    echo "📦 Installation de Docker..."
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    sudo usermod -aG docker $USER
    echo "✅ Docker installé (redémarrer pour groupe docker)"
else
    echo "✅ Docker déjà installé"
fi

echo ""
echo "✨ Setup terminé !"
echo "📖 Prochaines étapes:"
echo "   1. Ajoutez votre clé SSH à GitHub"
echo "   2. Clonez votre projet: git clone git@github.com:user/repo.git"
echo "   3. Lancez: docker-compose up -d"
```

#### C.2 Script de Nettoyage Git

```bash
#!/bin/bash
# git-cleanup.sh

echo "🧹 Nettoyage du dépôt Git"

# Supprimer les branches mergées
echo "Suppression des branches mergées..."
git branch --merged | grep -v "\*\|main\|develop" | xargs -n 1 git branch -d

# Nettoyer les références distantes
echo "Nettoyage des références distantes..."
git remote prune origin

# Garbage collection
echo "Optimisation du dépôt..."
git gc --aggressive --prune=now

# Afficher la taille
echo "Taille du dépôt:"
du -sh .git

echo "✅ Nettoyage terminé"
```

#### C.3 Script Pre-Commit Hook

```bash
#!/bin/bash
# .git/hooks/pre-commit

echo "🔍 Pre-commit checks..."

# Vérifier qu'il n'y a pas de conflits non résolus
if git diff --check --cached | grep "conflict"; then
    echo "❌ Conflits détectés"
    exit 1
fi

# Vérifier les secrets
if git diff --cached | grep -E "(password|secret|api_key|token).*=.*['\"]"; then
    echo "⚠️  Possible secret détecté!"
    read -p "Continuer quand même? (y/N) " -n 1 -r
    echo
    if [[ ! $REPLY =~ ^[Yy]$ ]]; then
        exit 1
    fi
fi

# Linter (si disponible)
if command -v npm &> /dev/null; then
    npm run lint --silent
    if [ $? -ne 0 ]; then
        echo "❌ Linting failed"
        exit 1
    fi
fi

# Tests (si disponible)
if [ -f "package.json" ] && grep -q "\"test\"" package.json; then
    npm run test --silent
    if [ $? -ne 0 ]; then
        echo "❌ Tests failed"
        exit 1
    fi
fi

echo "✅ Pre-commit checks passed"
exit 0
```

### D. Troubleshooting

#### D.1 Problèmes Fréquents

**Problème : "Permission denied (publickey)"**
```bash
# Vérifier la clé SSH
ssh -T git@github.com

# Ajouter la clé à l'agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Vérifier la config SSH
cat ~/.ssh/config

# Devrait contenir:
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
```

**Problème : "fatal: refusing to merge unrelated histories"**
```bash
# Forcer le merge
git pull origin main --allow-unrelated-histories
```

**Problème : "Your branch and 'origin/main' have diverged"**
```bash
# Option 1: Rebase (historique propre)
git pull --rebase origin main

# Option 2: Merge (préserve l'historique)
git pull origin main

# Option 3: Forcer (DANGER: écrase le remote)
git push --force-with-lease origin main
```

**Problème : Commit dans la mauvaise branche**
```bash
# Annuler le commit (garde les changements)
git reset HEAD~1

# Changer de branche
git stash
git checkout correct-branch
git stash pop

# Commiter dans la bonne branche
git add .
git commit -m "message"
```

**Problème : Besoin d'annuler un push**
```bash
# Créer un commit d'annulation (safe)
git revert abc123
git push origin main

# Ou forcer (DANGER: réécrit l'historique public)
git reset --hard HEAD~1
git push --force origin main
```

#### D.2 Récupération d'Urgence

**Fichier supprimé par erreur**
```bash
# Retrouver dans l'historique
git log -- fichier-supprimé.js

# Restaurer depuis un commit
git checkout abc123 -- fichier-supprimé.js
```

**Branche supprimée par erreur**
```bash
# Lister les refs perdus
git reflog

# Retrouver le dernier commit de la branche
git reflog | grep "branch-name"

# Recréer la branche
git checkout -b branch-name abc123
```

**Reset --hard par erreur**
```bash
# Retrouver dans reflog
git reflog

# Revenir à l'état précédent
git reset --hard HEAD@{1}
```

### E. Ressources et Liens

#### E.1 Documentation Officielle

- **Git** : https://git-scm.com/doc
- **GitHub** : https://docs.github.com
- **Docker** : https://docs.docker.com
- **GitHub Actions** : https://docs.github.com/actions

#### E.2 Outils Recommandés

**Clients Git GUI:**
- GitKraken : Interface graphique intuitive
- SourceTree : Gratuit, complet
- GitHub Desktop : Simple pour débutants

**Extensions VS Code:**
- GitLens : Visualisation avancée
- Git Graph : Graphe des branches
- Git History : Historique visuel

**Outils CI/CD:**
- GitHub Actions : Intégré à GitHub
- CircleCI : Alternative puissante
- Jenkins : Auto-hébergé

**Monitoring:**
- Sentry : Tracking d'erreurs
- DataDog : Monitoring applicatif
- Prometheus + Grafana : Métriques

#### E.3 Apprentissage

**Tutoriels Interactifs:**
- Learn Git Branching : https://learngitbranching.js.org/
- Git Immersion : https://gitimmersion.com/
- Katacoda : https://www.katacoda.com/courses/git

**Livres:**
- Pro Git (gratuit) : https://git-scm.com/book
- Git Pocket Guide
- Continuous Delivery

**Cheat Sheets:**
- Git Cheat Sheet : https://training.github.com/
- Docker Cheat Sheet : https://dockerlabs.collabnix.com/
- Markdown Guide : https://www.markdownguide.org/

---

## Conclusion

### Récapitulatif du Parcours

Vous avez maintenant parcouru un guide complet qui couvre :

✅ **Git Fondamentaux** : Commits, branches, merges, rebases
✅ **GitHub** : Collaboration, PRs, protections, reviews
✅ **Git Flow** : Stratégie de branches professionnelle
✅ **Docker** : Containerisation et orchestration
✅ **CI/CD** : Automatisation avec GitHub Actions
✅ **Bonnes Pratiques** : Conventions, sécurité, prévention
✅ **Cas Pratique** : Projet réel avec équipe de 5 personnes
✅ **Résolution de Conflits** : Stratégies et techniques avancées

### Prochaines Étapes

**Semaine 1-2 : Pratique de Base**
- Créer un projet personnel
- Faire 20+ commits avec conventions
- Créer et merger 5+ branches
- Expérimenter les conflits volontairement

**Semaine 3-4 : Docker et CI/CD**
- Dockeriser une application
- Configurer GitHub Actions
- Déployer sur un serveur de test
- Automatiser les tests

**Semaine 5-6 : Projet en Équipe**
- Rejoindre ou créer un projet collaboratif
- Appliquer le Git Flow
- Faire des code reviews
- Gérer des releases

**Semaine 7-8 : Expertise**
- Optimiser les workflows
- Implémenter des feature flags
- Configurer le monitoring
- Documenter pour l'équipe

### Conseils Finaux

🎯 **La pratique est essentielle** : Lisez moins, codez plus
🤝 **Collaborez** : Rejoignez des projets open-source
📚 **Restez à jour** : Suivez les blogs tech et changelogs
🔧 **Expérimentez** : Cassez les choses dans un environnement safe
💬 **Partagez** : Enseignez ce que vous apprenez

### Devenir un Expert

Un expert Git/DevOps n'est pas quelqu'un qui n'a jamais de problèmes, mais quelqu'un qui :
- Sait rapidement diagnostiquer et résoudre les problèmes
- Prévient les erreurs par de bonnes pratiques
- Automatise les tâches répétitives
- Partage ses connaissances avec l'équipe
- S'adapte aux besoins du projet

**Vous êtes maintenant équipé pour devenir cet expert !** 🚀

---

## Lexique

**Branch** : Branche de développement parallèle
**Commit** : Enregistrement d'un changement
**Merge** : Fusion de deux branches
**Rebase** : Rejouer des commits sur une autre base
**Pull Request (PR)** : Demande de fusion de code
**CI/CD** : Intégration et Déploiement Continus
**Container** : Environnement d'exécution isolé
**Image** : Template de container
**Pipeline** : Séquence automatisée d'actions
**Staging** : Environnement de pré-production
**Hotfix** : Correction urgente en production
**Feature Flag** : Activation conditionnelle de fonctionnalité
**Rollback** : Retour à une version précédente
**Artifact** : Fichier produit par le build

---

**Version du Guide :** 1.0.0
**Dernière Mise à Jour :** Octobre 2025
**Auteur :** Formation DevOps Complète
**License :** MIT - Libre d'utilisation et de modification 1.1 Installation et Configuration

#### Installation

**Linux/Mac :**
```bash
# Linux (Debian/Ubuntu)
sudo apt-get update
sudo apt-get install git

# Mac
brew install git
```

**Windows :** Téléchargez depuis [git-scm.com](https://git-scm.com)

#### Configuration Initiale

```bash
# Identité globale
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"

# Éditeur par défaut
git config --global core.editor "code --wait"  # VS Code

# Voir toute la configuration
git config --list
```

### 1.2 Concepts Fondamentaux

Git fonctionne avec trois zones principales :

1. **Working Directory** : Vos fichiers en cours de modification
2. **Staging Area (Index)** : Zone de préparation avant commit
3. **Repository** : Historique des commits

```
Working Directory → (git add) → Staging Area → (git commit) → Repository
```

### 1.3 Commandes Essentielles

#### Initialiser un Dépôt

```bash
# Créer un nouveau projet
mkdir mon-projet
cd mon-projet
git init

# Cloner un projet existant
git clone https://github.com/username/repo.git
```

#### Workflow de Base

```bash
# Vérifier l'état des fichiers
git status

# Ajouter des fichiers au staging
git add fichier.txt              # Fichier spécifique
git add .                        # Tous les fichiers
git add src/                     # Dossier complet

# Créer un commit
git commit -m "Description claire du changement"

# Pousser vers le dépôt distant
git push origin nom-de-branche
```

#### Gestion des Branches

```bash
# Créer une nouvelle branche
git branch feature/nouvelle-fonctionnalite

# Créer et basculer sur une branche
git checkout -b feature/login
# OU (Git 2.23+)
git switch -c feature/login

# Lister les branches
git branch                       # Locales
git branch -a                    # Toutes (locales + distantes)

# Basculer entre branches
git checkout develop
# OU
git switch develop

# Supprimer une branche
git branch -d feature/login      # Locale (sécurisé)
git branch -D feature/login      # Locale (force)
git push origin --delete feature/login  # Distante
```

#### Récupération et Synchronisation

```bash
# Récupérer les changements sans fusionner
git fetch origin

# Récupérer et fusionner
git pull origin develop

# Mettre à jour sa branche avec les derniers changements
git pull --rebase origin develop
```

### 1.4 Historique et Navigation

```bash
# Voir l'historique
git log
git log --oneline                # Format condensé
git log --graph --all            # Vue graphique

# Voir les différences
git diff                         # Working vs Staging
git diff --staged                # Staging vs Repository
git diff branch1 branch2         # Entre branches

# Revenir en arrière
git checkout -- fichier.txt      # Annuler modifications
git reset HEAD fichier.txt       # Retirer du staging
git revert abc123                # Annuler un commit (safe)
git reset --hard HEAD~1          # Supprimer le dernier commit (danger!)
```

---

## GitHub - Collaboration en Équipe

### 2.1 Configuration du Compte

#### Authentification SSH

```bash
# Générer une clé SSH
ssh-keygen -t ed25519 -C "votre.email@example.com"

# Démarrer l'agent SSH
eval "$(ssh-agent -s)"

# Ajouter la clé à l'agent
ssh-add ~/.ssh/id_ed25519

# Copier la clé publique
cat ~/.ssh/id_ed25519.pub
# Ajoutez cette clé dans GitHub : Settings > SSH and GPG keys
```

### 2.2 Organisation du Dépôt

#### Structure Recommandée

```
mon-projet/
├── .github/
│   └── workflows/           # GitHub Actions
│       └── ci-cd.yml
├── src/                     # Code source
├── tests/                   # Tests
├── docker/                  # Fichiers Docker
│   ├── Dockerfile
│   └── docker-compose.yml
├── docs/                    # Documentation
├── .gitignore
├── README.md
├── LICENSE
└── package.json
```

#### Fichier .gitignore Complet

```gitignore
# Dependencies
node_modules/
vendor/
bower_components/

# Build outputs
dist/
build/
*.exe
*.dll
*.so
*.dylib

# IDE
.vscode/
.idea/
*.swp
*.swo
*~

# OS
.DS_Store
Thumbs.db

# Logs
*.log
logs/

# Environment
.env
.env.local
.env.*.local

# Docker
*.dockerfile.swp

# Temporary
tmp/
temp/
*.tmp
```

### 2.3 Protection des Branches

#### Règles de Protection pour `main`

Dans GitHub : **Settings > Branches > Add rule**

- ✅ Require a pull request before merging
- ✅ Require approvals (au moins 1)
- ✅ Require status checks to pass (CI/CD)
- ✅ Require conversation resolution before merging
- ✅ Include administrators
- ✅ Restrict who can push (Lead Dev uniquement)

#### Règles pour `develop`

- ✅ Require a pull request before merging
- ✅ Require status checks to pass
- ⚠️ Permissions : Lead Dev + Senior Devs

---

## Stratégie de Branches Git Flow

### 3.1 Structure des Branches

```
main (production)
  │
  └── develop (intégration)
        │
        ├── feature/user-authentication
        ├── feature/payment-integration
        ├── feature/product-catalog
        │
        ├── bugfix/login-error
        │
        └── hotfix/critical-security-issue
```

### 3.2 Types de Branches

#### Branches Principales

**`main`** : Production
- Code stable et déployé
- Tagué avec versions (v1.0.0, v1.1.0)
- Merge uniquement depuis `develop` ou `hotfix/*`

**`develop`** : Intégration
- Code en cours de développement
- Toujours compilable et testable
- Base pour les features

#### Branches Temporaires

**`feature/*`** : Nouvelles fonctionnalités
```bash
# Créer depuis develop
git checkout develop
git pull origin develop
git checkout -b feature/user-profile

# Nommage : feature/description-courte
```

**`bugfix/*`** : Corrections de bugs non-critiques
```bash
git checkout develop
git checkout -b bugfix/fix-login-validation
```

**`hotfix/*`** : Corrections urgentes en production
```bash
git checkout main
git checkout -b hotfix/security-patch-v1.2.1
```

**`release/*`** : Préparation de version
```bash
git checkout develop
git checkout -b release/v1.2.0
```

### 3.3 Workflow Complet

#### 1. Démarrer une Nouvelle Tâche

```bash
# 1. S'assurer d'être à jour
git checkout develop
git pull origin develop

# 2. Créer sa branche de travail
git checkout -b feature/add-shopping-cart

# 3. Travailler et commiter régulièrement
git add .
git commit -m "feat: add cart model and database schema"
git commit -m "feat: implement add to cart functionality"
git commit -m "test: add unit tests for cart service"
```

#### 2. Pousser sa Branche

```bash
# Première fois
git push -u origin feature/add-shopping-cart

# Puis
git push
```

#### 3. Créer une Pull Request (PR)

**Sur GitHub :**
1. Cliquer sur "Compare & pull request"
2. Base : `develop` ← Compare : `feature/add-shopping-cart`
3. Remplir la description :

```markdown
## Description
Ajout de la fonctionnalité panier d'achat

## Type de changement
- [x] Nouvelle fonctionnalité
- [ ] Correction de bug
- [ ] Breaking change

## Checklist
- [x] Le code compile sans erreur
- [x] Les tests passent
- [x] La documentation est à jour
- [x] Le code est commenté si nécessaire

## Tests effectués
- Test ajout d'article au panier
- Test suppression d'article
- Test calcul du total
```

4. Assigner des reviewers (Lead Dev + 1 Dev)
5. Ajouter des labels (feature, backend, etc.)

#### 4. Review de Code

**Reviewer :**
```bash
# Récupérer la branche pour tester localement
git fetch origin
git checkout feature/add-shopping-cart

# Tester l'application
npm install
npm test
npm start
```

**Commentaires de Review :**
- ✅ "LGTM (Looks Good To Me)" : Approuvé
- 💬 "Question:" : Demande de clarification
- ⚠️ "Suggestion:" : Amélioration optionnelle
- ❌ "Changes requested:" : Modifications obligatoires

#### 5. Merge de la PR

```bash
# Option 1 : Merge commit (recommandé pour features)
# Garde l'historique complet

# Option 2 : Squash and merge (pour nettoyer l'historique)
# Combine tous les commits en un seul

# Option 3 : Rebase and merge (historique linéaire)
# Applique les commits un par un
```

#### 6. Après le Merge

```bash
# Supprimer la branche locale
git checkout develop
git branch -d feature/add-shopping-cart

# Mettre à jour develop
git pull origin develop
```

---

## Docker - Containerisation

### 4.1 Concepts Docker

**Image** : Template immuable contenant l'application et ses dépendances
**Container** : Instance en cours d'exécution d'une image
**Dockerfile** : Instructions pour construire une image
**Docker Compose** : Orchestration de plusieurs containers

### 4.2 Installation Docker

```bash
# Linux (Ubuntu)
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Ajouter l'utilisateur au groupe docker
sudo usermod -aG docker $USER

# Vérifier l'installation
docker --version
docker-compose --version
```

### 4.3 Dockerfile - Application Node.js

```dockerfile
# Étape 1 : Build
FROM node:18-alpine AS builder

WORKDIR /app

# Copier les fichiers de dépendances
COPY package*.json ./

# Installer les dépendances
RUN npm ci --only=production

# Copier le code source
COPY . .

# Build de l'application (si nécessaire)
RUN npm run build

# Étape 2 : Production
FROM node:18-alpine

WORKDIR /app

# Créer un utilisateur non-root
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Copier depuis le builder
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
COPY --from=builder --chown=nodejs:nodejs /app/dist ./dist
COPY --from=builder --chown=nodejs:nodejs /app/package*.json ./

# Changer d'utilisateur
USER nodejs

# Exposer le port
EXPOSE 3000

# Healthcheck
HEALTHCHECK --interval=30s --timeout=3s --start-period=40s --retries=3 \
  CMD node -e "require('http').get('http://localhost:3000/health', (r) => {process.exit(r.statusCode === 200 ? 0 : 1)})"

# Lancer l'application
CMD ["node", "dist/index.js"]
```

### 4.4 Docker Compose - Stack Complète

```yaml
version: '3.8'

services:
  # Application Backend
  api:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: ecommerce-api
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_NAME=ecommerce
      - DB_USER=postgres
      - DB_PASSWORD=${DB_PASSWORD}
      - REDIS_HOST=redis
      - REDIS_PORT=6379
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    networks:
      - app-network
    restart: unless-stopped
    volumes:
      - ./logs:/app/logs

  # Frontend
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    container_name: ecommerce-frontend
    ports:
      - "80:80"
    depends_on:
      - api
    networks:
      - app-network
    restart: unless-stopped

  # Base de données PostgreSQL
  postgres:
    image: postgres:15-alpine
    container_name: ecommerce-db
    environment:
      - POSTGRES_DB=ecommerce
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    ports:
      - "5432:5432"
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./database/init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

  # Cache Redis
  redis:
    image: redis:7-alpine
    container_name: ecommerce-cache
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
    networks:
      - app-network
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 5
    restart: unless-stopped

  # Monitoring (optionnel)
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./monitoring/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-data:/prometheus
    networks:
      - app-network
    restart: unless-stopped

volumes:
  postgres-data:
  redis-data:
  prometheus-data:

networks:
  app-network:
    driver: bridge
```

### 4.5 Commandes Docker Essentielles

```bash
# Construire une image
docker build -t mon-app:latest .

# Lancer un container
docker run -d -p 3000:3000 --name mon-app mon-app:latest

# Docker Compose
docker-compose up -d                    # Démarrer tous les services
docker-compose down                     # Arrêter et supprimer
docker-compose logs -f api              # Voir les logs
docker-compose ps                       # État des services
docker-compose restart api              # Redémarrer un service

# Gestion des images
docker images                           # Lister les images
docker rmi image-id                     # Supprimer une image
docker system prune -a                  # Nettoyer tout

# Gestion des containers
docker ps                               # Containers actifs
docker ps -a                            # Tous les containers
docker stop container-id                # Arrêter
docker rm container-id                  # Supprimer
docker exec -it container-id sh         # Accéder au shell

# Logs et débogage
docker logs -f container-id             # Suivre les logs
docker inspect container-id             # Informations détaillées
docker stats                            # Utilisation des ressources
```

---

## GitHub Actions - CI/CD

### 5.1 Qu'est-ce que le CI/CD ?

**CI (Continuous Integration)** :
- Tester automatiquement chaque commit
- Valider le code avant le merge
- Détecter les bugs rapidement

**CD (Continuous Deployment)** :
- Déployer automatiquement en staging/production
- Livraison continue et fiable
- Réduction des erreurs humaines

### 5.2 Workflow GitHub Actions Complet

Créer : `.github/workflows/ci-cd.yml`

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [develop, main]
  pull_request:
    branches: [develop, main]

env:
  NODE_VERSION: '18'
  DOCKER_REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  # Job 1 : Tests et Qualité du Code
  test:
    name: Tests et Linting
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run unit tests
        run: npm run test:unit

      - name: Run integration tests
        run: npm run test:integration

      - name: Generate coverage report
        run: npm run test:coverage

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info
          flags: unittests
          name: codecov-umbrella

  # Job 2 : Analyse de Sécurité
  security:
    name: Security Scan
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          scan-ref: '.'
          format: 'sarif'
          output: 'trivy-results.sarif'

      - name: Upload Trivy results to GitHub Security
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: 'trivy-results.sarif'

      - name: Check for vulnerabilities
        run: npm audit --audit-level=moderate

  # Job 3 : Build Docker Image
  build:
    name: Build Docker Image
    runs-on: ubuntu-latest
    needs: [test, security]
    if: github.event_name == 'push'
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to Container Registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.DOCKER_REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.DOCKER_REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=ref,event=branch
            type=sha,prefix={{branch}}-
            type=semver,pattern={{version}}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          cache-from: type=gha
          cache-to: type=gha,mode=max

  # Job 4 : Déploiement Staging
  deploy-staging:
    name: Deploy to Staging
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/develop'
    environment:
      name: staging
      url: https://staging.example.com
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to staging server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.STAGING_HOST }}
          username: ${{ secrets.STAGING_USER }}
          key: ${{ secrets.STAGING_SSH_KEY }}
          script: |
            cd /var/www/app
            docker-compose pull
            docker-compose up -d
            docker-compose exec -T api npm run migrate

      - name: Run smoke tests
        run: |
          sleep 30
          curl -f https://staging.example.com/health || exit 1

  # Job 5 : Déploiement Production
  deploy-production:
    name: Deploy to Production
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/main'
    environment:
      name: production
      url: https://example.com
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Create Release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: v${{ github.run_number }}
          release_name: Release v${{ github.run_number }}
          draft: false
          prerelease: false

      - name: Deploy to production
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.PROD_HOST }}
          username: ${{ secrets.PROD_USER }}
          key: ${{ secrets.PROD_SSH_KEY }}
          script: |
            cd /var/www/app
            docker-compose pull
            docker-compose up -d --no-deps api
            docker-compose exec -T api npm run migrate

      - name: Health check
        run: |
          sleep 30
          curl -f https://example.com/health || exit 1

      - name: Notify team
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          text: 'Déploiement production réussi! 🚀'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}
        if: success()
```

### 5.3 Secrets GitHub

**Settings > Secrets and variables > Actions**

Ajouter :
- `STAGING_HOST` : IP du serveur staging
- `STAGING_USER` : Utilisateur SSH
- `STAGING_SSH_KEY` : Clé privée SSH
- `PROD_HOST` : IP production
- `PROD_USER` : Utilisateur production
- `PROD_SSH_KEY` : Clé production
- `SLACK_WEBHOOK` : URL webhook Slack
- `DB_PASSWORD` : Mot de passe BDD

---

## Bonnes Pratiques

### 6.1 Conventions de Commit

Format : `<type>(<scope>): <description>`

**Types :**
- `feat`: Nouvelle fonctionnalité
- `fix`: Correction de bug
- `docs`: Documentation
- `style`: Formatage, point-virgules manquants, etc.
- `refactor`: Refactoring sans changer la fonctionnalité
- `test`: Ajout de tests
- `chore`: Maintenance (dépendances, config, etc.)
- `perf`: Amélioration des performances

**Exemples :**
```bash
git commit -m "feat(auth): add JWT token authentication"
git commit -m "fix(cart): resolve total calculation bug"
git commit -m "docs(readme): update installation instructions"
git commit -m "test(api): add integration tests for payment endpoint"
git commit -m "refactor(database): optimize query performance"
```

### 6.2 Structure d'une Pull Request

**Titre :** Court et descriptif
```
feat: Add user authentication with JWT
```

**Description :**
```markdown
## 📝 Description
Implémentation de l'authentification utilisateur avec JWT tokens

## 🎯 Motivation
Permettre aux utilisateurs de se connecter de manière sécurisée

## 🔧 Modifications
- Ajout du middleware d'authentification
- Création des endpoints login/register
- Gestion des tokens JWT
- Tests unitaires et d'intégration

## ✅ Checklist
- [x] Code testé localement
- [x] Tests unitaires ajoutés
- [x] Documentation mise à jour
- [x] Pas de conflits avec develop
- [x] Build CI passe

## 🖼️ Screenshots
[Si applicable]

## 🔗 Liens
- Issue #123
- Documentation : docs/authentication.md
```

### 6.3 Code Review - Checklist

**Reviewer doit vérifier :**

✅ **Fonctionnalité**
- Le code fait ce qu'il est censé faire
- Pas de régression introduite
- Edge cases gérés

✅ **Qualité du Code**
- Code lisible et maintenable
- Nommage clair des variables/fonctions
- Pas de duplication (DRY)
- Respect des conventions du projet

✅ **Performance**
- Pas de problèmes de performance évidents
- Requêtes BDD optimisées
- Pas de fuites mémoire

✅ **Sécurité**
- Pas de vulnérabilités évidentes
- Données sensibles protégées
- Validation des entrées utilisateur

✅ **Tests**
- Tests présents et pertinents
- Couverture suffisante
- Tests passent

✅ **Documentation**
- Code commenté si nécessaire
- README mis à jour
- API documentée

### 6.4 Éviter les Conflits

#### Stratégies Préventives

**1. Communiquer en équipe**
```
Sur Slack/Discord :
"Je travaille sur le fichier UserService.js (feature/add-notifications)"
```

**2. Mettre à jour régulièrement**
```bash
# Chaque matin
git checkout develop
git pull origin develop
git checkout feature/ma-branche
git rebase develop
```

**3. Commits atomiques et fréquents**
```bash
# Plutôt que 1 gros commit
git commit -m "feat: add, update, delete user + tests + docs"

# Préférer plusieurs petits commits
git commit -m "feat(user): add create user endpoint"
git commit -m "feat(user): add update user endpoint"
git commit -m "test(user): add user service tests"
```

**4. Pull Requests petites**
- Maximum 400 lignes de code
- Une seule fonctionnalité
- Plus facile à reviewer

**5. Feature Flags**
```javascript
// Permettre de merger du code non terminé
if (featureFlags.newCheckout) {
  return newCheckoutProcess();
} else {
  return oldCheckoutProcess();
}
```

### 6.5 Sécurité

#### Ne JAMAIS commiter

```bash
# .env
DB_PASSWORD=monmotdepasse123
API_KEY=sk_live_abc123xyz

# credentials.json
{
  "aws_access_key": "AKIA...",
  "aws_secret_key": "..."
}
```

#### Utiliser git-secrets

```bash
# Installation
brew install git-secrets

# Configuration
git secrets --install
git secrets --register-aws

# Scan du repo
git secrets --scan
```

#### Rotation des Secrets

Si un secret est commité :
1. Le retirer de l'historique Git
2. Révoquer/changer le secret immédiatement
3. Auditer l'utilisation potentiellement malveillante

---

## Cas Pratique - Projet E-Commerce

### 7.1 L'Équipe

**Composition :**
- 👤 **Sarah** - Lead Developer (Full Stack)
- 👤 **Marc** - Project Manager
- 👤 **Alex** - Developer Backend
- 👤 **Julie** - Developer Frontend
- 👤 **Tom** - Developer Full Stack

### 7.2 Le Projet

**Objectif :** Développer une plateforme e-commerce avec :
- Catalogue de produits
- Panier d'achat
- Système de paiement
- Gestion des commandes
- Tableau de bord admin

**Stack Technique :**
- Backend : Node.js + Express + PostgreSQL
- Frontend : React + TypeScript
- Infrastructure : Docker + GitHub Actions

### 7.3 Initialisation du Projet

#### Jour 1 - Sarah (Lead Dev)

```bash
# 1. Créer le repository sur GitHub
# Settings > Branches > Protection rules

# 2. Clone et setup initial
git clone git@github.com:equipe/ecommerce-app.git
cd ecommerce-app

# 3. Structure du projet
mkdir -p backend frontend docker .github/workflows docs

# 4. Initialize backend
cd backend
npm init -y
npm install express pg redis dotenv
npm install -D jest supertest eslint

# 5. Initialize frontend
cd ../frontend
npx create-react-app . --template typescript

# 6. Docker setup
cd ..
touch docker/Dockerfile docker/docker-compose.yml

# 7. Git setup
git add .
git commit -m "chore: initial project structure"
git push origin main

# 8. Créer la branche develop
git checkout -b develop
git push -u origin develop
```

#### Jour 1 - Configuration GitHub

**Sarah configure les protections de branches :**

```yaml
# Protection main :
- Require pull request (2 approvals : Sarah + 1 dev)
- Require status checks (CI must pass)
- No direct push (même pour Sarah)

# Protection develop :
- Require pull request (1 approval)
- Require status checks
- Sarah + devs peuvent merger
```

**Sarah configure les workflows CI/CD :**
```bash
# .github/workflows/ci-cd.yml
# (voir section 5.2 du cours)
git add .github/
git commit -m "ci: add GitHub Actions workflow"
git push origin develop
```

### 7.4 Sprint 1 - Semaine 1

#### Réunion de Planning (Marc - PM)

**Marc créé les issues sur GitHub :**

```markdown
Issue #1 : Setup Database Schema
- Assigné : Alex
- Labels : backend, database
- Sprint : 1

Issue #2 : User Authentication API
- Assigné : Alex
- Labels : backend, authentication
- Sprint : 1

Issue #3 : Product Catalog UI
- Assigné : Julie
- Labels : frontend, ui
- Sprint : 1

Issue #4 : Shopping Cart Logic
- Assigné : Tom
- Labels : backend, frontend
- Sprint : 1
```

### 7.5 Workflow Quotidien

#### Lundi Matin - Alex (Backend)

**Tâche : Setup Database Schema (Issue #1)**

```bash
# 1. Mettre à jour develop
git checkout develop
git pull origin develop

# 2. Créer la branche de travail
git checkout -b feature/database-schema

# 3. Créer le schéma SQL
cat > backend/database/schema.sql << 'EOF'
-- Tables utilisateurs
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tables produits
CREATE TABLE products (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  description TEXT,
  price DECIMAL(10,2) NOT NULL,
  stock_quantity INTEGER DEFAULT 0,
  category VARCHAR(100),
  image_url VARCHAR(500),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tables commandes
CREATE TABLE orders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  total_amount DECIMAL(10,2) NOT NULL,
  status VARCHAR(50) DEFAULT 'pending',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE order_items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  order_id UUID REFERENCES orders(id) ON DELETE CASCADE,
  product_id UUID REFERENCES products(id),
  quantity INTEGER NOT NULL,
  unit_price DECIMAL(10,2) NOT NULL
);

-- Index pour performance
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_products_category ON products(category);
CREATE INDEX idx_orders_user ON orders(user_id);
EOF

# 4. Créer les migrations
cat > backend/database/migrations/001_initial_schema.js << 'EOF'
const fs = require('fs');
const path = require('path');

exports.up = async (client) => {
  const sql = fs.readFileSync(
    path.join(__dirname, '../schema.sql'),
    'utf8'
  );
  await client.query(sql);
};

exports.down = async (client) => {
  await client.query('DROP TABLE IF EXISTS order_items CASCADE');
  await client.query('DROP TABLE IF EXISTS orders CASCADE');
  await client.query('DROP TABLE IF EXISTS products CASCADE');
  await client.query('DROP TABLE IF EXISTS users CASCADE');
};
EOF

# 5. Commiter le travail
git add backend/database/
git commit -m "feat(db): add initial database schema and migrations"

# 6. Pousser la branche
git push -u origin feature/database-schema
```

**Alex crée une Pull Request :**

```markdown
Titre : feat(db): Add initial database schema

## Description
Création du schéma de base de données initial pour l'application e-commerce.

## Tables créées
- `users` : Gestion des utilisateurs
- `products` : Catalogue de produits
- `orders` : Commandes clients
- `order_items` : Détails des commandes

## Checklist
- [x] Schéma SQL créé
- [x] Migrations ajoutées
- [x] Index de performance créés
- [x] Relations FK définies

Closes #1
```

#### Lundi Après-midi - Sarah (Review)

```bash
# 1. Récupérer la branche d'Alex
git fetch origin
git checkout feature/database-schema

# 2. Tester localement
cd backend
docker-compose up -d postgres
npm run migrate

# 3. Vérifier le schéma
psql -h localhost -U postgres -d ecommerce -c "\dt"

# Sarah approuve la PR sur GitHub
# ✅ "LGTM! Schema looks solid, migrations work correctly."
```

**Sarah merge la PR :**
- Option : "Squash and merge"
- Delete branch automatiquement

```bash
# 4. Alex met à jour son environnement
git checkout develop
git pull origin develop
git branch -d feature/database-schema
```

#### Mardi - Julie (Frontend)

**Tâche : Product Catalog UI (Issue #3)**

```bash
# 1. Sync avec develop
git checkout develop
git pull origin develop

# 2. Créer la branche
git checkout -b feature/product-catalog-ui

# 3. Créer le composant ProductCard
cat > frontend/src/components/ProductCard.tsx << 'EOF'
import React from 'react';
import './ProductCard.css';

interface Product {
  id: string;
  name: string;
  description: string;
  price: number;
  imageUrl: string;
  stockQuantity: number;
}

interface ProductCardProps {
  product: Product;
  onAddToCart: (productId: string) => void;
}

export const ProductCard: React.FC<ProductCardProps> = ({ 
  product, 
  onAddToCart 
}) => {
  return (
    <div className="product-card">
      <img 
        src={product.imageUrl} 
        alt={product.name}
        className="product-image"
      />
      <div className="product-info">
        <h3 className="product-name">{product.name}</h3>
        <p className="product-description">{product.description}</p>
        <div className="product-footer">
          <span className="product-price">${product.price.toFixed(2)}</span>
          <button 
            className="btn-add-cart"
            onClick={() => onAddToCart(product.id)}
            disabled={product.stockQuantity === 0}
          >
            {product.stockQuantity > 0 ? 'Add to Cart' : 'Out of Stock'}
          </button>
        </div>
      </div>
    </div>
  );
};
EOF

# 4. Créer les styles
cat > frontend/src/components/ProductCard.css << 'EOF'
.product-card {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.2s, box-shadow 0.2s;
  background: white;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.product-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.product-info {
  padding: 16px;
}

.product-name {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 8px;
}

.product-description {
  color: #666;
  font-size: 14px;
  margin-bottom: 12px;
}

.product-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.product-price {
  font-size: 20px;
  font-weight: bold;
  color: #2c5f2d;
}

.btn-add-cart {
  background: #007bff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 500;
}

.btn-add-cart:hover:not(:disabled) {
  background: #0056b3;
}

.btn-add-cart:disabled {
  background: #ccc;
  cursor: not-allowed;
}
EOF

# 5. Créer la page catalogue
cat > frontend/src/pages/ProductCatalog.tsx << 'EOF'
import React, { useState, useEffect } from 'react';
import { ProductCard } from '../components/ProductCard';
import './ProductCatalog.css';

export const ProductCatalog: React.FC = () => {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    fetchProducts();
  }, []);

  const fetchProducts = async () => {
    try {
      const response = await fetch('/api/products');
      if (!response.ok) throw new Error('Failed to fetch products');
      const data = await response.json();
      setProducts(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  const handleAddToCart = (productId: string) => {
    console.log('Add to cart:', productId);
    // TODO: Implement cart logic
  };

  if (loading) return <div className="loading">Loading products...</div>;
  if (error) return <div className="error">Error: {error}</div>;

  return (
    <div className="catalog-container">
      <h1>Product Catalog</h1>
      <div className="products-grid">
        {products.map((product) => (
          <ProductCard 
            key={product.id}
            product={product}
            onAddToCart={handleAddToCart}
          />
        ))}
      </div>
    </div>
  );
};
EOF

# 6. Tests unitaires
cat > frontend/src/components/ProductCard.test.tsx << 'EOF'
import React from 'react';
import { render, screen, fireEvent } from '@testing-library/react';
import { ProductCard } from './ProductCard';

describe('ProductCard', () => {
  const mockProduct = {
    id: '1',
    name: 'Test Product',
    description: 'Test Description',
    price: 29.99,
    imageUrl: 'https://example.com/image.jpg',
    stockQuantity: 10
  };

  const mockAddToCart = jest.fn();

  it('renders product information correctly', () => {
    render(<ProductCard product={mockProduct} onAddToCart={mockAddToCart} />);
    
    expect(screen.getByText('Test Product')).toBeInTheDocument();
    expect(screen.getByText('Test Description')).toBeInTheDocument();
    expect(screen.getByText('$29.99')).toBeInTheDocument();
  });

  it('calls onAddToCart when button is clicked', () => {
    render(<ProductCard product={mockProduct} onAddToCart={mockAddToCart} />);
    
    const button = screen.getByText('Add to Cart');
    fireEvent.click(button);
    
    expect(mockAddToCart).toHaveBeenCalledWith('1');
  });

  it('disables button when out of stock', () => {
    const outOfStockProduct = { ...mockProduct, stockQuantity: 0 };
    render(<ProductCard product={outOfStockProduct} onAddToCart={mockAddToCart} />);
    
    const button = screen.getByText('Out of Stock');
    expect(button).toBeDisabled();
  });
});
EOF

# 7. Commits atomiques
git add frontend/src/components/ProductCard.*
git commit -m "feat(ui): add ProductCard component"

git add frontend/src/pages/ProductCatalog.*
git commit -m "feat(ui): add ProductCatalog page"

git add frontend/src/components/ProductCard.test.tsx
git commit -m "test(ui): add ProductCard unit tests"

# 8. Push
git push -u origin feature/product-catalog-ui
```

**Julie crée la PR et assigne Sarah + Tom pour review**

#### Mercredi - Conflit Potentiel !

**Situation :** Tom et Alex travaillent tous deux sur le fichier `backend/src/services/CartService.js`

**Tom (Shopping Cart):**
```bash
git checkout develop
git pull origin develop
git checkout -b feature/shopping-cart

# Tom modifie CartService.js
git add backend/src/services/CartService.js
git commit -m "feat(cart): add cart total calculation"
git push -u origin feature/shopping-cart
```

**Tom crée une PR → Sarah merge rapidement**

**Alex (10 minutes plus tard - User Authentication):**
```bash
# Alex travaille sur sa branche depuis ce matin
git checkout feature/user-auth

# Alex modifie aussi CartService.js pour ajouter userId
git add backend/src/services/CartService.js
git commit -m "feat(auth): add user ID to cart service"
git push -u origin feature/user-auth

# Alex crée la PR
```

**GitHub signale un conflit ! 🔴**

#### Résolution du Conflit - Alex

```bash
# 1. Mettre à jour develop localement
git checkout develop
git pull origin develop

# 2. Revenir sur sa branche
git checkout feature/user-auth

# 3. Rebaser sur develop
git rebase develop

# ⚠️ Conflit détecté !
# Auto-merging backend/src/services/CartService.js
# CONFLICT (content): Merge conflict in backend/src/services/CartService.js

# 4. Ouvrir le fichier en conflit
code backend/src/services/CartService.js
```

**Fichier avec conflit :**
```javascript
class CartService {
  constructor() {
    this.carts = new Map();
  }

<<<<<<< HEAD (changes from develop - Tom's code)
  calculateTotal(cartItems) {
    return cartItems.reduce((total, item) => {
      return total + (item.price * item.quantity);
    }, 0);
  }
=======
  getUserCart(userId) {
    if (!this.carts.has(userId)) {
      this.carts.set(userId, []);
    }
    return this.carts.get(userId);
  }
>>>>>>> feature/user-auth (Alex's code)
}
```

**Résolution manuelle :**
```javascript
class CartService {
  constructor() {
    this.carts = new Map();
  }

  // Code de Tom (gardé)
  calculateTotal(cartItems) {
    return cartItems.reduce((total, item) => {
      return total + (item.price * item.quantity);
    }, 0);
  }

  // Code d'Alex (gardé et amélioré)
  getUserCart(userId) {
    if (!this.carts.has(userId)) {
      this.carts.set(userId, []);
    }
    return this.carts.get(userId);
  }

  // Nouvelle méthode qui combine les deux
  getUserCartTotal(userId) {
    const userCart = this.getUserCart(userId);
    return this.calculateTotal(userCart);
  }
}

module.exports = CartService;
```

```bash
# 5. Marquer comme résolu
git add backend/src/services/CartService.js

# 6. Continuer le rebase
git rebase --continue

# 7. Force push (le rebase réécrit l'historique)
git push --force-with-lease origin feature/user-auth

# 8. Notifier sur la PR
# "Conflit résolu avec feature/shopping-cart. 
#  J'ai combiné les deux approches. @Tom peux-tu vérifier ?"
```

**Tom review et approuve ✅**

### 7.6 Prévenir les Conflits - Communication

**Sur Slack - Canal #dev-backend :**

```
Tom: 🔨 Je travaille sur CartService.js pour le calcul des totaux
     Branch: feature/shopping-cart
     ETA: 2h

Alex: 👍 Ok, j'attendrai ton merge avant de toucher à ce fichier
      ou je travaille sur UserService en attendant
```

#### Jeudi - Merge vers Develop

**Toutes les features du Sprint 1 sont terminées :**

```bash
✅ feature/database-schema → merged
✅ feature/user-auth → merged  
✅ feature/product-catalog-ui → merged
✅ feature/shopping-cart → merged
```

**Sarah vérifie develop :**
```bash
git checkout develop
git pull origin develop

# Vérifier que tout compile
cd backend && npm run build
cd ../frontend && npm run build

# Lancer les tests
npm run test

# Vérifier avec Docker
docker-compose up -d
```

**Tout fonctionne ! ✅**

### 7.7 Release vers Production

#### Vendredi - Préparation Release

**Sarah (Lead Dev) :**

```bash
# 1. Créer la branche release
git checkout develop
git pull origin develop
git checkout -b release/v1.0.0

# 2. Mettre à jour la version
cd backend
npm version 1.0.0
cd ../frontend
npm version 1.0.0

# 3. Mettre à jour le CHANGELOG
cat > CHANGELOG.md << 'EOF'
# Changelog

## [1.0.0] - 2025-10-23

### Added
- User authentication with JWT
- Product catalog with search
- Shopping cart functionality
- Database schema and migrations
- Docker containerization
- CI/CD pipeline with GitHub Actions

### Security
- Password hashing with bcrypt
- JWT token validation
- SQL injection prevention
- XSS protection
EOF

git add .
git commit -m "chore(release): prepare v1.0.0"
git push -u origin release/v1.0.0
```

**Sarah crée une PR : `release/v1.0.0` → `main`**

```markdown
## Release v1.0.0

### Features
- ✅ Authentication système
- ✅ Catalogue produits
- ✅ Panier d'achat
- ✅ API REST complète

### Reviewers
@marc (PM approval required)
@team (notification)

### Deployment Checklist
- [x] All tests pass
- [x] Security scan completed
- [x] Documentation updated
- [x] Database migrations tested
- [x] Staging deployment successful
- [ ] PM approval
- [ ] Production deployment scheduled
```

**Marc (PM) approuve ✅**

**Sarah merge vers main :**

```bash
# GitHub Actions se déclenche automatiquement :
# 1. Run tests ✅
# 2. Build Docker images ✅
# 3. Deploy to production ✅
# 4. Health checks ✅
```

**Notification Slack :**
```
🚀 Deployment successful!
Version: v1.0.0
Environment: Production
Status: Healthy
URL: https://ecommerce-app.com
```

#### Après le Merge

```bash
# Merger release dans develop aussi
git checkout develop
git merge release/v1.0.0
git push origin develop

# Supprimer la branche release
git branch -d release/v1.0.0
git push origin --delete release/v1.0.0

# Créer un tag Git
git checkout main
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

### 7.8 Hotfix en Production

#### Lundi Suivant - Bug Critique ! 🔥

**Marc détecte un bug en production :**
```
❌ Les utilisateurs ne peuvent pas passer commande
Error: "Payment gateway timeout"
```

**Sarah (Lead Dev) agit immédiatement :**

```bash
# 1. Créer hotfix depuis main
git checkout main
git pull origin main
git checkout -b hotfix/payment-timeout-v1.0.1

# 2. Identifier et corriger le bug
cat > backend/src/services/PaymentService.js << 'EOF'
class PaymentService {
  constructor() {
    this.timeout = 30000; // Augmenté de 5s à 30s
  }

  async processPayment(orderId, amount) {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), this.timeout);

    try {
      const response = await fetch('https://payment-gateway.com/api/charge', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ orderId, amount }),
        signal: controller.signal
      });

      clearTimeout(timeoutId);
      return await response.json();
    } catch (error) {
      if (error.name === 'AbortError') {
        throw new Error('Payment gateway timeout');
      }
      throw error;
    }
  }
}
EOF

# 3. Test rapide
npm run test:unit -- PaymentService.test.js

# 4. Commit
git add backend/src/services/PaymentService.js
git commit -m "fix(payment): increase gateway timeout to 30s"

# 5. Mettre à jour la version
npm version patch  # 1.0.0 → 1.0.1

git commit -am "chore: bump version to v1.0.1"

# 6. Push
git push -u origin hotfix/payment-timeout-v1.0.1
```

**Sarah crée une PR urgente : `hotfix/payment-timeout-v1.0.1` → `main`**

```markdown
## 🔥 HOTFIX: Payment Gateway Timeout

### Severity: CRITICAL
### Impact: Users cannot complete purchases

### Root Cause
Payment gateway timeout set to 5s, gateway needs 10-15s

### Solution
Increased timeout to 30s with proper error handling

### Testing
- [x] Unit tests pass
- [x] Tested with staging payment gateway
- [x] Confirmed 30s is sufficient

### Deployment
Request immediate review and deployment

cc @marc @alex
```

**Marc approuve immédiatement ✅**
**Sarah merge et déploie :**

```bash
# Le pipeline CI/CD déploie automatiquement
# En 5 minutes, le fix est en production ✅
```

**Merger le hotfix dans develop aussi :**
```bash
git checkout develop
git pull origin develop
git merge hotfix/payment-timeout-v1.0.1
git push origin develop

git checkout main
git tag -a v1.0.1 -m "Hotfix: Payment gateway timeout"
git push origin v1.0.1
```

---

## Résolution de Conflits

### 8.1 Types de Conflits

#### Conflit Simple (Même Fichier, Différentes Lignes)

**Fichier : `src/config.js`**

Develop :
```javascript
const config = {
  apiUrl: 'https://api.example.com',
  timeout: 5000
};
```

Votre branche :
```javascript
const config = {
  apiUrl: 'https://api.example.com',
  retryAttempts: 3
};
```

**Résolution :**
```javascript
const config = {
  apiUrl: 'https://api.example.com',
  timeout: 5000,        // De develop
  retryAttempts: 3      // De votre branche
};
```

#### Conflit Complexe (Même Lignes Modifiées)

**Fichier avec conflit :**
```javascript
<<<<<<< HEAD (develop)
function calculatePrice(item) {
  return item.price * item.quantity * 1.2; // +20% tax
}
=======
function calculatePrice(item) {
  const subtotal = item.price * item.quantity;
  return subtotal - (subtotal * item.discount);
}
>>>>>>> feature/discount-system
```

**Résolution - Combiner les deux :**
```javascript
function calculatePrice(item) {
  const subtotal = item.price * item.quantity;
  const afterDiscount = subtotal - (subtotal * item.discount);
  return afterDiscount * 1.2; // Apply 20% tax after discount
}
```

### 8.2 Stratégies de Résolution

#### Méthode 1 : Merge

```bash
git checkout feature/ma-branche
git merge develop

# En cas de conflit
# 1. Résoudre manuellement les fichiers
# 2. git add <fichiers-résolus>
# 3. git commit (message auto-généré)
```

**Avantages :** Préserve tout l'historique
**Inconvénients :** Crée un commit de merge

#### Méthode 2 : Rebase (Recommandée)

```bash
git checkout feature/ma-branche
git rebase develop

# En cas de conflit
# 1. Résoudre manuellement
# 2. git add <fichiers-résolus>
# 3. git rebase --continue
# 4. Répéter si plusieurs conflits

# Pour annuler le rebase
git rebase --abort
```

**Avantages :** Historique linéaire et propre
**Inconvénients :** Réécrit l'historique (nécessite force push)

### 8.3 Outils de Résolution

#### VS Code

```bash
# Ouvrir le fichier en conflit dans VS Code
code fichier-en-conflit.js

# VS Code offre 4 options :
# 1. Accept Current Change (votre code)
# 2. Accept Incoming Change (code de develop)
# 3. Accept Both Changes
# 4. Compare Changes (vue côte à côte)
```

#### Git Mergetool

```bash
# Configurer un outil de merge
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait --merge $REMOTE $LOCAL $BASE $MERGED'

# Utiliser l'outil
git mergetool
```

#### GitKraken / SourceTree

Interface graphique pour visualiser et résoudre les conflits facilement.

### 8.4 Prévention des Conflits

#### Règle d'Or : Petites PRs Fréquentes

```bash
# ❌ Mauvais : Une énorme PR après 2 semaines
git diff develop...feature/huge-refactor
# 150 files changed, 5000 insertions, 3000 deletions

# ✅ Bon : Plusieurs petites PRs
# PR1: Refactor UserService (5 files)
# PR2: Update tests (10 files)
# PR3: Add new endpoints (8 files)
```

#### Synchronisation Quotidienne

```bash
# Chaque matin avant de commencer
git checkout develop
git pull origin develop
git checkout feature/ma-branche
git rebase develop
git push --force-with-lease origin feature/ma-branche
```

#### Feature Flags pour Code Non Terminé

```javascript
// Ne pas attendre que tout soit fini pour merger
const features = {
  newCheckout: process.env.FEATURE_NEW_CHECKOUT === 'true'
};

function processCheckout(cart) {
  if (features.newCheckout) {
    return newCheckoutProcess(cart); // Code en développement
  }
  return legacyCheckoutProcess(cart); // Code stable
}
```

### 8.5 Conflits sur node_modules ou package-lock.json

```bash
# En cas de conflit sur package-lock.json
git checkout develop -- package-lock.json
npm install
git add package-lock.json
git rebase --continue

# Pour node_modules : JAMAIS commiter
# Toujours dans .gitignore
```

### 8.6 Workflow de Résolution Complet

```bash
# Situation : Vous voulez merger votre PR mais il y a des conflits

# 1. Sauvegarder votre travail en cours
git add .
git stash

# 2. Mettre à jour develop
git checkout develop
git pull origin develop

# 3. Retourner sur votre branche
git checkout feature/ma-fonctionnalite

# 4. Restaurer votre travail
git stash pop

# 5. Rebaser sur develop
git rebase develop

# 6. Si conflit, résoudre fichier par fichier
# Ouvrir chaque fichier en conflit
code <fichier-en-conflit>

# Supprimer les marqueurs de conflit et garder le bon code
# <<<<<<< HEAD
# =======  
# >>>>>>>

# 7. Marquer comme résolu
git add <fichier-résolu>

# 8. Continuer le rebase
git rebase --continue

# 9. Push avec force (rebase réécrit l'historique)
git push --force-with-lease origin feature/ma-fonctionnalite

# 10. La PR est maintenant sans conflit ! ✅
```

---

## Checklist de l'Expert

### 9.1 Avant de Commencer une Tâche

```bash
✅ Tâche bien définie dans une issue
✅ Issue assignée à moi
✅ Branche develop à jour localement
✅ Nouvelle branche créée depuis develop
✅ Nommage de branche correct (feature/task-description)
✅ Communication sur Slack de la tâche en cours
```

### 9.2 Pendant le Développement

```bash
✅ Commits atomiques et fréquents
✅ Messages de commit clairs (convention feat/fix/docs/etc.)
✅ Tests écrits et qui passent
✅ Code reviewé personnellement avant push
✅ Pas de console.log oubliés
✅ Pas de TODO laissés
✅ Documentation mise à jour si nécessaire
✅ Sync quotidien avec develop (rebase)
```

###
