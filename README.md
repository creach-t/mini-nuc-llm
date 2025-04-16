# 🧠 LLM Local sur Mini PC NUC

![Banner](https://raw.githubusercontent.com/creach-t/mini-nuc-llm/main/images/banner.jpg)

> *"L'intelligence artificielle à portée de main, sans connexion internet, sans abonnement, dans votre salon."*

## 📋 Vue d'ensemble

Ce projet vous guide dans l'installation et l'exécution de grands modèles de langage (LLM) sur un Mini PC NUC avec processeur Intel i5. Contrairement aux solutions cloud, cette approche vous permet de :

- **Garder vos données privées** - Tout reste sur votre machine
- **Fonctionner hors ligne** - Pas besoin d'internet pour générer du texte
- **Économiser** - Pas d'abonnement mensuel à payer
- **Contrôler** - Personnalisez et ajustez selon vos besoins

Notre configuration cible :
- 🖥️ **Mini PC ALTIK** avec Intel i5-10210U (ou similaire)
- 💾 **8 Go de RAM** et **SSD 500 Go**
- 📦 **Format compact** qui se glisse n'importe où
- ⚡ **Consommation modérée** (15-35W)

## 🎯 Objectifs du projet

- Installer un système optimisé pour l'IA
- Configurer llama.cpp pour une inférence efficace
- Installer une interface web conviviale 
- Télécharger et quantifier des modèles populaires
- Créer un système de démarrage automatique

## 🚀 Démarrage rapide

```bash
# Cloner ce dépôt
git clone https://github.com/creach-t/mini-nuc-llm.git
cd mini-nuc-llm

# Lancer le script d'installation
./install.sh
```

## 🏗️ Architecture du projet

```
mini-nuc-llm/
├── install.sh            # Script d'installation principal
├── scripts/              # Scripts utilitaires
│   ├── setup_system.sh   # Configuration système
│   ├── install_llm.sh    # Installation des modèles
│   └── setup_webui.sh    # Configuration interface web
├── models/               # Dossier pour stocker les modèles
├── webui/                # Interface utilisateur web
└── docs/                 # Documentation détaillée
```

## 🛠️ Matériel recommandé

Notre configuration recommandée:

| Composant | Spécification | Notes |
|-----------|---------------|-------|
| Processeur | Intel i5-10210U | 4 cœurs/8 threads, jusqu'à 4.2 GHz |
| RAM | 8 Go+ | 16 Go optimal pour modèles 13B |
| Stockage | SSD 500 Go | Nécessaire pour les modèles et swap |
| Système | Ubuntu/Debian | Ou Windows avec WSL2 |
| GPU | Intégré Intel UHD | Suffisant pour les petits modèles |

## 📊 Performances attendues

| Modèle | Taille | Quantification | Temps moyen/token | Notes |
|--------|--------|----------------|-------------------|-------|
| Llama2 | 7B | Q4_K_M | ~50-80 ms | Usage général |
| Mistral | 7B | Q4_K_M | ~40-75 ms | Performant |
| Phi-2 | 2.7B | Q4_K_M | ~20-40 ms | Efficace mais limité |
| TinyLlama | 1.1B | Q4_K_M | ~10-25 ms | Très rapide mais basique |

## 📋 Étapes détaillées

### 1️⃣ Préparation du système
Voir [docs/system_setup.md](docs/system_setup.md)

### 2️⃣ Installation des dépendances
Voir [docs/dependencies.md](docs/dependencies.md)

### 3️⃣ Installation et compilation de llama.cpp
Voir [docs/llama_cpp.md](docs/llama_cpp.md)

### 4️⃣ Téléchargement et quantification des modèles
Voir [docs/models.md](docs/models.md)

### 5️⃣ Configuration de l'interface utilisateur
Voir [docs/webui.md](docs/webui.md)

### 6️⃣ Optimisations avancées
Voir [docs/optimizations.md](docs/optimizations.md)

## 📱 Interfaces disponibles

- **[llama.cpp server](https://github.com/ggerganov/llama.cpp/tree/master/examples/server)** - API compatible OpenAI
- **[text-generation-webui](https://github.com/oobabooga/text-generation-webui)** - Interface web complète
- **[LM Studio](https://lmstudio.ai/)** - Application desktop intuitive
- **Simple terminal** - Via l'exemple main de llama.cpp

## 💡 Conseils et astuces

- **Optimisez le swap** pour les modèles plus gros
- **Ajustez les paramètres de contexte** pour économiser la RAM
- **Utilisez les modèles quantifiés** pour de meilleures performances
- **Essayez différents modèles** pour trouver le meilleur compromis

## 🔄 Mise à jour

```bash
git pull
./update.sh
```

## 🤝 Contribution

Les contributions sont bienvenues ! Voir [CONTRIBUTING.md](CONTRIBUTING.md) pour les détails.

## 📜 Licence

Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails.

## ⭐ Si ce projet vous est utile...

N'hésitez pas à laisser une étoile sur GitHub et à partager avec ceux qui pourraient être intéressés !

---

*Créé avec ❤️ pour rendre l'IA accessible à tous.*