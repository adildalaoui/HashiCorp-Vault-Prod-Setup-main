# 🔐 Déploiement de HashiCorp Vault sur Amazon EKS pour la gestion sécurisée des secrets

## 📋 Présentation

Ce projet met en œuvre une plateforme complète de gestion des secrets basée sur **HashiCorp Vault** déployée sur **Amazon EKS (Elastic Kubernetes Service)**.

L'infrastructure est provisionnée avec **Terraform**, tandis que Vault est déployé en mode **Haute Disponibilité (HA)** avec le stockage intégré **Raft**. Les applications Kubernetes s'authentifient auprès de Vault grâce à la méthode **Kubernetes Authentication**, permettant de récupérer dynamiquement les secrets sans exposer d'informations sensibles dans les manifests Kubernetes, les ConfigMaps ou le code source.

---

# 🏗️ Architecture

```text
AWS
│
├── Amazon EKS
│
├── HashiCorp Vault (HA - 3 Pods)
│   │
│   ├── Stockage Raft
│   ├── Authentification Kubernetes
│   └── Vault Agent Injector
│
├── MySQL
│   │
│   └── Récupération des secrets depuis Vault
│
└── Application Spring Boot (BankApp)
    │
    └── Récupération dynamique des secrets depuis Vault
```

---

# 🚀 Technologies utilisées

## Cloud & Infrastructure

* AWS
* Amazon EKS
* Amazon EBS
* IAM
* OIDC Provider

## Infrastructure as Code

* Terraform

## Conteneurisation & Orchestration

* Docker
* Kubernetes
* kubectl
* Helm

## Gestion des secrets

* HashiCorp Vault
* Vault Agent Injector
* Kubernetes Authentication

## Stockage

* AWS EBS CSI Driver
* Persistent Volumes (PV)
* Persistent Volume Claims (PVC)

## Applications

* MySQL 8
* Application Spring Boot

---

# 🎯 Objectifs du projet

L'objectif principal est de démontrer une approche sécurisée de gestion des secrets dans Kubernetes en :

* Déployant une infrastructure Kubernetes sur AWS.
* Installant HashiCorp Vault en haute disponibilité.
* Centralisant la gestion des secrets.
* Évitant l'utilisation de mots de passe dans les manifests Kubernetes.
* Injectant automatiquement les secrets dans les pods applicatifs.
* Respectant le principe du moindre privilège (Least Privilege).

---

# ⚙️ Fonctionnalités mises en œuvre

## Provisionnement automatisé de l'infrastructure

Terraform permet la création automatisée des ressources AWS :

* VPC
* Subnets
* Internet Gateway
* Tables de routage
* Security Groups
* Cluster EKS
* Node Group
* Rôles et politiques IAM

---

## Déploiement d'un cluster EKS

Le cluster Kubernetes est entièrement déployé sur Amazon EKS avec :

* Gestion des nœuds managés (Managed Node Groups)
* Configuration kubectl
* Association du fournisseur OIDC
* Intégration avec AWS IAM

---

## Déploiement de Vault en Haute Disponibilité

Vault est installé via Helm avec :

* 3 réplicas
* Stockage intégré Raft
* Volumes persistants AWS EBS
* Interface Web activée
* Vault Agent Injector activé

Cette architecture permet d'assurer la continuité de service en cas de défaillance d'un pod Vault.

---

## Authentification Kubernetes

Vault est configuré pour faire confiance à l'API Kubernetes.

Flux d'authentification :

```text
Pod Kubernetes
        │
        ▼
Service Account
        │
        ▼
Vault Kubernetes Auth
        │
        ▼
Validation du Token
        │
        ▼
Application des Policies Vault
        │
        ▼
Accès aux secrets
```

---

## Gestion centralisée des secrets

Les secrets sont stockés dans le moteur KV Version 2 de Vault.

Exemple :

```bash
vault kv put secret/mysql \
MYSQL_DATABASE=bankappdb \
MYSQL_ROOT_PASSWORD=********
```

Secrets utilisés :

```text
secret/mysql
secret/frontend
```

---

## Injection automatique des secrets

Grâce à Vault Agent Injector, les applications récupèrent automatiquement leurs secrets.

Annotations Kubernetes :

```yaml
vault.hashicorp.com/agent-inject: "true"
vault.hashicorp.com/role: "vault-role"
```

Lors du démarrage du pod :

1. Vault Agent est injecté automatiquement.
2. Le pod s'authentifie auprès de Vault.
3. Les secrets sont récupérés.
4. Les secrets sont montés dans le conteneur applicatif.

Aucune information sensible n'est stockée dans les manifests Kubernetes.

---

# 🔒 Sécurité

Cette architecture permet d'éliminer plusieurs risques courants :

❌ Mots de passe codés en dur dans le code source

❌ Secrets stockés dans GitHub

❌ Secrets présents dans les fichiers YAML

❌ Distribution manuelle des identifiants

À la place :

✅ Gestion centralisée des secrets

✅ Contrôle d'accès basé sur des policies

✅ Authentification native Kubernetes

✅ Isolation des applications par namespace

✅ Principe du moindre privilège

---

# 🗄️ Gestion du stockage persistant

Le projet utilise le driver AWS EBS CSI pour la création dynamique de volumes persistants.

Fonctionnement :

```text
PersistentVolumeClaim
        │
        ▼
AWS EBS CSI Driver
        │
        ▼
Création automatique d'un volume EBS
        │
        ▼
Montage dans le pod MySQL
```

Cela garantit la persistance des données même en cas de redémarrage du pod.

---

# 📚 Compétences démontrées

## DevOps

* Infrastructure as Code (Terraform)
* Administration Kubernetes
* Gestion des déploiements avec Helm
* Automatisation de l'infrastructure

## Cloud Engineering

* Amazon EKS
* Réseaux AWS
* IAM
* Stockage persistant AWS

## Sécurité

* HashiCorp Vault
* Gestion des secrets
* Authentification Kubernetes
* Contrôle d'accès basé sur les rôles
* Bonnes pratiques Cloud Native

## Platform Engineering

* Haute disponibilité
* Applications Stateful
* Service Discovery
* Injection dynamique de secrets

---

# 📈 Résultats obtenus

* Déploiement automatisé d'un cluster Kubernetes sur AWS.
* Mise en place d'un cluster Vault HA avec stockage Raft.
* Gestion sécurisée des secrets centralisée.
* Injection automatique des secrets dans les applications.
* Déploiement d'une base MySQL avec stockage persistant.
* Déploiement d'une application Spring Boot consommant ses secrets depuis Vault.

---

# 🔮 Améliorations possibles

* Activation du TLS entre Vault et Kubernetes.
* Auto-Unseal avec AWS KMS.
* Rotation automatique des secrets.
* Intégration GitOps avec ArgoCD.
* CI/CD avec GitHub Actions.
* Supervision avec Prometheus et Grafana.
* Mise en place d'un plan de reprise après sinistre (Disaster Recovery).

---

**Compétences mises en pratique :**

Terraform • AWS • EKS • Kubernetes • Helm • HashiCorp Vault • IAM • OIDC • EBS CSI Driver • MySQL • Spring Boot • Cloud Security
