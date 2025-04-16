# Configuration du système

Cette section détaille la préparation de votre Mini PC NUC pour obtenir les meilleures performances lors de l'exécution de modèles de langage.

## Installation du système d'exploitation

Pour ce projet, nous recommandons **Ubuntu Server 22.04 LTS** ou **Debian 12**, qui offrent un bon équilibre entre performances et facilité d'utilisation. L'installation d'une version serveur (sans interface graphique) permettra de libérer plus de ressources pour les modèles.

### Procédure d'installation

1. Téléchargez l'image [Ubuntu Server 22.04 LTS](https://ubuntu.com/download/server)
2. Créez une clé USB bootable avec [Balena Etcher](https://www.balena.io/etcher/)
3. Démarrez sur la clé USB et suivez les instructions d'installation
4. Choisissez l'installation minimale sans environnement de bureau

## Optimisations système

### Configuration du swap

Pour permettre l'exécution de modèles plus grands, nous allons configurer un swap généreux sur le SSD:

```bash
# Vérifier l'espace disque disponible
df -h

# Créer un fichier swap de 16 Go
sudo fallocate -l 16G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Rendre le swap permanent
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Vérifier l'activation du swap
free -h
```

### Optimisation du swap pour SSD

Ajustez les paramètres de swap pour optimiser l'utilisation sur SSD:

```bash
# Réduire la tendance du système à utiliser le swap
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf

# Augmenter la compression du cache
echo 'vm.vfs_cache_pressure=50' | sudo tee -a /etc/sysctl.conf

# Appliquer les changements
sudo sysctl -p
```

### Configuration du CPU

Pour optimiser les performances du processeur i5-10210U:

```bash
# Installer les outils nécessaires
sudo apt update
sudo apt install -y cpufrequtils

# Définir le gouverneur de fréquence sur 'performance'
echo 'GOVERNOR="performance"' | sudo tee /etc/default/cpufrequtils
sudo systemctl restart cpufrequtils

# Vérifier le gouverneur actuel
cpufreq-info
```

## Dépendances système essentielles

Installez les packages système nécessaires:

```bash
sudo apt update
sudo apt install -y \
    build-essential \
    cmake \
    git \
    python3 \
    python3-pip \
    python3-venv \
    ninja-build \
    pkg-config \
    libopenblas-dev \
    libcurl4-openssl-dev \
    curl \
    wget \
    htop \
    ufw
```

## Configuration réseau (optionnel)

Si vous souhaitez accéder à votre serveur LLM depuis d'autres appareils:

```bash
# Configurer le pare-feu
sudo ufw allow ssh
sudo ufw allow 8080/tcp  # Port pour l'interface web
sudo ufw enable

# Vérifier la configuration du pare-feu
sudo ufw status
```

## Optimisation mémoire

Désactivez les services inutiles pour libérer de la RAM:

```bash
# Lister les services en cours d'exécution
systemctl list-units --type=service --state=running

# Désactiver les services non essentiels
sudo systemctl disable --now snapd
sudo systemctl disable --now packagekit
sudo systemctl disable --now ModemManager
sudo systemctl disable --now wpa_supplicant  # Si connexion Ethernet uniquement
```

## Configuration de l'environnement Python

```bash
# Créer un environnement virtuel
python3 -m venv ~/llm-env

# Activer l'environnement
source ~/llm-env/bin/activate

# Installer les dépendances Python de base
pip install --upgrade pip setuptools wheel
```

## Prochaines étapes

Une fois votre système configuré, vous pouvez passer à [l'installation des dépendances](dependencies.md) pour les modèles LLM.