
## 🎬 Introduction

L’objectif principal de ce projet est de surveiller, détecter et analyser des événements de sécurité provenant de systèmes **Linux et Windows**, dans un environnement cloud réaliste.

---

## 🌍 Contexte et objectifs

Aujourd’hui, les entreprises font face à une augmentation importante des menaces de cybersécurité, avec la multiplication des endpoints, des attaques de plus en plus sophistiquées, et des contraintes réglementaires strictes.

Dans ce contexte, l’objectif de cet atelier était de :
- Déployer une infrastructure SIEM/EDR complète sur AWS
- Comprendre le fonctionnement et la complémentarité entre SIEM et EDR
- Générer des événements de sécurité réalistes
- Analyser et corréler ces événements comme dans un SOC réel

---

## 🏗️ Architecture globale

L’architecture repose sur un **VPC AWS dédié**, dans lequel j’ai déployé trois instances EC2 :

- Un **serveur Wazuh**, qui centralise l’analyse et la visualisation
- Un **client Linux**, supervisé par un agent Wazuh
- Un **client Windows**, également supervisé par un agent Wazuh

Toutes les instances sont placées dans un **sous-réseau public**, avec une **Internet Gateway** attachée au VPC pour permettre l’accès Internet contrôlé.

Les communications entre les agents et le serveur Wazuh se font **via les adresses IP privées**, ce qui renforce la sécurité.

---

## 🔐 Sécurisation réseau

Pour limiter les accès réseau, j’ai configuré **trois groupes de sécurité distincts** :

- Un groupe pour le serveur Wazuh autorisant :
  - SSH pour l’administration
  - HTTPS pour l’accès au Dashboard
  - Les ports 1514 et 1515 pour la communication avec les agents

- Un groupe pour le client Linux autorisant uniquement SSH
- Un groupe pour le client Windows autorisant uniquement RDP

Cette configuration applique le principe du **moindre privilège**.

---

## ⚙️ Installation de Wazuh

Sur l’instance Ubuntu dédiée au serveur, j’ai installé **Wazuh en mode All-in-One**, incluant :
- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

Une fois l’installation terminée, le Dashboard est accessible via HTTPS et permet de superviser l’ensemble de l’infrastructure.

---

## 🔗 Enrôlement des agents

Ensuite, j’ai ajouté les agents :

- L’agent Linux a été installé via les commandes générées depuis le Dashboard
- L’agent Windows a été installé à l’aide du **fichier MSI généré par Wazuh**

Après résolution d’un conflit de nom d’agent, les deux agents apparaissent comme **actifs** dans le Dashboard et remontent correctement les événements.

---

## 🧪 Démonstration de sécurité – Exemple Windows

Je vais maintenant présenter **un exemple de test de sécurité**.

J’ai effectué plusieurs tentatives de connexion **RDP** sur le client Windows en utilisant **un nom d’utilisateur et un mot de passe incorrects**.

Cette action simule une tentative d’accès non autorisée ou une attaque par force brute.

Suite à ces tentatives, l’agent Wazuh détecte les événements Windows correspondants, notamment l’**Event ID 4625**, indiquant un échec d’authentification.

Ces événements sont transmis au serveur Wazuh, analysés, puis affichés dans le Dashboard sous forme d’alertes de sécurité.

---

## 📊 Analyse des alertes

Les alertes générées contiennent :
- Le compte ciblé
- L’adresse IP source
- Le type de connexion (RDP)
- Le niveau de gravité

La corrélation de plusieurs tentatives échouées permet d’identifier rapidement un comportement suspect.

---

## ✅ Conclusion

Pour conclure, ce projet démontre :
- Le déploiement d’un **SOC moderne dans le cloud**
- L’intégration efficace de **SIEM et EDR**
- La capacité de Wazuh à détecter des comportements malveillants sur Linux et Windows

Ce travail constitue une base solide pour aller plus loin, notamment vers l’automatisation des réponses, le SOAR et la détection avancée.

Merci pour votre attention.

---

