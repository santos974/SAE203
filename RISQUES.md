# Gestion des risques et solutions envisagées

> Application de génération d'audits de sécurité informatique  
> Stack : HTML / CSS / JavaScript / PHP  
> Équipe : 4-5 personnes

---

## 1. Identification des risques

### 1.1 Risques techniques

| ID | Risque | Probabilité | Impact | Criticité |
|----|--------|-------------|--------|-----------|
| R01 | Injection SQL dans les formulaires PHP | Élevée | Critique | 🔴 Critique |
| R02 | Failles XSS (Cross-Site Scripting) côté JS | Élevée | Élevé | 🔴 Critique |
| R03 | Exposition de données sensibles dans les audits générés | Moyenne | Critique | 🔴 Critique |
| R04 | Authentification insuffisante (accès non autorisé) | Moyenne | Élevé | 🟠 Élevé |
| R05 | Mauvaise gestion des sessions PHP | Moyenne | Élevé | 🟠 Élevé |
| R06 | Téléversement de fichiers malveillants | Faible | Critique | 🟠 Élevé |
| R07 | Perte de données (base de données non sauvegardée) | Faible | Élevé | 🟡 Moyen |
| R08 | Dépendances JavaScript obsolètes ou vulnérables | Moyenne | Moyen | 🟡 Moyen |

### 1.2 Risques organisationnels

| ID | Risque | Probabilité | Impact | Criticité |
|----|--------|-------------|--------|-----------|
| R09 | Retard dans les livraisons (surcharge d'un membre) | Moyenne | Moyen | 🟡 Moyen |
| R10 | Conflits Git (mauvaise gestion des branches) | Élevée | Faible | 🟡 Moyen |
| R11 | Documentation incomplète ou manquante | Moyenne | Moyen | 🟡 Moyen |
| R12 | Mauvaise répartition des tâches | Faible | Moyen | 🟢 Faible |

---

## 2. Solutions et mesures de mitigation

### R01 — Injection SQL
**Solution :**
- Utiliser des **requêtes préparées PDO** avec des paramètres liés (jamais de concaténation directe)
- Valider et filtrer toutes les entrées côté serveur
- Limiter les droits de l'utilisateur SQL (lecture seule si possible)

```php
// ✅ Correct
$stmt = $pdo->prepare("SELECT * FROM audits WHERE id = :id");
$stmt->execute([':id' => $id]);

// ❌ À éviter
$query = "SELECT * FROM audits WHERE id = " . $_GET['id'];
```

---

### R02 — Cross-Site Scripting (XSS)
**Solution :**
- Échapper toutes les sorties HTML avec `htmlspecialchars()`
- Mettre en place une **Content Security Policy (CSP)** dans les en-têtes HTTP
- Valider les données côté client ET côté serveur

```php
// ✅ Correct
echo htmlspecialchars($userInput, ENT_QUOTES, 'UTF-8');
```

---

### R03 — Exposition de données sensibles
**Solution :**
- Ne jamais stocker de mots de passe ou clés API en clair
- Chiffrer les rapports d'audit contenant des informations critiques
- Utiliser HTTPS (certificat TLS obligatoire en production)
- Masquer les informations sensibles dans les logs

---

### R04 — Authentification insuffisante
**Solution :**
- Implémenter un système de login avec **hachage bcrypt** pour les mots de passe
- Mettre en place une politique de mot de passe fort
- Ajouter une authentification à deux facteurs (2FA) si possible

```php
// ✅ Hachage du mot de passe
$hash = password_hash($password, PASSWORD_BCRYPT);

// ✅ Vérification
if (password_verify($inputPassword, $hash)) { /* OK */ }
```

---

### R05 — Gestion des sessions PHP
**Solution :**
- Régénérer l'ID de session après chaque connexion (`session_regenerate_id(true)`)
- Définir une durée de vie de session limitée
- Configurer les cookies de session avec les flags `HttpOnly` et `Secure`

```php
session_set_cookie_params(['secure' => true, 'httponly' => true, 'samesite' => 'Strict']);
session_start();
session_regenerate_id(true);
```

---

### R06 — Téléversement de fichiers malveillants
**Solution :**
- Vérifier le type MIME côté serveur (pas seulement l'extension)
- Limiter la taille maximale des fichiers
- Stocker les fichiers en dehors de la racine web
- Scanner les fichiers avec un antivirus si possible

---

### R07 — Perte de données
**Solution :**
- Mettre en place des **sauvegardes automatiques** de la base de données (CRON)
- Versionner les sauvegardes et les stocker hors site
- Tester régulièrement la restauration

---

### R08 — Dépendances obsolètes
**Solution :**
- Utiliser `npm audit` régulièrement pour les dépendances JS
- Maintenir un fichier `package.json` à jour
- Ne pas inclure de bibliothèques inutiles

---

### R09 — Retards de livraison
**Solution :**
- Découper les tâches en sprints de 1-2 semaines (méthode agile)
- Utiliser un tableau Kanban (GitHub Projects ou Trello)
- Faire des points d'avancement hebdomadaires

---

### R10 — Conflits Git
**Solution :**
- Adopter une stratégie de branches claire : `main`, `develop`, `feature/xxx`
- Faire des Pull Requests avec revue de code avant merge
- Ne jamais pousser directement sur `main`

---

## 3. Tableau de suivi des risques

| ID | Risque | Responsable | Statut | Date de révision |
|----|--------|-------------|--------|-----------------|
| R01 | Injection SQL | À assigner | 🔲 En attente | — |
| R02 | XSS | À assigner | 🔲 En attente | — |
| R03 | Données sensibles | À assigner | 🔲 En attente | — |
| R04 | Authentification | À assigner | 🔲 En attente | — |
| R05 | Sessions PHP | À assigner | 🔲 En attente | — |
| R06 | Upload fichiers | À assigner | 🔲 En attente | — |
| R07 | Sauvegarde BDD | À assigner | 🔲 En attente | — |
| R08 | Dépendances JS | À assigner | 🔲 En attente | — |
| R09 | Retards | Chef de projet | 🔲 En attente | — |
| R10 | Conflits Git | Toute l'équipe | 🔲 En attente | — |

---

*Document maintenu par l'équipe — à mettre à jour à chaque sprint.*
