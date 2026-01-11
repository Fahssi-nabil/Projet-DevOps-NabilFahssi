# Instructions de configuration rapide

## Commandes Git essentielles

### 1. Initialiser Git et faire le premier commit

```bash
git init
git add .
git commit -m "Initial commit: structure du projet Java/Maven avec application simple"
```

### 2. Créer le dépôt GitHub

1. Allez sur https://github.com/new
2. Nom du dépôt: `Projet-DevOps-{{VotreNom}}{{VotrePrenom}}`
   - Exemple: `Projet-DevOps-DupontJean`
3. Ne cochez PAS "Initialize with README"
4. Cliquez sur "Create repository"

### 3. Lier le dépôt local à GitHub

```bash
git remote add origin https://github.com/VOTRE_USERNAME/Projet-DevOps-VotreNomVotrePrenom.git
git branch -M main
git push -u origin main
```

**Important:** Remplacez:
- `VOTRE_USERNAME` par votre nom d'utilisateur GitHub
- `VotreNomVotrePrenom` par votre nom et prénom (sans espaces)

### 4. Créer et pousser la branche dev

```bash
git checkout -b dev
git push -u origin dev
```

### 5. Vérifier que l'application fonctionne

```bash
mvn clean test
mvn package
java -jar target/min-project-1.0-SNAPSHOT.jar
```

Vous devriez voir: "Bonjour et bon courage dans votre projet en DevOps"

---

## Fichiers à personnaliser

Avant de pousser vers GitHub, modifiez ces fichiers:

### README.md
- Remplacez `[Votre Nom]` et `[Votre Prénom]`

### Jenkinsfile (ligne 10)
```groovy
git branch: 'main', url: 'https://github.com/VOTRE_USERNAME/Projet-DevOps-NomPrenom.git'
```
Remplacez `VOTRE_USERNAME` et `NomPrenom`

---

## Workflow GitHub Actions

Le fichier `.github/workflows/ci.yml` est déjà configuré. Il s'exécutera automatiquement:
- Lors des push sur `main` ou `dev`
- Lors des pull requests vers `main`

Pour tester:
1. Faites une modification sur la branche `dev`
2. Commit et push
3. Allez dans GitHub > Actions pour voir le workflow s'exécuter

---

## Configuration Jenkins

### Prérequis
- Jenkins installé et démarré
- Plugins installés: GitHub, Pipeline, Maven Integration, Slack Notification

### Étapes
1. Créer un nouveau Pipeline: `PipeLine-{{VotreNom}}{{VotrePrenom}}`
2. Configuration > Pipeline script from SCM
3. Repository: URL de votre dépôt GitHub
4. Script Path: `Jenkinsfile`
5. Sauvegarder et lancer "Build Now"

### Vue personnalisée
- Créer une vue "Pipeline Projects"
- Expression régulière: `.*PipeLine.*`

---

## Configuration Slack (optionnel mais recommandé)

1. Créer une application Slack sur https://api.slack.com/apps
2. Activer "Incoming Webhooks" ou "Bot Token Scopes"
3. Obtenir le token ou webhook URL
4. Configurer dans Jenkins: Manage Jenkins > Configure System > Slack
5. Ajouter les credentials
6. Canal par défaut: `#devops-notifications` (ou votre canal)

Le Jenkinsfile inclut déjà la configuration Slack. Assurez-vous que le plugin Slack est installé dans Jenkins.
