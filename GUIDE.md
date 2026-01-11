# Guide étape par étape - Mini Projet DevOps

## Partie 1: Git, GitHub et GitHub Actions

### 1. Préparation de l'environnement de travail

#### Étape 1.1: Personnaliser le README.md
Ouvrez `README.md` et remplacez `[Votre Nom]` et `[Votre Prénom]` par vos informations.

#### Étape 1.2: Initialiser le dépôt Git local
```bash
git init
git add .
git commit -m "Initial commit: structure du projet Java/Maven"
```

#### Étape 1.3: Créer le dépôt GitHub
1. Allez sur https://github.com
2. Cliquez sur "New repository"
3. Nommez-le: `Projet-DevOps-{{VotreNom}}{{VotrePrenom}}` (remplacez par vos vrais noms)
4. Ne cochez PAS "Initialize this repository with a README" (vous avez déjà un README)
5. Cliquez sur "Create repository"

#### Étape 1.4: Lier le dépôt local à GitHub
```bash
git remote add origin https://github.com/VOTRE_USERNAME/Projet-DevOps-VotreNomVotrePrenom.git
git branch -M main
git push -u origin main
```

Remplacez `VOTRE_USERNAME` et `VotreNomVotrePrenom` par vos vraies valeurs.

#### Étape 1.5: Vérifier la branche main
```bash
git branch
```
Vous devriez être sur la branche `main`. Si ce n'est pas le cas:
```bash
git checkout -b main
git push -u origin main
```

### 2. Développement de l'application

L'application est déjà créée dans `src/main/java/com/devops/App.java`.

#### Étape 2.1: Vérifier que l'application fonctionne localement
```bash
mvn clean compile
mvn exec:java -Dexec.mainClass="com.devops.App"
```

Vous devriez voir: "Bonjour et bon courage dans votre projet en DevOps"

#### Étape 2.2: Commit et push vers main
Si vous avez fait des modifications:
```bash
git add .
git commit -m "Application Java simple créée"
git push origin main
```

#### Étape 2.3: Créer la branche dev
```bash
git checkout -b dev
git push -u origin dev
```

### 3. Mise en place de GitHub Actions

#### Étape 3.1: Ajouter des modifications à la branche dev
1. Assurez-vous d'être sur la branche dev:
```bash
git checkout dev
```

2. Faites une petite modification (par exemple, ajoutez un commentaire dans App.java)

3. Commit et push:
```bash
git add .
git commit -m "Modification sur la branche dev"
git push origin dev
```

#### Étape 3.2: Le workflow GitHub Actions
Le workflow est déjà créé dans `.github/workflows/ci.yml`. Il sera automatiquement déclenché lors des push et pull requests.

#### Étape 3.3: Créer une Pull Request
1. Allez sur votre dépôt GitHub
2. Cliquez sur "Pull requests"
3. Cliquez sur "New pull request"
4. Sélectionnez `dev` comme source et `main` comme destination
5. Donnez un titre: "Merge dev into main"
6. Cliquez sur "Create pull request"
7. Le workflow GitHub Actions devrait se déclencher automatiquement

---

## Partie 2: Jenkins

### 1. Mise en place de Jenkins

#### Étape 1.1: Installer et démarrer Jenkins
Si Jenkins n'est pas installé:
- Windows: Téléchargez et installez depuis https://www.jenkins.io/download/
- Ou utilisez Docker:
```bash
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts
```

Accédez à http://localhost:8080

#### Étape 1.2: Configurer le projet Jenkins
1. Connectez-vous à Jenkins
2. Cliquez sur "New Item"
3. Nommez-le: `PipeLine-{{VotreNom}}{{VotrePrenom}}`
4. Sélectionnez "Pipeline"
5. Cliquez sur "OK"

#### Étape 1.3: Configurer le projet pour surveiller GitHub
Dans la configuration du projet:
1. Dans "Pipeline Definition", sélectionnez "Pipeline script from SCM"
2. SCM: Git
3. Repository URL: `https://github.com/VOTRE_USERNAME/Projet-DevOps-VotreNomVotrePrenom.git`
4. Branches to build: `*/main`
5. Script Path: `Jenkinsfile`
6. Cliquez sur "Save"

#### Étape 1.4: Créer une vue personnalisée
1. Cliquez sur "+" à côté de "All"
2. Nommez-la: "Pipeline Projects"
3. Sélectionnez "List View"
4. Cochez "Use a regular expression to include jobs into this view"
5. Expression: `.*PipeLine.*`
6. Cliquez sur "OK"

### 2. Automatisation du Build et Test

#### Étape 2.1: Installer les plugins nécessaires
1. Manage Jenkins > Manage Plugins
2. Installez:
   - GitHub plugin
   - Pipeline plugin
   - Maven Integration plugin
   - Slack Notification plugin (pour la partie 4)

#### Étape 2.2: Configurer Maven dans Jenkins
1. Manage Jenkins > Global Tool Configuration
2. Ajoutez Maven (version recommandée: 3.9.x)
3. Configurez Java (JDK 11)

#### Étape 2.3: Mettre à jour le Jenkinsfile
Ouvrez le `Jenkinsfile` et remplacez:
- `VOTRE_USERNAME` par votre nom d'utilisateur GitHub
- `Projet-DevOps-NomPrenom` par le nom réel de votre dépôt
- Le chemin JAVA_HOME si nécessaire

#### Étape 2.4: Exécuter le Pipeline
1. Allez sur votre projet Jenkins
2. Cliquez sur "Build Now"
3. Surveillez l'exécution dans "Console Output"

### 3. Déploiement

L'étape Archive est déjà dans le Jenkinsfile. L'étape Deploy est optionnelle et peut être configurée pour déployer localement ou sur un cloud.

### 4. Livraison - Notification Slack

#### Étape 4.1: Configurer Slack dans Jenkins
1. Manage Jenkins > Configure System
2. Section "Slack"
3. Configurez:
   - Workspace: votre workspace Slack
   - Credential: ajoutez votre token Slack
   - Default Channel: #devops-notifications (ou votre canal)

#### Étape 4.2: Le Jenkinsfile inclut déjà la notification Slack
Vérifiez que la section "Notify Slack" est présente dans votre Jenkinsfile.

---

## Vérifications finales

1. ✅ Repository GitHub créé et lié
2. ✅ Branche main avec commits
3. ✅ Branche dev créée
4. ✅ GitHub Actions workflow fonctionne
5. ✅ Pull Request créée
6. ✅ Jenkins projet configuré
7. ✅ Pipeline Jenkins fonctionne
8. ✅ Vue personnalisée créée
9. ✅ Notifications Slack configurées

---

## Notes importantes

- Remplacez tous les placeholders (VOTRE_USERNAME, NomPrénom, etc.) par vos vraies valeurs
- Pour Slack, vous devez créer une application Slack et obtenir un token
- Le Jenkinsfile peut nécessiter des ajustements selon votre configuration Jenkins
- Assurez-vous que Maven et Java sont installés sur votre machine Jenkins
