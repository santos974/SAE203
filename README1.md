# AuditSec — Générateur d'audits de sécurité informatique

![PHP](https://img.shields.io/badge/PHP-8.0+-777BB4?logo=php&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?logo=javascript&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL-8.0+-4479A1?logo=mysql&logoColor=white)
![Licence](https://img.shields.io/badge/Licence-MIT-green)
![Statut](https://img.shields.io/badge/Statut-En%20développement-orange)

> Application web permettant aux experts en cybersécurité de générer, gérer et exporter des rapports d'audit de sécurité informatique de façon rapide et structurée.

---

## Sommaire

- [Présentation](#présentation)
- [Fonctionnalités](#fonctionnalités)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Tests](#tests)
- [Gestion des risques](#gestion-des-risques)
- [Équipe](#équipe)
- [Licence](#licence)

---

## Présentation

AuditSec est une application web développée dans le cadre d'un projet de groupe. Elle permet à des experts en sécurité informatique de :

- Configurer et lancer des audits de sécurité (réseau, web, infrastructure)
- Générer automatiquement des rapports structurés
- Consulter l'historique des audits réalisés
- Exporter les rapports en PDF

---

## Fonctionnalités

- Authentification sécurisée avec gestion des rôles (expert / admin)
- Formulaire de configuration d'audit paramétrable
- Génération automatique de rapports d'audit
- Export PDF des rapports
- Historique et recherche des audits
- Interface responsive (desktop & mobile)
- Gestion des utilisateurs (admin)

---

## Installation

Consultez la documentation complète d'installation :

📄 **[INSTALLATION.md](./INSTALLATION.md)**

En résumé :

```bash
git clone https://github.com/VOTRE_USERNAME/auditsec.git
cd auditsec
cp config/config.example.php config/config.php
# Remplir config.php avec vos paramètres
composer install
mysql -u user -p audit_db < database/schema.sql
php -S localhost:8000
```

---

## Utilisation

Consultez la documentation d'utilisation dans :

📄 **[INSTALLATION.md — Section Utilisation](./INSTALLATION.md#3-utilisation)**

---

## Tests

La stratégie de test complète est disponible ici :

📄 **[TESTS.md](./TESTS.md)**

Elle couvre : tests unitaires (PHPUnit), tests fonctionnels, tests de sécurité (OWASP ZAP), et tests de compatibilité navigateur.

---

## Gestion des risques

Les risques identifiés et les solutions mises en place sont documentés ici :

📄 **[RISQUES.md](./RISQUES.md)**

Les principaux risques couverts : injections SQL, XSS, gestion des sessions, authentification, et risques organisationnels.

---

## Équipe

| Nom | Rôle | GitHub |
|-----|------|--------|
| — | Chef de projet | [@username]() |
| — | Développeur back-end (PHP) | [@username]() |
| — | Développeur front-end (JS/CSS) | [@username]() |
| — | Testeur / QA | [@username]() |
| — | DevOps / Documentation | [@username]() |

> Remplacez les `—` et `@username` par les vraies informations de l'équipe.

---

## Structure du projet

```
/
├── assets/           → CSS, JS, images
├── config/           → Configuration (non versionné)
├── database/         → Schéma SQL
├── includes/         → Fonctions PHP partagées
├── modules/          → Modules fonctionnels
├── tests/            → Tests PHPUnit
├── INSTALLATION.md   → Doc d'installation et d'utilisation
├── TESTS.md          → Stratégie de test
├── RISQUES.md        → Gestion des risques
└── README.md         → Ce fichier
```

---

## Licence

Ce projet est sous licence MIT. Voir le fichier [LICENSE](./LICENSE).
