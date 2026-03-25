# Projet : API de Monitoring Serveur (HealthCheck API)

**Cours :** Introduction à l'API REST
**Formateur :** Franck DJACOTO
**Technologies :** Libre choix (Python, Go, PHP, Node.js, Rust, Java, etc.)
**Note :** _____ / 20

---

# PARTIE 1 — Sonde de monitoring (obligatoire — 15 points)

---

## 1. Contexte et objectif

Vous êtes administrateur système dans une entreprise qui gère plusieurs serveurs. Votre responsable vous demande de développer une **API REST** qui permettra de surveiller l'état de santé des serveurs en temps réel.

Cette API sera utilisée par un tableau de bord (dashboard) pour afficher les métriques de tous les serveurs de l'entreprise.

L'objectif est de créer une **sonde de monitoring** qui expose les informations système (CPU, RAM, disque, etc.) via des endpoints REST accessibles en HTTP.

---

## 2. Endpoints à implémenter

Tous les endpoints doivent utiliser le préfixe `/api/v1/` pour respecter les bonnes pratiques de versioning d'API.

| Endpoint           | Méthode | Description                                    |
| ------------------ | ------- | ---------------------------------------------- |
| `/api/v1/health`   | GET     | État général du serveur (UP/DOWN)              |
| `/api/v1/cpu`      | GET     | Utilisation du CPU (%)                         |
| `/api/v1/memory`   | GET     | Utilisation de la RAM (%)                      |
| `/api/v1/disk`     | GET     | Utilisation du disque (%)                      |
| `/api/v1/all`      | GET     | Toutes les métriques en une seule requête      |

> **💡 Pourquoi le préfixe `/api/v1/` ?**
> Le versioning permet de faire évoluer votre API sans casser les clients existants. Si demain vous changez le format des réponses, vous créerez un `/api/v2/` tout en gardant le v1 fonctionnel.

---

## 3. Exemples de réponses JSON

Chaque endpoint doit renvoyer une réponse JSON structurée. Voici les formats attendus :

### 3.1 GET /api/v1/health (1,5 points)

```json
{
  "status": "UP",
  "hostname": "server-prod-01.local",
  "os": "linux",
  "platform": "ubuntu",
  "checked_at": "Sun, 22 Jan 2026 22:29:16 CET"
}
```

### 3.2 GET /api/v1/cpu (1,5 points)

```json
{
  "total_usage_percent": 21.56,
  "logical_cores": 8,
  "physical_cores": 8,
  "checked_at": "Sun, 22 Jan 2026 22:30:01 CET"
}
```

### 3.3 GET /api/v1/memory (1,5 points)

```json
{
  "total_gb": 32,
  "used_gb": 22,
  "free_gb": 10,
  "used_percent": 70.94,
  "checked_at": "Sun, 22 Jan 2026 22:32:35 CET"
}
```

### 3.4 GET /api/v1/disk (1,5 points)

```json
{
  "total_gb": 460,
  "used_gb": 443,
  "free_gb": 17,
  "used_percent": 96.23,
  "checked_at": "Sun, 22 Jan 2026 22:33:08 CET"
}
```

### 3.5 GET /api/v1/all (2 points)

```json
{
  "host_info": {
    "status": "UP",
    "hostname": "server-prod-01.local",
    "os": "linux",
    "platform": "ubuntu",
    "checked_at": "Sun, 22 Jan 2026 22:33:53 CET"
  },
  "cpu_info": {
    "total_usage_percent": 31.13,
    "logical_cores": 8,
    "physical_cores": 8,
    "checked_at": "Sun, 22 Jan 2026 22:33:54 CET"
  },
  "memory_info": {
    "total_gb": 32,
    "used_gb": 22,
    "available_gb": 10,
    "used_percent": 70.93,
    "checked_at": "Sun, 22 Jan 2026 22:33:54 CET"
  },
  "disk_info": {
    "total_gb": 460,
    "used_gb": 443,
    "free_gb": 17,
    "used_percent": 96.23,
    "checked_at": "Sun, 22 Jan 2026 22:33:54 CET"
  }
}
```

---

## 4. Gestion des erreurs

Votre API doit gérer les erreurs proprement et renvoyer des réponses JSON structurées avec les codes HTTP appropriés.

### 4.1 Codes HTTP attendus

| Code  | Statut                 | Quand ?                                              |
| ----- | ---------------------- | ---------------------------------------------------- |
| `200` | OK                     | La requête a réussi                                  |
| `404` | Not Found              | Endpoint inexistant                                  |
| `500` | Internal Server Error  | Erreur lors de la lecture des métriques système s'il y a une erreur       |

### 4.2 Format d'erreur attendu

En cas d'erreur, l'API doit renvoyer une réponse JSON avec le format suivant :

```json
{
  "error": "Unable to read CPU metrics",
  "status": 500,
  "checked_at": "Sun, 22 Jan 2026 22:40:00 CET"
}
```

---

## 5. Contraintes techniques

- Toutes les réponses doivent être au format JSON avec le header `Content-Type: application/json`
- Le champ `checked_at` doit refléter l'heure exacte de la requête (pas une valeur en cache)
- Le code doit être versionné avec Git

---

## 6. Livrables — Partie 1

- [ ] Code source de l'API (dépôt Git ou archive ZIP)
- [ ] Collection Postman avec tous les endpoints testés et les réponses sauvegardées

---

## 7. Barème — Partie 1

| Critère                                              | Points |
| ---------------------------------------------------- | :----: |
| API fonctionnelle avec tous les 5 endpoints          |   8    |
| Gestion des erreurs (réponses structurées avec codes HTTP) |   2    |
| Collection Postman complète            |   5    |
| **Sous-total Partie 1**                              | **15** |

---
---

# PARTIE 2 — Système d'alertes automatiques (5 points)

---

## 8. Contexte

Votre sonde de monitoring fonctionne, mais elle se contente d'afficher des métriques. En production, un administrateur système ne peut pas surveiller un dashboard 24h/24. Il faut que le système **réagisse automatiquement** quand quelque chose ne va pas.

Votre responsable vous demande maintenant d'ajouter un **système d'alertes** : lorsqu'une métrique dépasse un seuil critique, votre API doit automatiquement créer un **incident** sur la plateforme de monitoring centralisée.

> **Objectif pédagogique :** Dans la Partie 1, vous avez développé une API (vous êtes le serveur). Dans la Partie 2, vous allez **consommer** une API externe (vous êtes le client). C'est la compétence inverse, et les deux sont essentielles.

---

## 9. API de monitoring centralisée

La plateforme de monitoring centralisée est accessible à l'adresse suivante :

- **Documentation Swagger :** [https://monitoring-app.on-forge.com/swagger](https://monitoring-app.on-forge.com/swagger)
- **Documentation alternative :** [https://monitoring-app.on-forge.com/docs/](https://monitoring-app.on-forge.com/docs/)

### 9.1 Authentification

L'API de monitoring utilise un **Bearer Token** pour l'authentification. Voici les étapes :

**Étape 1 — Créer un compte [si ce n'est pas fait]**

```
POST https://monitoring-app.on-forge.com/api/v1/auth/register
Content-Type: application/json

{
  "name": "Votre Nom",
  "email": "votre.email@example.com",
  "password": "votre_mot_de_passe",
  "password_confirmation": "votre_mot_de_passe"
}
```

**Étape 2 — Se connecter et récupérer le token**

```
POST https://monitoring-app.on-forge.com/api/auth/login
Content-Type: application/json

{
  "email": "votre.email@example.com",
  "password": "votre_mot_de_passe"
}
```

Réponse attendue :

```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "user": {
      "id": 1,
      "name": "Votre Nom",
      "email": "votre.email@example.com"
    },
    "token": "1|abc123..."
  }
}
```

**Étape 3 — Utiliser le token dans vos requêtes**

Toutes les requêtes authentifiées doivent inclure le header :

```
Authorization: Bearer 1|abc123...
```

### 9.2 Enregistrer votre application

Avant de pouvoir créer des incidents, vous devez enregistrer votre sonde sur la plateforme de monitoring :

```
POST https://monitoring-app.on-forge.com/api/applications
Authorization: Bearer {votre_token}
Content-Type: application/json

{
  "name": "Sonde Monitoring - {Votre Nom}",
  "url": "http://localhost:8000"
}
```

Notez l'`id` retourné dans la réponse — c'est votre `application_id` pour la création d'incidents.

---

## 10. Fonctionnalité à implémenter

### 10.1 Règle d'alerte

À chaque appel sur `/api/v1/cpu`, `/api/v1/memory`, `/api/v1/disk` ou `/api/v1/all`, votre API doit vérifier si une métrique dépasse le **seuil de 30%** d'utilisation.

| Métrique         | Champ vérifié         | Seuil  |
| ---------------- | --------------------- | ------ |
| CPU              | `total_usage_percent` | > 30%  |
| RAM              | `used_percent`        | > 30%  |
| Disque           | `used_percent`        | > 30%  |

> **⚠️ Note :** Le seuil de 30% est volontairement bas pour que vous puissiez facilement déclencher des incidents pendant vos tests. En production, les seuils typiques sont de 80-90%.

### 10.2 Création automatique d'un incident

Si une métrique dépasse le seuil, votre API doit envoyer une requête `POST` vers l'API de monitoring centralisée :

```
POST https://monitoring-app.on-forge.com/api/incidents
Authorization: Bearer {votre_token}
Content-Type: application/json

{
  "title": "ALERTE CPU — Utilisation à 85.42%",
  "description": "Le serveur server-prod-01.local a détecté une utilisation CPU anormale de 85.42% à Sun, 22 Jan 2026 14:30:00 CET.",
  "application_id": "{id_de_votre_application}",
  "status": "OPEN",
  "severity": "HIGH",
  "start_date": "2026-03-22T14:30:00"
}
```

### 10.3 Champs de l'incident

| Champ            | Type     | Description                                                             |
| ---------------- | -------- | ----------------------------------------------------------------------- |
| `title`          | string   | Titre descriptif incluant la métrique et la valeur (max 255 caractères) |
| `description`    | string   | Description détaillée avec hostname, valeur et horodatage               |
| `application_id` | string   | L'ID de votre application sur la plateforme (voir section 9.2)          |
| `status`         | enum     | `OPEN` (toujours OPEN à la création)                                    |
| `severity`       | enum     | Déterminée automatiquement selon le pourcentage (voir 10.4)             |
| `start_date`     | datetime | Date et heure de détection au format ISO 8601                           |

### 10.4 Détermination de la sévérité

Votre API doit automatiquement déterminer la sévérité de l'incident en fonction du pourcentage :

| Pourcentage       | Sévérité   |
| ----------------- | ---------- |
| 30% — 60%         | `LOW`      |
| 60% — 90%         | `HIGH`     |
| > 90%             | `CRITICAL` |

### 10.5 Comportement attendu

Voici le scénario complet lorsqu'un utilisateur appelle `GET /api/v1/cpu` :

1. Votre API lit les métriques CPU du serveur
2. Votre API vérifie si `total_usage_percent` > 30%
3. **Si oui** → votre API envoie un `POST` vers `https://monitoring-app.on-forge.com/api/v1/incidents` pour créer un incident, puis renvoie la réponse JSON au client avec `alert_triggered: true` et les détails de l'incident
4. **Si non** → votre API renvoie la réponse JSON normale avec `alert_triggered: false`

Le même comportement s'applique à `/api/v1/memory`, `/api/v1/disk`, et `/api/v1/all` (qui vérifie les 3 métriques et peut créer plusieurs incidents en une seule requête).

### 10.6 Exemples de réponses enrichies

**Quand une alerte est déclenchée (CPU > 30%) :**

```json
{
  "total_usage_percent": 85.42,
  "logical_cores": 8,
  "physical_cores": 8,
  "checked_at": "Sun, 22 Jan 2026 14:30:00 CET",
  "alert_triggered": true,
  "incident": {
    "id": 42,
    "severity": "HIGH",
    "message": "Incident created on monitoring platform"
  }
}
```

**Quand tout est normal (CPU ≤ 30%) :**

```json
{
  "total_usage_percent": 12.30,
  "logical_cores": 8,
  "physical_cores": 8,
  "checked_at": "Sun, 22 Jan 2026 14:30:00 CET",
  "alert_triggered": false
}
```

**GET /api/v1/all — avec alertes mixtes :**

```json
{
  "host_info": {
    "status": "UP",
    "hostname": "server-prod-01.local",
    "os": "linux",
    "platform": "ubuntu",
    "checked_at": "Sun, 22 Jan 2026 22:33:53 CET"
  },
  "cpu_info": {
    "total_usage_percent": 31.13,
    "logical_cores": 8,
    "physical_cores": 8,
    "checked_at": "Sun, 22 Jan 2026 22:33:54 CET",
    "alert_triggered": true,
    "incident": {
      "id": 45,
      "severity": "LOW",
      "message": "Incident created on monitoring platform"
    }
  },
  "memory_info": {
    "total_gb": 32,
    "used_gb": 22,
    "available_gb": 10,
    "used_percent": 70.93,
    "checked_at": "Sun, 22 Jan 2026 22:33:54 CET",
    "alert_triggered": true,
    "incident": {
      "id": 46,
      "severity": "HIGH",
      "message": "Incident created on monitoring platform"
    }
  },
  "disk_info": {
    "total_gb": 460,
    "used_gb": 100,
    "free_gb": 360,
    "used_percent": 21.74,
    "checked_at": "Sun, 22 Jan 2026 22:33:54 CET",
    "alert_triggered": false
  }
}
```

---

## 11. Endpoint supplémentaire

En plus des 5 endpoints de la Partie 1, ajoutez :

| Endpoint              | Méthode | Description                                                    |
| --------------------- | ------- | -------------------------------------------------------------- |
| `/api/v1/incidents`   | GET     | Liste des incidents créés par votre sonde (via l'API distante) |

Cet endpoint doit interroger `GET https://monitoring-app.on-forge.com/api/incidents` et renvoyer la liste des incidents liés à votre application.

```json
{
  "incidents": [
    {
      "id": 42,
      "title": "ALERTE CPU — Utilisation à 85.42%",
      "severity": "HIGH",
      "status": "OPEN",
      "start_date": "2026-03-22T14:30:00"
    },
    {
      "id": 43,
      "title": "ALERTE RAM — Utilisation à 70.94%",
      "severity": "HIGH",
      "status": "OPEN",
      "start_date": "2026-03-22T14:32:35"
    }
  ],
  "total": 2,
  "checked_at": "Sun, 22 Jan 2026 22:40:00 CET"
}
```

---

## 12. Livrables — Partie 2 (en complément de la Partie 1)

- [ ] Système d'alertes fonctionnel qui crée des incidents automatiquement
- [ ] Endpoint `/api/v1/incidents` fonctionnel
- [ ] Collection Postman mise à jour avec les nouveaux endpoints et des exemples de réponses avec/sans alerte

---

## 13. Barème — Partie 2

| Critère                                                             | Points |
| ------------------------------------------------------------------- | :----: |
| Authentification fonctionnelle (login + token) et création automatique d'incidents |   4   |
| Endpoint `/api/v1/incidents` fonctionnel                            |   1    |
| **Sous-total Partie 2**                                            |  **5** |

---
---

# Récapitulatif

| Partie                                    | Points   |
| ----------------------------------------- | :------: |
| **Partie 1** — Sonde de monitoring        | **15**   |
| **Partie 2** — Système d'alertes          |  **5**   |
| **TOTAL**                                 | **20**   |

---

## Ressources utiles

### Python

- **Flask :** [https://flask.palletsprojects.com](https://flask.palletsprojects.com)
- **psutil (métriques système) :** [https://psutil.readthedocs.io](https://psutil.readthedocs.io)
- **requests (appels HTTP) :** [https://docs.python-requests.org](https://docs.python-requests.org)

### Go

- **net/http (standard library) :** [https://pkg.go.dev/net/http](https://pkg.go.dev/net/http)
- **gopsutil :** [https://github.com/shirou/gopsutil](https://github.com/shirou/gopsutil)

### PHP

- **Laravel :** [https://laravel.com/docs](https://laravel.com/docs)
- **Guzzle (appels HTTP) :** [https://docs.guzzlephp.org](https://docs.guzzlephp.org)

### Node.js

- **Express :** [https://expressjs.com](https://expressjs.com)
- **systeminformation :** [https://www.npmjs.com/package/systeminformation](https://www.npmjs.com/package/systeminformation)
- **axios (appels HTTP) :** [https://axios-http.com](https://axios-http.com)

### Rust

- **Actix Web :** [https://actix.rs](https://actix.rs)
- **sysinfo :** [https://docs.rs/sysinfo](https://docs.rs/sysinfo)
- **reqwest (appels HTTP) :** [https://docs.rs/reqwest](https://docs.rs/reqwest)

---

> « On ne sait jamais ce dont on est capable tant qu’on n’a pas essayé. »
> — William COBBETT
>
> « Cela semble toujours impossible,  jusqu'à ce qu'on le fasse. »
>— Nelson MANDELA
>
>
>
> Bon courage à tous  ^ ^
