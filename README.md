# 🏗️ Infrastructure Hyperconvergée Haute Disponibilité — Print&Rest

> **Projet Académique** | Licence en Informatique & Cybersécurité  
> Refonte d'une infrastructure virtualisée, sécurisée et hautement disponible pour une imprimerie.

---

## 📋 Résumé du Projet

**Print&Rest** est une imprimerie traditionnelle spécialisée dans la production de supports publicitaires, livres, brochures, flyers et packaging. Son infrastructure informatique reposait sur un **serveur unique** sans redondance, entraînant des problèmes de performance, des vulnérabilités de sécurité et des risques critiques de perte de données.

Ce projet consiste à **concevoir, sécuriser et déployer** une infrastructure virtualisée haute disponibilité pour répondre à ces problématiques, sur deux bâtiments reliés par fibre optique.

---

## 🎯 Objectifs

- ✅ Mettre en place une **infrastructure virtuelle hautement disponible**
- ✅ Implémenter un **cluster Failover S2D Windows Server** pour la continuité des services critiques
- ✅ Déployer deux **contrôleurs de domaine Active Directory** redondants
- ✅ Configurer une **double redondance de pare-feux pfSense** avec basculement CARP
- ✅ Installer et configurer un **SIEM** (Elastic Stack) pour la détection d'événements de sécurité
- ✅ Mettre en œuvre un **serveur de sauvegarde centralisé** (UrBackup)
- ✅ Appliquer les bonnes pratiques de cybersécurité (segmentation, supervision, alertes)
- ✅ Documenter l'ensemble de l'environnement et des procédures

---

## 🧱 Architecture Générale

L'infrastructure s'étend sur deux bâtiments :

| Bâtiment | Rôle |
|----------|------|
| **Bâtiment 1** | Administration — commandes, comptabilité, service client |
| **Bâtiment 2** | Production — impression, façonnage, gestion des machines |

### Composants Principaux

| Composant | Technologie |
|-----------|-------------|
| Hyperviseur | VMware Workstation Pro |
| Pare-feu (HA) | pfSense + CARP + pfsync |
| Cluster de stockage | Windows Server S2D (Storage Spaces Direct) |
| Contrôleurs de domaine | Active Directory (x2, redondants) |
| Authentification | RADIUS (NPS sur AD) |
| SIEM | Elastic Stack (Elasticsearch + Kibana + Beats) via Docker |
| Sauvegarde | UrBackup |
| Services Web | IIS sur Windows Server Core |

---

## 🔐 Sécurité

### Durcissement de l'Active Directory
- Désactivation de NTLM, Kerberos imposé
- GPOs restrictives déployées et testées
- Audit de sécurité réalisé avec **PingCastle**
- Modèle d'administration en niveaux (tiering)

### GPOs Déployées
- Politique de mots de passe renforcée
- Restriction des périphériques USB
- Verrouillage d'écran et expiration de session
- Politiques de restriction logicielle
- Journalisation d'audit (connexion, utilisation des privilèges, accès aux objets)

### Sécurité Réseau
- Segmentation réseau par VLANs
- Cluster pfSense avec IP Virtuelle (CARP)
- Authentification RADIUS intégrée à pfSense
- Supervision SIEM avec alertes en temps réel

---

## 🖧 Plan d'Adressage IP

### Site A (extrait)

| Machine | Rôle | Adresse IP |
|---------|------|------------|
| CORE01 | Nœud 1 du cluster S2D | Voir rapport complet |
| CORE02 | Nœud 2 du cluster S2D | Voir rapport complet |
| DC01 | Contrôleur de domaine principal | Voir rapport complet |
| DC02 | Contrôleur de domaine secondaire | Voir rapport complet |
| pfSense Master | Pare-feu principal | Voir rapport complet |
| pfSense Slave | Pare-feu secondaire | Voir rapport complet |

> Les tableaux d'adressage complets pour le Site A et le Site B sont disponibles dans le rapport.

---

## 📦 Services Déployés

### Cluster Failover S2D
- Deux nœuds Windows Server Core (CORE01 / CORE02)
- Storage Spaces Direct pour un stockage partagé tolérant aux pannes
- Rôle Serveur de fichiers configuré sur le cluster
- IIS déployé sur les deux nœuds (résolution du conflit de port documentée)

### pfSense HA avec CARP
- Configuration Maître/Esclave pfSense
- pfsync pour la synchronisation des états
- IP Virtuelle (VIP) pour un basculement transparent
- Client RADIUS configuré pour une authentification centralisée

### SIEM — Elastic Stack (Docker)
- Elasticsearch + Kibana + Beats déployés via Docker Compose
- Collecte centralisée des logs de toute l'infrastructure
- Tableaux de bord de sécurité et alertes configurés

### Sauvegarde — UrBackup
- Serveur de sauvegarde centralisé
- Planifications automatiques pour les systèmes critiques
- Procédures de restauration documentées

---

## 📈 Résultats Clés

| Indicateur | Résultat |
|------------|----------|
| Vitesse d'accès aux données | **3x plus rapide** grâce au Storage Spaces Direct |
| Points de défaillance uniques (SPOF) | **Éliminés** (pare-feu, stockage, AD) |
| Supervision de sécurité | **Centralisée** via Elastic SIEM |
| Capacité de reprise | **PRA/PCA documentés** avec RTO/RPO par service |
| Coût de la stack | **Zéro coût** (pfSense, ELK, UrBackup — 100% open-source) |

---

## 📁 Structure du Dépôt

```
📁 infrastructure-hyperconvergee/
├── README.md                  ← Ce fichier
├── docs/
│   └── rapport_complet.pdf    ← Rapport technique complet (100 pages)
├── configs/
│   ├── pfsense/               ← Règles pare-feu & config CARP
│   ├── active-directory/      ← Exports GPO & structure AD
│   └── reseau/                ← Plans IP & config VLAN
├── scripts/
│   └── powershell/            ← Scripts de déploiement et supervision
├── docker/
│   └── elastic-stack/         ← Docker Compose pour le SIEM
└── schemas/
    └── infrastructure.png     ← Schéma réseau / architecture
```

---

## 🛣️ Améliorations Futures

- **MFA** — Intégrer YubiKey ou Microsoft Authenticator
- **Orchestration** — Migration de VMware Workstation vers ESXi ou Proxmox
- **Cloud hybride** — Azure AD Hybrid Join
- **Zero Trust** — Modèle "Ne jamais faire confiance, toujours vérifier"
- **Détection avancée** — Intégration de Microsoft Sentinel pour une surveillance SOC

---

## 📚 Références & Normes

- CIS Benchmarks (Windows Server, Windows 10/11)
- NIST Cybersecurity Framework
- Référentiels ANSSI
- Conformité RGPD / ISO 27001

---

## 👤 Auteur

**Licence en Informatique & Cybersécurité**  
Projet : Refonte d'une Infrastructure Hyperconvergée Sécurisée  
Environnement client : Print&Rest (environnement de simulation professionnelle)

---

> *« Une infrastructure résiliente n'est pas un coût, c'est un levier de croissance. »*
