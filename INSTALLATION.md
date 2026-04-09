# Documentation d'installation et d'utilisation

> Application de génération d'audits de sécurité informatique  
> Stack : HTML / CSS / JavaScript / PHP

---

## Prérequis

Avant de commencer, assurez-vous d'avoir installé :

- **PHP** ≥ 8.0
- **MySQL** ≥ 8.0 (ou MariaDB ≥ 10.5)
- **Apache** ou **Nginx** (ou utiliser XAMPP / WAMP / MAMP)
- **Composer** (gestionnaire de dépendances PHP)
- **Git**
- Un navigateur moderne (Chrome, Firefox, Edge)

---

## 1. Installation

### Étape 1 — Cloner le dépôt

```bash
git clone https://github.com/VOTRE_USERNAME/NOM_DU_DEPOT.git
cd NOM_DU_DEPOT
```

---

### Étape 2 — Configurer l'environnement

Copiez le fichier de configuration d'exemple et renseignez vos paramètres :

```bash
cp config/config.example.php config/config.php
```

Ouvrez `config/config.php` et remplissez les valeurs :

```php
<?php
define('DB_HOST', 'localhost');
define('DB_NAME', 'audit_db');
define('DB_USER', 'votre_utilisateur');
define('DB_PASS', 'votre_mot_de_passe');
define('APP_URL', 'http://localhost/NOM_DU_DEPOT');
define('APP_ENV', 'development'); // 'production' en ligne
?>
```

> ⚠️ Ne jamais committer `config.php` — il est déjà dans `.gitignore`.

---

### Étape 3 — Créer la base de données

Connectez-vous à MySQL et créez la base :

```sql
CREATE DATABASE audit_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

Importez le schéma initial :

```bash
mysql -u votre_utilisateur -p audit_db < database/schema.sql
```

---

### Étape 4 — Installer les dépendances PHP

```bash
composer install
```

---

### Étape 5 — Configurer le serveur web

**Avec XAMPP / WAMP :**
- Placez le projet dans le dossier `htdocs/` (XAMPP) ou `www/` (WAMP)
- Démarrez Apache et MySQL depuis le panneau de contrôle
- Accédez à `http://localhost/NOM_DU_DEPOT`

**Avec le serveur PHP intégré (développement uniquement) :**

```bash
php -S localhost:8000
```

Puis ouvrez `http://localhost:8000`

---

### Étape 6 — Créer le premier compte administrateur

```bash
php scripts/create_admin.php
```

Suivez les instructions pour définir l'email et le mot de passe.

---

## 2. Structure du projet

```
/
├── assets/
│   ├── css/          → Feuilles de style
│   ├── js/           → Scripts JavaScript
│   └── img/          → Images et icônes
├── config/
│   ├── config.php        → Configuration locale (non versionné)
│   └── config.example.php → Modèle de configuration
├── database/
│   └── schema.sql        → Structure de la base de données
├── includes/
│   ├── auth.php          → Gestion de l'authentification
│   ├── db.php            → Connexion PDO à la base de données
│   └── functions.php     → Fonctions utilitaires
├── modules/
│   └── audit/            → Module de génération d'audits
├── scripts/
│   └── create_admin.php  → Script de création d'admin
├── tests/                → Tests PHPUnit
├── index.php             → Point d'entrée
└── README.md
```

---

## 3. Utilisation

### 3.1 Connexion

1. Accédez à l'URL de l'application
2. Entrez votre **email** et **mot de passe**
3. Cliquez sur **Se connecter**

> Les comptes sont gérés par l'administrateur. Contactez votre chef de projet pour obtenir un accès.

---

### 3.2 Générer un audit

1. Depuis le tableau de bord, cliquez sur **Nouvel audit**
2. Remplissez le formulaire :
   - **Nom du client / système cible**
   - **Type d'audit** (réseau, application web, infrastructure...)
   - **Périmètre** (adresses IP, URLs, plage de ports)
   - **Niveau de détail** (basique / standard / avancé)
3. Cliquez sur **Lancer l'audit**
4. Patientez pendant la génération du rapport
5. Téléchargez le rapport en PDF ou consultez-le en ligne

---

### 3.3 Gérer les audits

- **Voir tous les audits** → Menu **Historique**
- **Rechercher un audit** → Barre de recherche par nom ou date
- **Supprimer un audit** → Icône corbeille (admin uniquement)
- **Exporter un rapport** → Bouton **Télécharger** sur la page du rapport

---

### 3.4 Gestion des utilisateurs (admin)

1. Accédez à **Paramètres → Utilisateurs**
2. Cliquez sur **Ajouter un utilisateur**
3. Remplissez : nom, email, rôle (expert / admin)
4. L'utilisateur reçoit un email d'invitation avec un lien pour définir son mot de passe

---

## 4. Mise en production

```bash
# Passer en mode production dans config.php
define('APP_ENV', 'production');

# Désactiver l'affichage des erreurs PHP
# Dans php.ini ou .htaccess :
# display_errors = Off
# log_errors = On

# Activer HTTPS (certificat TLS requis)
# Configurer les redirections HTTP → HTTPS dans .htaccess
```

> ⚠️ En production, ne jamais laisser les fichiers de config, logs ou scripts d'installation accessibles publiquement.

---

## 5. Problèmes fréquents

| Problème | Cause probable | Solution |
|----------|---------------|----------|
| Page blanche | Erreur PHP cachée | Vérifier les logs Apache / PHP |
| Erreur de connexion BDD | Mauvais paramètres dans `config.php` | Vérifier host, user, password, dbname |
| Accès refusé (403) | Permissions fichiers incorrectes | `chmod -R 755 .` puis `chmod -R 644 *.php` |
| Session expirée en boucle | Cookie mal configuré | Vérifier `session_set_cookie_params()` |
| Composer introuvable | Composer non installé | [getcomposer.org](https://getcomposer.org) |

---

## 6. Contribuer au projet

1. Créez une branche depuis `develop` : `git checkout -b feature/ma-fonctionnalite`
2. Faites vos modifications et committez : `git commit -m "Ajout de la fonctionnalité X"`
3. Poussez : `git push origin feature/ma-fonctionnalite`
4. Ouvrez une **Pull Request** vers `develop` sur GitHub
5. Un autre membre de l'équipe effectue la revue avant le merge

---

*Document maintenu par l'équipe — à mettre à jour à chaque changement d'installation ou de fonctionnalité.*
