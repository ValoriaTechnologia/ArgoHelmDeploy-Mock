# ArgoHelmDeploy Mock Repository

Dépôt Git minimal destiné à tester l’action GitHub **ArgoHelmDeploy**. Ce repo simule un dépôt ArgoCD que l’action clone, modifie (mise à jour de `spec.source.targetRevision`), puis pousse.

## Structure du dépôt

```
ArgoHelmDeploy-Mock/
├── packages.yaml      # Déclaration des packages (nom + path)
├── application.yaml   # Manifest ArgoCD Application (chart, targetRevision)
├── README.md
└── scripts/
    └── init-repo.sh   # Optionnel : initialisation Git sans historique
```

- **packages.yaml** (à la racine) : liste des packages au format `name` + `path`. L’action utilise ce fichier pour trouver le package par nom et résoudre le chemin vers le manifest Application.
- **application.yaml** (à la racine) : manifest ArgoCD `kind: Application` avec `spec.source.chart` et `spec.source.targetRevision`. C’est ce fichier que l’action met à jour avant commit et push.

## Utilisation comme repo mock

### Cloner le dépôt

```bash
git clone <URL_DU_REPO>
cd ArgoHelmDeploy-Mock
```

### Données pour tester l’action

| Donnée | Valeur à utiliser |
|--------|-------------------|
| **package-name** | `argo-app` (défini dans `packages.yaml`) |
| **package-file-path** | `application.yaml` ou `./application.yaml` (fichier Application dans le `path` du package, ici `./`) |

### Scénario de test

1. L’action clone ce dépôt.
2. Elle lit `packages.yaml` et trouve le package par nom (`argo-app`).
3. Elle résout le `path` du package (ici `./`) vers le répertoire contenant le manifest Application.
4. Elle met à jour `spec.source.targetRevision` dans `application.yaml` (ex. vers une nouvelle version).
5. Elle effectue un commit et un push.

Le dépôt est minimal et prêt à servir de cible pour une action qui attend un repo avec `packages.yaml` et un manifest ArgoCD Application.

## Initialisation du dépôt

Si vous recevez ce mock sans historique Git (zip, copie de fichiers), initialisez le dépôt avec :

```bash
git init
git add packages.yaml application.yaml README.md scripts/
git commit -m "Initial mock for ArgoHelmDeploy"
```

Sous Linux/macOS, vous pouvez aussi exécuter le script fourni :

```bash
./scripts/init-repo.sh
```

Sous Windows (PowerShell) :

```powershell
.\scripts\init-repo.ps1
```
