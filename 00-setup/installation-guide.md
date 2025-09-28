# 00-setup - Installation et Configuration

## 📋 Vue d'ensemble

Ce module vous guide dans l'installation et la configuration de tous les outils nécessaires pour suivre le laboratoire Kubernetes.

## 🛠️ Outils Requis

### Obligatoires
- **kubectl** - Client en ligne de commande Kubernetes
- **Un cluster Kubernetes local** (choisir une option) :
    - minikube (recommandé pour débuter)
    - kind (Kubernetes in Docker)
    - Docker Desktop avec Kubernetes

### Optionnels mais Recommandés
- **Helm** - Gestionnaire de packages Kubernetes
- **kubectx/kubens** - Changement rapide de contexte
- **k9s** - Interface TUI pour Kubernetes
- **Visual Studio Code** avec extensions Kubernetes

## 🚀 Installation par OS

### 🐧 Linux (Ubuntu/Debian)

#### 1. Installation de kubectl
```bash
# Télécharger la dernière version
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Installer kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Vérifier l'installation
kubectl version --client
```

#### 2. Installation de minikube
```bash
# Télécharger minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

# Installer minikube
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Démarrer minikube
minikube start --driver=docker --cpus=2 --memory=4096
```

#### 3. Installation de Helm
```bash
# Télécharger et installer Helm
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

### 🍎 macOS

#### 1. Installation avec Homebrew
```bash
# Installer kubectl
brew install kubectl

# Installer minikube
brew install minikube

# Installer Helm
brew install helm

# Installer les outils optionnels
brew install kubectx k9s
```

#### 2. Démarrer minikube
```bash
minikube start --driver=hyperkit --cpus=2 --memory=4096
```

### 🪟 Windows

#### 1. Installation avec Chocolatey
```powershell
# Installer kubectl
choco install kubernetes-cli

# Installer minikube
choco install minikube

# Installer Helm
choco install kubernetes-helm
```

#### 2. Ou avec winget
```powershell
# Installer kubectl
winget install kubectl

# Installer minikube
winget install minikube
```

## 🐳 Options de Cluster Local

### Option 1: minikube (Recommandé pour débuter)

**Avantages :**
- Interface simple et intuitive
- Addons intégrés (dashboard, ingress, etc.)
- Support multi-driver (Docker, VirtualBox, Hyper-V)

```bash
# Démarrer avec configuration optimale
minikube start \
  --driver=docker \
  --cpus=2 \
  --memory=4096 \
  --disk-size=20g \
  --kubernetes-version=v1.28.0

# Activer des addons utiles
minikube addons enable dashboard
minikube addons enable ingress
minikube addons enable metrics-server

# Accéder au dashboard
minikube dashboard
```

### Option 2: kind (Kubernetes in Docker)

**Avantages :**
- Très léger et rapide
- Multi-node clusters
- CI/CD friendly

```bash
# Installer kind
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Créer un cluster
kind create cluster --name k8s-lab

# Cluster multi-node (optionnel)
cat <<EOF | kind create cluster --name k8s-lab-multi --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
EOF
```

### Option 3: Docker Desktop

**Avantages :**
- Intégration native avec Docker
- Interface graphique
- Facile pour débuter

1. Ouvrir Docker Desktop
2. Aller dans Settings > Kubernetes
3. Cocher "Enable Kubernetes"
4. Cliquer "Apply & Restart"

## ✅ Vérification de l'Installation

### Script de Vérification Automatique

```bash
#!/bin/bash
# check-installation.sh

echo "🔍 Vérification de l'installation Kubernetes..."

# Vérifier kubectl
if command -v kubectl &> /dev/null; then
    echo "✅ kubectl installé : $(kubectl version --client --short)"
else
    echo "❌ kubectl non trouvé"
    exit 1
fi

# Vérifier la connexion au cluster
if kubectl cluster-info &> /dev/null; then
    echo "✅ Connexion au cluster réussie"
    kubectl get nodes
else
    echo "❌ Impossible de se connecter au cluster"
    exit 1
fi

# Vérifier Helm (optionnel)
if command -v helm &> /dev/null; then
    echo "✅ Helm installé : $(helm version --short)"
else
    echo "⚠️ Helm non installé (optionnel)"
fi

# Tester la création d'un pod
echo "🧪 Test de création d'un pod..."
kubectl run test-pod --image=nginx:alpine --restart=Never --rm -i --tty -- echo "Hello Kubernetes"

if [ $? -eq 0 ]; then
    echo "✅ Test réussi ! Votre environnement est prêt."
else
    echo "❌ Test échoué"
    exit 1
fi
```

### Tests Manuels

```bash
# 1. Vérifier les versions
kubectl version
helm version

# 2. Vérifier l'état du cluster
kubectl cluster-info
kubectl get nodes -o wide

# 3. Tester la création de ressources
kubectl create namespace test-lab
kubectl get namespaces

# 4. Nettoyer
kubectl delete namespace test-lab
```

## 🔧 Configuration et Outils Utiles

### Configuration de kubectl

```bash
# Voir la configuration actuelle
kubectl config view

# Changer de contexte (si plusieurs clusters)
kubectl config use-context minikube

# Définir un namespace par défaut
kubectl config set-context --current --namespace=default
```

### Installation d'outils supplémentaires

```bash
# kubectx et kubens (changement rapide de contexte)
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens

# k9s (interface TUI)
curl -sS https://webinstall.dev/k9s | bash

# stern (logs multi-pods)
brew install stern  # macOS
# ou
wget https://github.com/stern/stern/releases/download/v1.25.0/stern_1.25.0_linux_amd64.tar.gz
```

### Configuration VS Code

Extensions recommandées :
- **Kubernetes** par Microsoft
- **YAML** par Red Hat
- **Docker** par Microsoft
- **Helm Intellisense** par Tim Hansen



## 🎯 Premier Test Complet

```yaml
# test-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-kubernetes
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello-kubernetes
  template:
    metadata:
      labels:
        app: hello-kubernetes
    spec:
      containers:
      - name: hello-kubernetes
        image: paulbouwer/hello-kubernetes:1.10
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: hello-kubernetes-svc
spec:
  selector:
    app: hello-kubernetes
  ports:
  - port: 80
    targetPort: 8080
  type: ClusterIP
```

```bash
# Déployer l'application de test
kubectl apply -f test-deployment.yaml

# Vérifier le déploiement
kubectl get deployments
kubectl get pods
kubectl get services

# Tester l'accès (minikube)
minikube service hello-kubernetes-svc

# Nettoyer
kubectl delete -f test-deployment.yaml
```

## 📚 Ressources Supplémentaires

### Documentation Officielle
- [Documentation Kubernetes](https://kubernetes.io/docs/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [Helm Documentation](https://helm.sh/docs/)

### Outils de Développement
- **Skaffold** - Développement local avec hot reload
- **Tilt** - Interface graphique pour le développement
- **Lens** - IDE Kubernetes avec interface graphique

### Communauté
- [Kubernetes Slack](https://kubernetes.slack.com)
- [Reddit r/kubernetes](https://reddit.com/r/kubernetes)
- [CNCF Community](https://community.cncf.io)

## 🚨 Dépannage Courant

### Problème : minikube ne démarre pas
```bash
# Vérifier Docker
docker version

# Supprimer et recréer le cluster
minikube delete
minikube start --driver=docker
```

### Problème : kubectl command not found
```bash
# Vérifier le PATH
echo $PATH

# Réinstaller kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install kubectl /usr/local/bin/
```

### Problème : Impossible de se connecter au cluster
```bash
# Vérifier la configuration
kubectl config current-context
kubectl config view

# Réinitialiser la configuration
kubectl config use-context minikube
```

---

**✅ Une fois l'installation terminée, vous êtes prêt à passer au [Module 1: Introduction à Kubernetes](../01-beginner/01-introduction/)!**