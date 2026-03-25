# Projet : Client CLI pour MonitoringApp

**Cours :** Introduction à l'API REST
**Formateur :** Franck DJACOTO
**Langage :** Bash ou PowerShell (au choix)
**Travail :** En groupe de 3 ou 4
**Note :** _____ / 20

---

## 1. Contexte et objectif

Vous êtes administrateur système et vous utilisez la plateforme **MonitoringApp** pour surveiller vos serveurs. Actuellement, vous devez passer par l'interface web ou Postman pour gérer vos applications et incidents.

Votre responsable vous demande de créer un **outil en ligne de commande (CLI)** qui permettra de gérer la plateforme MonitoringApp directement depuis le terminal, sans quitter votre console.

L'objectif est de développer un **script interactif** en Bash ou PowerShell qui communique avec l'API REST de MonitoringApp pour effectuer des opérations CRUD sur les groupes, les applications et les incidents.

> **📌 Prérequis :** Avant de commencer, vous devez avoir créé un compte sur la plateforme MonitoringApp via l'interface web ou Postman.
>
> - **Interface web:** [https://frontend-monitoring-app.on-forge.com/fr/register](https://frontend-monitoring-app.on-forge.com/fr/register)
> - **Documentation api :** [https://monitoring-app.on-forge.com/docs](https://monitoring-app.on-forge.com/docs)
> - **Documentation API version Swagger :** [https://monitoring-app.on-forge.com/swagger](https://monitoring-app.on-forge.com/swagger)

---

## 2. Fonctionnement attendu

Le script doit être **interactif**. Il doit permettre aux utilisateurs de saisir les informations nécessaires pour chaque opération (ex : nom d'une application, ID d'un incident, etc.) et afficher les résultats de manière lisible.

### 2.1 Configuration du token

Le token est récupéré via la route de login (endpoint 1) et stocké dans une variable `$TOKEN` pour être utilisé dans les requêtes suivantes.

## 3. Endpoints à implémenter

### 3.1 Authentification

| # | Action | Méthode | URL | Saisie interactive |
|---|--------|---------|-----|--------------------|
| 1 | Se connecter | POST | `/api/v1/auth/login` |  email + password |

Le login doit récupérer le token retourné et le stocker dans la variable `$TOKEN` pour le reste de la session.

**Payload :**

```json
{
  "email": "saisie utilisateur",
  "password": "saisie utilisateur"
}
```

---

### 3.2 Groupes

Vous ne pouvez supprimer, modifier  un groupe que vous avez créé.

| # | Action | Méthode | URL | Saisie interactive |
|---|--------|---------|-----|--------------------|
| 2 | Lister les groupes | GET | `/api/v1/application-groups` | Aucune |
| 3 | Voir un groupe | GET | `/api/v1/application-groups/{id}` |  ID du groupe |
| 4 | Créer un groupe | POST | `/api/v1/application-groups` |  name + description |
| 5 | Modifier un groupe | PUT | `/api/v1/application-groups/{id}` |  ID + name + description |
| 6 | Supprimer un groupe | DELETE | `/api/v1/application-groups/{id}` |  ID + confirmation |

**Payload création / modification :**

```json
{
  "name": "saisie utilisateur",
  "description": "saisie utilisateur"
}
```

---

### 3.3 Applications

Vous ne pouvez supprimer, modifier  une application que vous avez créée.

| # | Action | Méthode | URL | Saisie interactive |
|---|--------|---------|-----|--------------------|
| 7 | Lister les applications | GET | `/api/v1/applications` | Aucune |
| 8 | Voir une application | GET | `/api/v1/applications/{id}` |  ID |
| 9 | Créer une application | POST | `/api/v1/applications` |  name + url + description |
| 10 | Modifier une application | PUT | `/api/v1/applications/{id}` |  ID + name + url + description |
| 11 | Supprimer une application | DELETE | `/api/v1/applications/{id}` |  ID + confirmation |

**Payload création / modification :**

```json
{
  "name": "saisie utilisateur",
  "url": "saisie utilisateur",
  "description": "saisie utilisateur",
  "monitoring_enabled": true
}
```

---

### 3.4 Incidents (7 endpoints)

Vous ne pouvez supprimer, modifier  un incident que vous avez créé.

| # | Action | Méthode | URL | Saisie interactive |
|---|--------|---------|-----|--------------------|
| 12 | Lister les incidents | GET | `/api/v1/incidents` | Aucune |
| 13 | Voir un incident | GET | `/api/v1/incidents/{id}` |  ID |
| 14 | Créer un incident | POST | `/api/v1/incidents` |  title + description + application_id + severity |
| 15 | Modifier un incident | PUT | `/api/v1/incidents/{id}` |  ID + title + description + status + severity |
| 16 | Supprimer un incident | DELETE | `/api/v1/incidents/{id}` |  ID + confirmation |
| 17 | Résoudre un incident | PUT | `/api/v1/incidents/{id}/resolve` |  ID |
| 18 | Rouvrir un incident | PUT | `/api/v1/incidents/{id}/reopen` |  ID |

**Payload création :**

```json
{
  "title": "saisie utilisateur",
  "description": "saisie utilisateur",
  "application_id": "saisie utilisateur",
  "status": "OPEN",
  "severity": "saisie utilisateur (LOW / HIGH / CRITICAL)",
  "started_at": "date/heure courante au format ISO 8601"
}
```

**Payload modification :**

```json
{
  "title": "saisie utilisateur",
  "description": "saisie utilisateur",
  "status": "saisie utilisateur (OPEN / IN_PROGRESS / RESOLVED / CLOSED)",
  "severity": "saisie utilisateur (LOW / HIGH / CRITICAL)"
}
```

> **💡 Note :** Pour résoudre (route 17) et rouvrir (route 18), il n'y a pas de body à envoyer. Le script doit simplement demander l'ID de l'incident et envoyer la requête PUT.

---

## 4. Contraintes techniques

- Les opérations de suppression doivent demander une confirmation avant d'exécuter (`Êtes-vous sûr ? (o/n)`)

---

## 5. Barème de notation

### Endpoints (18 points)

Chaque endpoint fonctionnel et interactif vaut **1 point** :

| # | Endpoint | Points |
|---|----------|:------:|
| 1 | Login (POST /auth/login) | 1,5 |
| 2 | Lister les groupes | 1 |
| 3 | Voir un groupe | 1 |
| 4 | Créer un groupe | 1,5 |
| 5 | Modifier un groupe | 1 |
| 6 | Supprimer un groupe | 1 |
| 7 | Lister les applications | 1 |
| 8 | Voir une application | 1 |
| 9 | Créer une application | 1,5 |
| 10 | Modifier une application | 1 |
| 11 | Supprimer une application | 1 |
| 12 | Lister les incidents | 1 |
| 13 | Voir un incident | 1 |
| 14 | Créer un incident | 1,5 |
| 15 | Modifier un incident | 1 |
| 16 | Supprimer un incident | 1 |
| 17 | Résoudre un incident | 1 |
| 18 | Rouvrir un incident | 1 |
| | **Sous-total endpoints** | **20** |

## 6. Ressources utiles

### API MonitoringApp

- **Documentation complète :** [https://monitoring-app.on-forge.com/docs](https://monitoring-app.on-forge.com/docs)
- **Swagger (test en ligne) :** [https://monitoring-app.on-forge.com/swagger](https://monitoring-app.on-forge.com/swagger)
- **dashboard web :** [https://frontend-monitoring-app.on-forge.com/fr/login](https://frontend-monitoring-app.on-forge.com/fr/login)

---

*Bon courage et bon scripting !*
