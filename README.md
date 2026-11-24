# TP3 : Pipeline CD Jenkins — Déploiement Multi-Environnements (Partie CD)

**Université Sidi Mohamed Ben Abdellah**  
**École Nationale des Sciences Appliquées de Fès (ENSA Fès)**  
**Filière ILIA / Semestre 5 — DevOps**

---

## 📌 Présentation du Projet
Ce projet constitue la seconde partie du TP3 DevOps : la mise en œuvre de la chaîne de **Déploiement Continu / Livraison Continue (CD)** avec **Jenkins**.

Ce pipeline applique le principe fondamental : **"Build once, deploy many times"**.  
Il ne recompile ni ne reconstruit le code : il télécharge l'artefact validé `Release.zip` depuis le dépôt Nexus (`python-release`), décompresse l'archive et la déploie successivement sur les différents environnements simulés (`C:\DEV`, `C:\TEST`, etc.) en effectuant des vérifications et tests fonctionnels post-déploiement.

---

## 📂 Structure du Répertoire
```
cd_project/
├── Jenkinsfile                # Définition du pipeline Jenkins CD
├── .gitignore                 # Fichiers ignorés par Git
└── README.md                  # Documentation du projet
```

---

## ⚙️ Détail du Pipeline Jenkins CD (`Jenkinsfile`)

Le pipeline déclaratif effectue les opérations suivantes :
1. **Nettoyage du Workspace** : `deleteDir()`.
2. **Téléchargement de la Release** : récupération de `Release.zip` depuis Nexus à l'aide de `curl` authentifié (`http://localhost:8081/repository/python-release/Release.zip`).
3. **Vérification du téléchargement** : contrôle de l'existence du fichier `Release.zip` avec message d'erreur explicite en cas d'absence.
4. **Décompression de la Release** : extraction de l'archive dans le dossier `Release/` avec `Expand-Archive`.
5. **Vérification de la Release** : contrôle du dossier extrait et de ses fichiers.
6. **Déploiement DEV** : création du dossier `C:\DEV` si inexistant et copie des fichiers de la Release avec `xcopy`.
7. **Vérification DEV** : listing du dossier et exécution de test de l'application.
8. **Déploiement TEST** : création du dossier `C:\TEST` et copie de la Release.
9. **Tests fonctionnels** : exécution automatique de la suite de tests sur l'environnement TEST (`python C:\TEST\test_calculator.py`).
10. **Post Actions** : notification de succès/échec et archivage de l'artefact `Release.zip` avec empreinte (fingerprint).

---

## 🖥️ Environnements Simulés

Les différents environnements de l'entreprise sont simulés localement par des dossiers racines :
- `C:\DEV` : environnement de développement
- `C:\TEST` : environnement de test fonctionnel / QA
- `C:\UAT` : validation utilisateur (recette)
- `C:\STAGING` : préproduction
- `C:\PRODUCTION` : production

---

## 🔄 Commandes Git

```bash
git remote add origin https://github.com/VOTRE_NOM/cd_project.git
git branch -M main
git push -u origin main
```
