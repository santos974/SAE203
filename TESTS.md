# Stratégie de test

> Application de génération d'audits de sécurité informatique  
> Stack : HTML / CSS / JavaScript / PHP

---

## 1. Objectifs des tests

- Vérifier que l'application génère des audits corrects et complets
- S'assurer de l'absence de failles de sécurité (XSS, injections SQL, etc.)
- Valider l'interface utilisateur sur différents navigateurs et appareils
- Garantir la stabilité et la fiabilité avant chaque livraison

---

## 2. Types de tests

### 2.1 Tests unitaires
Tester chaque fonction PHP de façon isolée.

**Outils recommandés :** PHPUnit

**Ce qu'on teste :**
- Les fonctions de génération d'audit (résultat attendu vs résultat obtenu)
- Les fonctions de validation des entrées utilisateur
- Les fonctions de hachage et de gestion des sessions

**Exemple :**
```php
// Test PHPUnit — vérifier que la fonction génère bien un audit non vide
public function testGenerateAuditNotEmpty() {
    $audit = generateAudit(['target' => '192.168.1.1', 'type' => 'reseau']);
    $this->assertNotEmpty($audit);
}
```

---

### 2.2 Tests d'intégration
Tester les interactions entre les modules (PHP ↔ base de données, formulaires ↔ traitement).

**Ce qu'on teste :**
- La connexion et les requêtes à la base de données
- L'enregistrement et la récupération d'un audit complet
- Le flux complet : saisie du formulaire → traitement PHP → affichage du rapport

---

### 2.3 Tests fonctionnels / end-to-end
Simuler un utilisateur réel qui utilise l'application.

**Outils recommandés :** Cypress (JS) ou tests manuels structurés

**Scénarios à tester :**

| ID | Scénario | Résultat attendu |
|----|----------|-----------------|
| T01 | Connexion avec identifiants valides | Redirection vers le tableau de bord |
| T02 | Connexion avec identifiants invalides | Message d'erreur, pas d'accès |
| T03 | Génération d'un audit réseau | Rapport complet affiché et téléchargeable |
| T04 | Génération d'un audit avec champ vide | Message d'erreur de validation |
| T05 | Téléchargement du rapport PDF | Fichier PDF valide téléchargé |
| T06 | Déconnexion | Session détruite, redirection vers login |
| T07 | Accès à une page protégée sans connexion | Redirection vers login |

---

### 2.4 Tests de sécurité
Vérifier que l'application résiste aux attaques courantes.

**Ce qu'on teste :**

| Test | Méthode | Résultat attendu |
|------|---------|-----------------|
| Injection SQL | Saisir `' OR 1=1 --` dans un champ | Requête rejetée, pas d'accès aux données |
| XSS | Saisir `<script>alert('xss')</script>` | Script non exécuté, texte échappé |
| Accès direct aux fichiers PHP | Tenter d'accéder à `config.php` | Accès refusé (403) |
| Session hijacking | Copier un cookie de session | Session invalidée après déconnexion |
| Upload malveillant | Envoyer un fichier `.php` déguisé | Rejet du fichier |

**Outils recommandés :** OWASP ZAP, Burp Suite (version communautaire)

---

### 2.5 Tests de compatibilité
S'assurer que l'interface fonctionne sur différents environnements.

**Navigateurs à tester :**
- Chrome (dernière version)
- Firefox (dernière version)
- Edge (dernière version)
- Safari (si possible)

**Résolutions à tester :**
- Desktop : 1920×1080, 1366×768
- Tablette : 768×1024
- Mobile : 375×667

---

### 2.6 Tests de régression
Après chaque modification majeure, re-tester les fonctionnalités existantes pour s'assurer qu'elles n'ont pas été cassées.

---

## 3. Processus de test

```
Développement d'une feature
        ↓
Tests unitaires (développeur)
        ↓
Tests d'intégration (développeur)
        ↓
Pull Request + revue de code
        ↓
Tests fonctionnels (autre membre de l'équipe)
        ↓
Tests de sécurité (avant chaque release)
        ↓
Merge sur develop → tests de régression
        ↓
Merge sur main (release validée)
```

---

## 4. Rapport de test

Chaque test doit être documenté avec :

| Champ | Description |
|-------|-------------|
| ID | Identifiant unique du test |
| Date | Date d'exécution |
| Testeur | Membre de l'équipe |
| Environnement | Local / Staging / Production |
| Résultat | ✅ Succès / ❌ Échec |
| Bug lié | Numéro d'issue GitHub si échec |

---

## 5. Outils récapitulatifs

| Outil | Usage |
|-------|-------|
| PHPUnit | Tests unitaires PHP |
| Cypress | Tests end-to-end JS |
| OWASP ZAP | Tests de sécurité automatisés |
| GitHub Issues | Suivi des bugs trouvés |
| GitHub Actions | Automatisation des tests (CI/CD) |

---

*Document maintenu par l'équipe — à mettre à jour à chaque ajout de fonctionnalité.*
