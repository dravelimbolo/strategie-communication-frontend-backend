# 5. Des conventions communes

## 5.1 Décider avant de coder

Avant de commencer le développement, les deux équipes se mettent d'accord sur un petit ensemble de règles. Cela évite énormément de discussions inutiles pendant le projet.

Ces décisions sont écrites dans le dépôt :

| Fichier | Contenu |
|---|---|
| `README.md` | Présentation du projet, installation, lancement |
| `docs/conventions.md` | Choix techniques et conventions de code |
| `docs/openapi.yaml` | Contrat d'API (chapitre 2) |
| `CONTRIBUTING.md` | Processus de travail : branches, commits, pull requests, relecture |
| `.env.example` | Variables d'environnement attendues |

## 5.2 Exemple de `docs/conventions.md`

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
- Contrat OpenAPI dans docs/openapi.yaml
- Authentification par JWT
- Format d'erreur unique (chapitre 2)

Git
- Branches feature/*, fix/*, hotfix/*
- Conventional Commits
- Pull request obligatoire avec relecture
```

Le choix de JavaScript ou de TypeScript importe moins que le fait que **tout le monde fasse le même choix**. TypeScript apporte une sécurité supplémentaire : avec un outil comme `openapi-typescript`, les types du Frontend peuvent même être générés directement à partir du contrat.

## 5.3 Les mêmes outils de qualité des deux côtés

- **ESLint** et **Prettier** configurés de la même façon dans `frontend/` et `backend/` ;
- un script `npm run lint` et un script `npm test` dans chaque projet ;
- la même version de Node.js partout, fixée dans un fichier `.nvmrc` ;
- la CI lance le lint et les tests des deux projets sur chaque pull request.

Un formatage automatique commun supprime les débats sur le style (espaces, guillemets, points-virgules) : l'outil décide, pas les personnes.

## 5.4 Variables d'environnement partagées

Le Frontend a besoin de connaître l'URL de l'API, et le Backend l'origine autorisée du Frontend. Ces valeurs sont documentées dans des fichiers `.env.example` versionnés :

`frontend/.env.example` :

```env
# Vide en développement : les appels /api passent par le proxy de Vite (section 5.5)
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

## 5.5 CORS en développement

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

## 5.6 Une URL de staging commune

Les deux équipes doivent connaître et utiliser les mêmes environnements :

| Environnement | Frontend | API |
|---|---|---|
| Local | `http://localhost:5173` | `http://localhost:3000` |
| Staging | `https://staging.example.com` | `https://api-staging.example.com` |
| Production | `https://app.example.com` | `https://api.example.com` |

Ces URL sont écrites dans le README pour que personne n'ait à les demander.
