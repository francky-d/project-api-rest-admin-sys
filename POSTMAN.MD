# Guide Postman — JSONPlaceholder API

**Cours :** Introduction à l'API REST
**Formateur :** Franck DJACOTO
**Objectif :** Créer une collection Postman complète pour les ressources **Posts**, **Comments** et **Todos**

**URL de base :** `https://jsonplaceholder.typicode.com`

> **⚠️ Important :** JSONPlaceholder est une API de test. Les opérations POST, PUT, PATCH et DELETE sont simulées : le serveur renvoie une réponse comme si l'opération avait réussi, mais rien n'est réellement modifié côté serveur.

---

## Organisation de votre collection Postman

Créez une collection nommée **"JSONPlaceholder"** avec 3 dossiers :

```
📁 JSONPlaceholder
├── 📁 Posts
├── 📁 Comments
└── 📁 Todos
```

---
---

# 📁 Posts

Un post représente un article de blog.

**Structure d'un post :**

```json
{
  "id": 1,
  "userId": 1,
  "title": "Titre du post",
  "body": "Contenu du post"
}
```

---

## 1. Lister tous les posts

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/posts` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

**Réponse :**

```json
[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit recusandae..."
  },
  {
    "userId": 1,
    "id": 2,
    "title": "qui est esse",
    "body": "est rerum tempore vitae..."
  }
]
```

> **💡 Note :** L'API renvoie 100 posts au total.

---

## 2. Récupérer un post par son ID

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/posts/1` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

**Réponse :**

```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur..."
}
```

---

## 3. Filtrer les posts par userId

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/posts?userId=1` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

**Réponse :** Tous les posts de l'utilisateur 1 (10 posts).

---

## 4. Récupérer les commentaires d'un post (route imbriquée)

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/posts/1/comments` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

**Réponse :**

```json
[
  {
    "postId": 1,
    "id": 1,
    "name": "id labore ex et quam laborum",
    "email": "Eliseo@gardner.biz",
    "body": "laudantium enim quasi est quidem magnam voluptate..."
  }
]
```

> **💡 Note :** Cette route est équivalente à `/comments?postId=1`

---

## 5. Créer un nouveau post

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `POST` |
| **URL** | `https://jsonplaceholder.typicode.com/posts` |
| **Headers** | `Content-Type: application/json` |
| **Code attendu** | `201 Created` |

**Body (raw JSON) :**

```json
{
  "title": "Mon premier post",
  "body": "Ceci est le contenu de mon post de test",
  "userId": 1
}
```

**Réponse :**

```json
{
  "title": "Mon premier post",
  "body": "Ceci est le contenu de mon post de test",
  "userId": 1,
  "id": 101
}
```

---

## 6. Modifier un post entièrement (PUT)

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `PUT` |
| **URL** | `https://jsonplaceholder.typicode.com/posts/1` |
| **Headers** | `Content-Type: application/json` |
| **Code attendu** | `200 OK` |

**Body (raw JSON) :**

```json
{
  "id": 1,
  "title": "Post modifié entièrement",
  "body": "Nouveau contenu complet du post",
  "userId": 1
}
```

**Réponse :**

```json
{
  "id": 1,
  "title": "Post modifié entièrement",
  "body": "Nouveau contenu complet du post",
  "userId": 1
}
```

> **⚠️ Rappel :** PUT remplace TOUT l'objet. Il faut renvoyer tous les champs.

---

## 7. Modifier partiellement un post (PATCH)

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `PATCH` |
| **URL** | `https://jsonplaceholder.typicode.com/posts/1` |
| **Headers** | `Content-Type: application/json` |
| **Code attendu** | `200 OK` |

**Body (raw JSON) :**

```json
{
  "title": "Seulement le titre est modifié"
}
```

**Réponse :**

```json
{
  "userId": 1,
  "id": 1,
  "title": "Seulement le titre est modifié",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur..."
}
```

> **💡 Note :** Seul le champ `title` a changé. Les autres champs restent intacts. C'est la différence avec PUT.

---

## 8. Supprimer un post

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `DELETE` |
| **URL** | `https://jsonplaceholder.typicode.com/posts/1` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

**Réponse :**

```json
{}
```

---
---

# 📁 Comments

Un commentaire est lié à un post via le champ `postId`.

**Structure d'un comment :**

```json
{
  "postId": 1,
  "id": 1,
  "name": "Titre du commentaire",
  "email": "auteur@example.com",
  "body": "Contenu du commentaire"
}
```

---

## 9. Lister tous les commentaires

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/comments` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

> **💡 Note :** L'API renvoie 500 commentaires au total.

---

## 10. Récupérer un commentaire par son ID

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/comments/1` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

**Réponse :**

```json
{
  "postId": 1,
  "id": 1,
  "name": "id labore ex et quam laborum",
  "email": "Eliseo@gardner.biz",
  "body": "laudantium enim quasi est quidem magnam voluptate ipsam eos\ntempora quo..."
}
```

---

## 11. Filtrer les commentaires par postId

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/comments?postId=1` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

**Réponse :** Tous les commentaires du post 1 (5 commentaires).

> **💡 Note :** Cette route est équivalente à `/posts/1/comments`

---

## 12. Créer un nouveau commentaire

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `POST` |
| **URL** | `https://jsonplaceholder.typicode.com/comments` |
| **Headers** | `Content-Type: application/json` |
| **Code attendu** | `201 Created` |

**Body (raw JSON) :**

```json
{
  "postId": 1,
  "name": "Mon commentaire",
  "email": "etudiant@admin-sys.fr",
  "body": "Ceci est un commentaire de test sur le post 1"
}
```

**Réponse :**

```json
{
  "postId": 1,
  "name": "Mon commentaire",
  "email": "etudiant@admin-sys.fr",
  "body": "Ceci est un commentaire de test sur le post 1",
  "id": 501
}
```

---

## 13. Modifier un commentaire entièrement (PUT)

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `PUT` |
| **URL** | `https://jsonplaceholder.typicode.com/comments/1` |
| **Headers** | `Content-Type: application/json` |
| **Code attendu** | `200 OK` |

**Body (raw JSON) :**

```json
{
  "postId": 1,
  "id": 1,
  "name": "Commentaire modifié",
  "email": "etudiant@admin-sys.fr",
  "body": "Contenu entièrement remplacé"
}
```

---

## 14. Modifier partiellement un commentaire (PATCH)

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `PATCH` |
| **URL** | `https://jsonplaceholder.typicode.com/comments/1` |
| **Headers** | `Content-Type: application/json` |
| **Code attendu** | `200 OK` |

**Body (raw JSON) :**

```json
{
  "body": "Seul le contenu du commentaire est modifié"
}
```

---

## 15. Supprimer un commentaire

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `DELETE` |
| **URL** | `https://jsonplaceholder.typicode.com/comments/1` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

---
---

# 📁 Todos

Un todo représente une tâche à faire, liée à un utilisateur via `userId`.

**Structure d'un todo :**

```json
{
  "userId": 1,
  "id": 1,
  "title": "Titre de la tâche",
  "completed": false
}
```

---

## 16. Lister tous les todos

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/todos` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

> **💡 Note :** L'API renvoie 200 todos au total.

---

## 17. Récupérer un todo par son ID

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/todos/1` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

**Réponse :**

```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

---

## 18. Filtrer les todos par userId

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/todos?userId=1` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

**Réponse :** Tous les todos de l'utilisateur 1 (20 todos).

---

## 19. Filtrer les todos complétés

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `GET` |
| **URL** | `https://jsonplaceholder.typicode.com/todos?completed=true` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

**Réponse :** Tous les todos dont `completed` est `true`.

---

## 20. Créer un nouveau todo

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `POST` |
| **URL** | `https://jsonplaceholder.typicode.com/todos` |
| **Headers** | `Content-Type: application/json` |
| **Code attendu** | `201 Created` |

**Body (raw JSON) :**

```json
{
  "userId": 1,
  "title": "Finir le TP API REST",
  "completed": false
}
```

**Réponse :**

```json
{
  "userId": 1,
  "title": "Finir le TP API REST",
  "completed": false,
  "id": 201
}
```

---

## 21. Modifier un todo entièrement (PUT)

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `PUT` |
| **URL** | `https://jsonplaceholder.typicode.com/todos/1` |
| **Headers** | `Content-Type: application/json` |
| **Code attendu** | `200 OK` |

**Body (raw JSON) :**

```json
{
  "userId": 1,
  "id": 1,
  "title": "Tâche modifiée entièrement",
  "completed": true
}
```

---

## 22. Marquer un todo comme complété (PATCH)

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `PATCH` |
| **URL** | `https://jsonplaceholder.typicode.com/todos/1` |
| **Headers** | `Content-Type: application/json` |
| **Code attendu** | `200 OK` |

**Body (raw JSON) :**

```json
{
  "completed": true
}
```

**Réponse :**

```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": true
}
```

> **💡 Note :** Cas d'usage typique de PATCH — on modifie seulement le statut `completed` sans toucher au reste.

---

## 23. Supprimer un todo

| Propriété | Valeur |
|-----------|--------|
| **Méthode** | `DELETE` |
| **URL** | `https://jsonplaceholder.typicode.com/todos/1` |
| **Body** | Aucun |
| **Code attendu** | `200 OK` |

---
---

# Récapitulatif des routes

## Posts (8 routes)

| # | Méthode | URL | Description |
|---|---------|-----|-------------|
| 1 | GET | `/posts` | Lister tous les posts |
| 2 | GET | `/posts/1` | Récupérer le post 1 |
| 3 | GET | `/posts?userId=1` | Filtrer les posts par utilisateur |
| 4 | GET | `/posts/1/comments` | Commentaires du post 1 |
| 5 | POST | `/posts` | Créer un post |
| 6 | PUT | `/posts/1` | Modifier entièrement le post 1 |
| 7 | PATCH | `/posts/1` | Modifier partiellement le post 1 |
| 8 | DELETE | `/posts/1` | Supprimer le post 1 |

## Comments (7 routes)

| # | Méthode | URL | Description |
|---|---------|-----|-------------|
| 9 | GET | `/comments` | Lister tous les commentaires |
| 10 | GET | `/comments/1` | Récupérer le commentaire 1 |
| 11 | GET | `/comments?postId=1` | Filtrer par post |
| 12 | POST | `/comments` | Créer un commentaire |
| 13 | PUT | `/comments/1` | Modifier entièrement |
| 14 | PATCH | `/comments/1` | Modifier partiellement |
| 15 | DELETE | `/comments/1` | Supprimer |

## Todos (8 routes)

| # | Méthode | URL | Description |
|---|---------|-----|-------------|
| 16 | GET | `/todos` | Lister tous les todos |
| 17 | GET | `/todos/1` | Récupérer le todo 1 |
| 18 | GET | `/todos?userId=1` | Filtrer par utilisateur |
| 19 | GET | `/todos?completed=true` | Filtrer les complétés |
| 20 | POST | `/todos` | Créer un todo |
| 21 | PUT | `/todos/1` | Modifier entièrement |
| 22 | PATCH | `/todos/1` | Marquer comme complété |
| 23 | DELETE | `/todos/1` | Supprimer |

**Total : 23 requêtes à sauvegarder dans votre collection Postman.**

---

> **📌 N'oubliez pas :** Sauvegardez chaque requête dans Postman avec la réponse correspondante. Utilisez le bouton **Save Response** après chaque appel pour garder une trace des résultats.

*Bon TP !*