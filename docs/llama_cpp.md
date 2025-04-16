# Installation et configuration de llama.cpp

[llama.cpp](https://github.com/ggerganov/llama.cpp) est une implémentation hautement optimisée en C/C++ des modèles LLM de type Llama. C'est le moteur idéal pour faire fonctionner des modèles sur notre Mini PC NUC avec des performances optimales.

## Installation depuis les sources

La méthode recommandée consiste à compiler llama.cpp depuis les sources pour bénéficier des optimisations spécifiques à votre processeur:

```bash
# Cloner le dépôt
git clone https://github.com/ggerganov/llama.cpp.git
cd llama.cpp

# Compiler avec optimisations pour processeurs modernes
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DLLAMA_NATIVE=ON
cmake --build . --config Release -j4

# Vérifier l'installation
./bin/llama-cli --help
```

## Options de compilation recommandées

Pour un Intel i5-10210U, nous recommandons les options de compilation suivantes:

```bash
cmake .. -DCMAKE_BUILD_TYPE=Release \
   -DLLAMA_NATIVE=ON \
   -DLLAMA_AVX=ON \
   -DLLAMA_AVX2=ON \
   -DLLAMA_F16C=ON \
   -DLLAMA_FMA=ON \
   -DLLAMA_BLAS=ON \
   -DLLAMA_BLAS_VENDOR=OpenBLAS
```

Ces options activent les instructions spécifiques au processeur (AVX2, F16C, FMA) et utilisent OpenBLAS pour accélérer les calculs matriciels.

## Installation des bindings Python (optionnel)

Si vous prévoyez d'utiliser llama.cpp avec Python:

```bash
# Installer les bindings Python
cd ..  # Revenir à la racine du dépôt llama.cpp
pip install -e .

# Tester l'installation
python3 -c "import llama_cpp; print(llama_cpp.__version__)"
```

## Configuration du serveur API

llama.cpp propose un serveur compatible avec l'API OpenAI:

```bash
# Compiler le serveur
cd llama.cpp
make server

# Exécuter le serveur avec un modèle
./build/bin/server \
  -m ./models/llama-2-7b-chat.Q4_K_M.gguf \
  --host 0.0.0.0 \
  --port 8080 \
  --context-size 2048 \
  --threads 4 \
  --n-gpu-layers 0
```

## Vérification de l'installation

Pour vérifier que llama.cpp fonctionne correctement:

```bash
# Télécharger un petit modèle de test (si ce n'est pas déjà fait)
cd llama.cpp
./models/download-ggml-model.sh phi2

# Exécuter un test simple
./build/bin/llama-cli \
  -m ./models/phi2.gguf \
  -p "Explique-moi comment fonctionne un LLM:" \
  -n 100 \
  --ctx-size 512 \
  --temp 0.7 \
  --seed 42
```

## Options de performance importantes

### Paramètres clés

| Paramètre | Description | Valeur recommandée |
|-----------|-------------|-------------------|
| --threads | Nombre de threads CPU | 4-8 (selon votre CPU) |
| --ctx-size / --context-size | Taille du contexte | 2048-4096 |
| --batch-size | Taille des lots | 512 |
| --n-gpu-layers | Couches à exécuter sur GPU | 0 (CPU uniquement) |
| --rope-scaling | Mise à l'échelle RoPE | "linear" |
| --keep | Tokens à conserver en mémoire | 64 |

### Exemple d'utilisation optimisée

```bash
./build/bin/llama-cli \
  -m ./models/mistral-7b-instruct-v0.2.Q4_K_M.gguf \
  -p "Écris un poème sur l'intelligence artificielle:" \
  -n 300 \
  --ctx-size 2048 \
  --threads 6 \
  --batch-size 512 \
  --temp 0.7 \
  --top-p 0.9 \
  --repeat-penalty 1.1
```

## Gestion des modèles

Nous recommandons de stocker vos modèles dans un répertoire dédié:

```bash
# Créer un répertoire pour les modèles
mkdir -p ~/llm-models

# Créer des liens symboliques pour faciliter l'accès
ln -s ~/llm-models llama.cpp/models
```

## Prochaines étapes

Passez à la section [Téléchargement et quantification des modèles](models.md) pour apprendre à obtenir et optimiser des modèles pour votre Mini PC.