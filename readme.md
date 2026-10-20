<div align="center">

# CertESSCA

**Installateur macOS en un clic**, au design verre dépoli lilas et rose.

![macOS](https://img.shields.io/badge/macOS-13%2B-9ea3eb?style=flat-square)
![Swift](https://img.shields.io/badge/Swift-5.9-eda3b3?style=flat-square)
![SwiftUI](https://img.shields.io/badge/SwiftUI-100%25-73d99e?style=flat-square)

</div>

---

## Présentation

L'installateur récupère automatiquement la **dernière version** de CertESSCA publiée sur GitHub, la télécharge avec une barre de progression, puis l'installe dans le dossier **Applications**. Il suffit d'un clic.

- Détection de la dernière release via l'API GitHub
- Téléchargement avec progression en temps réel
- Prise en charge des archives `.zip` et `.dmg`
- Remplacement propre d'une ancienne version (l'ancienne est mise à la corbeille)
- Lancement automatique de l'application une fois installée
- Messages d'erreur clairs et bouton **Réessayer**

## Installation

1. Rends-toi dans l'onglet [**Releases**](../../releases/latest) du dépôt.
2. Télécharge le fichier `Installer.zip`.
3. Double-clique pour le décompresser, puis ouvre **Installer.app**.
4. Clique sur **Installer**.

> **Premier lancement :** l'application n'étant pas notarisée par Apple, macOS peut afficher un avertissement.
> Fais un **clic droit sur Installer.app, puis Ouvrir**, et confirme. Cette étape n'est nécessaire qu'une seule fois.
> Si besoin : **Réglages Système, Confidentialité et sécurité, Ouvrir quand même**.

## Compiler soi-même

Prérequis : un Mac avec Xcode ou les outils en ligne de commande (`xcode-select --install`).

```bash
git clone https://github.com/Intouchable0312/CertESSCA.git
cd CertESSCA
./build_app.sh
```

Le script produit `Installer.app` (binaire universel Apple Silicon et Intel). Pour le distribuer :

```bash
ditto -c -k --keepParent Installer.app Installer.zip
```

## Configuration

Tous les réglages se trouvent dans `Sources/Installer/Config.swift` :

| Option | Rôle |
| --- | --- |
| `owner` / `repo` | Dépôt GitHub d'où l'application est téléchargée (`Intouchable0312` / `CertESSCA`) |
| `appDisplayName` | Nom affiché dans la fenêtre |
| `appBundleName` | Nom exact du `.app` attendu dans l'archive (vide = premier `.app` trouvé) |
| `assetNameContains` | Filtre sur le nom du fichier de la release (par exemple `arm64`) |
| `installDirectory` | Dossier d'installation (`/Applications`, avec repli sur `~/Applications`) |
| `removeQuarantine` | Retire l'attribut de quarantaine après installation |
| `launchAfterInstall` | Ouvre l'application une fois installée |

### Personnaliser le logo

Dépose un fichier `logo.png` (ou `.jpg`) dans `Sources/Installer/Resources/`, puis relance `./build_app.sh`. Sans logo, un cadenas aux couleurs du thème est affiché.

## Publier une nouvelle version

```bash
ditto -c -k --keepParent MonApp.app MonApp.zip
gh release create v1.0.1 MonApp.zip --title "v1.0.1" --notes "Notes de version"
```

L'installateur prend toujours la release la plus récente. Le dépôt doit rester **public** et la release ne doit pas être un brouillon.

## Structure du projet

```
Installer/
├── Package.swift
├── build_app.sh
└── Sources/Installer/
    ├── Theme.swift            # Thème : couleurs, verre, boutons, composants
    ├── Config.swift           # Réglages du dépôt et de l'installation
    ├── InstallerModel.swift   # Récupération GitHub, téléchargement, installation
    ├── InstallerApp.swift     # Interface SwiftUI
    └── Resources/             # logo.png
```

## Dépannage

| Problème | Solution |
| --- | --- |
| « Aucune release trouvée » | Vérifie que la release est publiée (pas en brouillon) et que le dépôt est public |
| « Aucun fichier compatible » | La release doit contenir un `.zip` ou un `.dmg` |
| « Aucune application .app trouvée » | L'archive doit contenir un `.app`, créé avec `ditto -c -k --keepParent` |
| L'app ne s'ouvre pas | Clic droit, puis Ouvrir (voir le premier lancement ci-dessus) |

---

<div align="center">

Fait avec SwiftUI par **Intouchable0312**

</div>
