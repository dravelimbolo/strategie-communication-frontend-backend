# 3. Les conventions communes

## 3.1 Décider avant de coder

Une convention est une règle de forme que tout le monde applique de la même façon. Les deux équipes les décident ensemble **avant** de commencer le développement. Cela évite énormément de discussions inutiles pendant le projet, et règle les problèmes « les données ne correspondent pas » et « ça marche chez moi » (chapitre 2).

Le choix fait importe souvent moins que le fait que **tout le monde fasse le même choix**.

Les conventions sont écrites dans le dépôt :

| Fichier | Contenu |
|---|---|
| `docs/conventions.md` | Choix techniques et conventions (ce chapitre) |
| `docs/contrat-equipe.md` | Contrat d'équipe complet (chapitre 7) |
| `.env.example` | Variables d'environnement attendues |
| `CONTRIBUTING.md` | Processus de travail : branches, commits, pull requests |

## 3.2 Conventions techniques

Exemple de `docs/conventions.md` :

```text
Frontend
- React avec Vite
- JavaScript
- ESLint et Prettier
- Appels à l'API regroupés dans src/api/

Backend
- Node.js et Express
- JavaScript
- ESLint et Prettier
- Une route = un contrôleur, la logique métier dans des services

API
- REST et JSON
- Authentification par JWT
- Contrat écrit au format OpenAPI dans docs/openapi.yaml

Git
- Branches feature/*, fix/*, hotfix/*
- Conventional Commits
- Pull request obligatoire avec relecture
```

JavaScript ou TypeScript : les deux conviennent, l'essentiel est que les deux équipes fassent le même choix.

## 3.3 Conventions de l'API

Ce sont les conventions les plus importantes, car elles touchent directement la frontière entre les deux équipes.

### Nommage

| Sujet | Convention conseillée |
|---|---|
| Routes | Préfixe `/api`, noms au pluriel : `/api/users`, `/api/users/:id` |
| Champs JSON | `camelCase` : `firstName`, `createdAt` |
| Dates | Format ISO 8601 en UTC : `2026-10-05T14:30:00Z` |
| Identifiants | Toujours le même type (nombre ou chaîne) dans toute l'API |
| Booléens | Préfixe `is` ou `has` : `isActive`, `hasPaid` |

### Méthodes HTTP

| Méthode | Usage | Exemple |
|---|---|---|
| `GET` | Lire | `GET /api/users/15` |
| `POST` | Créer | `POST /api/users` |
| `PATCH` | Modifier une partie | `PATCH /api/users/15` |
| `DELETE` | Supprimer | `DELETE /api/users/15` |

### Codes HTTP

| Code | Signification | Exemple |
|---|---|---|
| `200` | Succès | Lecture d'une ressource |
| `201` | Ressource créée | Création d'un utilisateur |
| `204` | Succès sans contenu | Suppression |
| `400` | Données invalides | Email mal formé |
| `401` | Non authentifié | Token absent ou expiré |
| `403` | Authentifié mais non autorisé | Un utilisateur tente une action d'administrateur |
| `404` | Ressource introuvable | Utilisateur inexistant |
| `409` | Conflit | Email déjà utilisé |
| `500` | Erreur interne | Bug côté serveur |

### Un format d'erreur unique

Toutes les erreurs de l'API ont la **même forme**. Le Frontend peut alors les afficher avec un seul morceau de code.

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Les données envoyées sont invalides.",
    "details": [
      { "field": "email", "message": "Format d'email invalide." }
    ]
  }
}
```

- `code` : identifiant stable, en majuscules, utilisé par le code du Frontend ;
- `message` : texte lisible, qui peut changer ;
- `details` : facultatif, pour les erreurs de validation champ par champ.

Une erreur `500` ne renvoie jamais de détail technique (pile d'appels, requête SQL) au Frontend.

### Une pagination commune

Toutes les listes sont paginées de la même façon :

```text
GET /api/users?page=2&limit=20
```

```json
{
  "data": [
    { "id": 21, "name": "Dravel", "email": "dravel@example.com" }
  ],
  "page": 2,
  "limit": 20,
  "total": 134
}
```

Ces conventions d'API seront ensuite écrites de façon précise et vérifiable dans le contrat OpenAPI (chapitre 8).

## 3.4 Conventions Git

Résumé (le détail est au chapitre 10) :

- branches `feature/front-...`, `feature/back-...`, `fix/...`, créées depuis `develop` ;
- messages de commit au format Conventional Commits : `feat(api): ajouter la route POST /api/users` ;
- aucune fusion sans pull request relue.

## 3.5 Les mêmes outils de qualité des deux côtés

- **ESLint** et **Prettier** configurés de la même façon dans `frontend/` et `backend/` ;
- un script `npm run lint` et un script `npm test` dans chaque projet ;
- la même version de Node.js partout, fixée dans un fichier `.nvmrc` ;
- la CI lance le lint et les tests des deux projets sur chaque pull request.

Un formatage automatique commun supprime les débats sur le style (espaces, guillemets, points-virgules) : l'outil décide, pas les personnes.

## 3.6 Variables d'environnement partagées

Le Frontend a besoin de connaître l'URL de l'API, et le Backend l'origine autorisée du Frontend. Ces valeurs sont documentées dans des fichiers `.env.example` versionnés.

`frontend/.env.example` :

```env
# Vide en développement : les appels /api passent par le proxy de Vite (section 3.7)
# En staging et en production : URL publique de l'API, par exemple https://api.example.com
VITE_API_URL=
VITE_USE_MOCKS=false
```

Côté Frontend, les appels sont alors écrits ainsi :

```js
const response = await fetch(`${import.meta.env.VITE_API_URL}/api/users`);
```

`backend/.env.example` :

```env
PORT=3000
CORS_ORIGIN=http://localhost:5173
DATABASE_URL=
JWT_SECRET=
```

Quand une équipe ajoute une variable, elle met à jour le `.env.example` dans la même pull request et prévient l'autre équipe.

## 3.7 CORS en développement

En développement, le Frontend (`http://localhost:5173`) et le Backend (`http://localhost:3000`) ont deux origines différentes. Le navigateur bloque les appels sauf si le Backend les autorise (CORS). C'est une source très fréquente de « ça marche chez moi ».

La solution la plus simple en développement : le proxy de Vite. Dans `frontend/vite.config.js` :

```js
export default {
  server: {
    proxy: {
      '/api': 'http://localhost:3000',
    },
  },
};
```

Le Frontend appelle `/api/users`, et Vite transmet la requête au Backend : le navigateur ne voit qu'une seule origine, il n'y a plus de problème CORS en développement.

En staging et en production, la configuration CORS côté Backend reste nécessaire si le Frontend et l'API sont sur deux domaines différents (voir le cours de déploiement, chapitre 2).

## 3.8 Des environnements communs

Les deux équipes connaissent et utilisent les mêmes environnements :

| Environnement | Frontend | API |
|---|---|---|
| Local | `http://localhost:5173` | `http://localhost:3000` |
| Staging | `https://staging.example.com` | `https://api-staging.example.com` |
| Production | `https://app.example.com` | `https://api.example.com` |

Ces URL sont écrites dans le README pour que personne n'ait à les demander.
