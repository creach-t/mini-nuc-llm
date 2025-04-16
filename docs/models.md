# Modèles recommandés et quantification

Cette section vous guide dans le choix, le téléchargement et la quantification des modèles pour votre Mini PC NUC avec processeur i5.

## Modèles recommandés

Pour un Mini PC avec 8 Go de RAM, nous recommandons les modèles suivants:

| Modèle | Taille | Forces | Lien | Quantification |
|--------|--------|--------|------|----------------|
| Mistral 7B Instruct | 7B | Polyvalent, performant | [Lien](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2) | Q4_K_M |
| Llama-2-7B-Chat | 7B | Usage conversationnel | [Lien](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf) | Q4_K_M |
| Phi-2 | 2.7B | Compact mais puissant | [Lien](https://huggingface.co/microsoft/phi-2) | Q4_K_M |
| TinyLlama | 1.1B | Très léger | [Lien](https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0) | Q4_K_M |

## Téléchargement des modèles

Plusieurs options s'offrent à vous pour obtenir les modèles:

### 1. Modèles pré-quantifiés de TheBloke

La méthode la plus simple consiste à télécharger des versions déjà quantifiées:

```bash
# Créer un répertoire pour les modèles
mkdir -p ~/llm-models
cd ~/llm-models

# Exemple pour Mistral 7B quantifié en GGUF
wget https://huggingface.co/TheBloke/Mistral-7B-Instruct-v0.2-GGUF/resolve/main/mistral-7b-instruct-v0.2.Q4_K_M.gguf
```

### 2. Télécharger et quantifier vous-même

Si vous préférez avoir plus de contrôle sur la quantification:

```bash
# Télécharger via HuggingFace CLI
pip install huggingface_hub
python -c "from huggingface_hub import snapshot_download; snapshot_download(repo_id='mistralai/Mistral-7B-Instruct-v0.2', local_dir='mistral-7b-original')"

# Convertir et quantifier avec llama.cpp
cd ~/llama.cpp
python convert.py ~/llm-models/mistral-7b-original \
    --outfile ~/llm-models/mistral-7b-instruct-v0.2.gguf \
    --outtype f16

# Quantifier le modèle converti
./build/bin/quantize \
    ~/llm-models/mistral-7b-instruct-v0.2.gguf \
    ~/llm-models/mistral-7b-instruct-v0.2.Q4_K_M.gguf \
    q4_k_m
```

## Types de quantification

| Type | Précision | Taille | Recommandé pour |
|------|-----------|--------|----------------|
| Q4_K_M | ≈4-bit | ≈3.3 Go pour un modèle 7B | Équilibre qualité/performance |
| Q5_K_M | ≈5-bit | ≈4.0 Go pour un modèle 7B | Meilleure qualité |
| Q3_K_M | ≈3-bit | ≈2.6 Go pour un modèle 7B | Économie de RAM |
| Q2_K | ≈2-bit | ≈2.0 Go pour un modèle 7B | Maximum d'économie, qualité réduite |

Pour un système avec 8 Go de RAM, la quantification Q4_K_M offre le meilleur rapport qualité/performance pour les modèles 7B.

## Script de téléchargement automatisé

Voici un script utilitaire pour télécharger plusieurs modèles populaires:

```bash
#!/bin/bash
# download_models.sh
MODELS_DIR=~/llm-models
mkdir -p $MODELS_DIR
cd $MODELS_DIR

# Array of models to download
declare -A MODELS=(
  ["mistral-7b"]="https://huggingface.co/TheBloke/Mistral-7B-Instruct-v0.2-GGUF/resolve/main/mistral-7b-instruct-v0.2.Q4_K_M.gguf"
  ["llama2-7b"]="https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGUF/resolve/main/llama-2-7b-chat.Q4_K_M.gguf"
  ["phi2"]="https://huggingface.co/TheBloke/phi-2-GGUF/resolve/main/phi-2.Q4_K_M.gguf"
  ["tinyllama"]="https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF/resolve/main/tinyllama-1.1b-chat-v1.0.Q4_K_M.gguf"
)

# Download each model
for model_name in "${!MODELS[@]}"; do
  url="${MODELS[$model_name]}"
  echo "Downloading $model_name from $url"
  wget -c "$url" -O "$model_name.Q4_K_M.gguf"
done

echo "All models downloaded successfully to $MODELS_DIR"
```

Rendez ce script exécutable et lancez-le:

```bash
chmod +x download_models.sh
./download_models.sh
```

## Test des modèles téléchargés

Pour vérifier que vos modèles fonctionnent correctement:

```bash
cd ~/llama.cpp
./build/bin/llama-cli \
  -m ~/llm-models/mistral-7b.Q4_K_M.gguf \
  -p "Explique-moi comment fonctionne un Mini PC NUC:" \
  -n 200 \
  --ctx-size 2048 \
  --temp 0.7
```

## Optimisations spécifiques aux modèles

### Mistral 7B

```bash
./build/bin/llama-cli \
  -m ~/llm-models/mistral-7b.Q4_K_M.gguf \
  --rope-scaling "linear" \
  --ctx-size 4096 \
  --threads 6 \
  --batch-size 512 \
  -p "[INST] Écris un résumé de l'importance des LLM locaux pour la vie privée [/INST]" \
  -n 300
```

### Llama-2-7B

```bash
./build/bin/llama-cli \
  -m ~/llm-models/llama2-7b.Q4_K_M.gguf \
  --repeat-penalty 1.1 \
  --ctx-size 4096 \
  --threads 6 \
  -p "[INST] Écris un résumé de l'importance des LLM locaux pour la vie privée [/INST]" \
  -n 300
```

## Prochaines étapes

Passez à la section [Configuration de l'interface utilisateur](webui.md) pour apprendre à mettre en place une interface conviviale pour interagir avec vos modèles.