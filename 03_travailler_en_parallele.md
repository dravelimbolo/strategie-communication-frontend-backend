# 3. Travailler en parallèle grâce aux mocks

## 3.1 Le problème « le Frontend attend le Backend »

C'est une source classique de conflit.

Au lieu de faire :

```text
Le Backend termine
       ↓
Le Frontend attend
       ↓
Intégration
       ↓
Problèmes
       ↓
Corrections
```

Faites :

```text
       CONTRAT D'API
            │
     ┌──────┴──────┐
     ↓             ↓
  Backend       Frontend
     ↓             ↓
 API réelle     API simulée (mock)
     └──────┬──────┘
            ↓
       INTÉGRATION
```

Le Frontend travaille avec des **données simulées** (mockées) qui respectent le contrat, pendant que le Backend développe l'API réelle.

Par exemple, d'après le contrat, `GET /api/users/15` renvoie :

```json
{
  "id": 15,
  "name": "Dravel",
  "email": "dravel@example.com"
}
```

Le Frontend construit son interface avec cette structure sans attendre que l'API soit terminée. Quand l'API réelle est prête, on remplace le mock par la vraie URL : si les deux équipes ont respecté le contrat, l'intégration se passe sans surprise.

## 3.2 Trois façons de simuler l'API

### Prism : un mock généré depuis le contrat

Prism lit le fichier OpenAPI et démarre un faux serveur qui répond avec les exemples du contrat :

```bash
npx @stoplight/prism-cli mock docs/openapi.yaml
```

Le mock écoute par défaut sur `http://127.0.0.1:4010`. C'est la solution la plus fiable : le mock est **toujours** conforme au contrat, puisqu'il est généré à partir de lui.

### MSW : intercepter les requêtes dans le navigateur

MSW (Mock Service Worker) intercepte les appels `fetch` du Frontend et renvoie des réponses définies en JavaScript. Le code du Frontend ne change pas : il appelle les vraies URL.

```bash
npm install msw --save-dev
npx msw init public/ --save
```

Fichier `src/mocks/handlers.js` :

```js
import { http, HttpResponse } from 'msw';

export const handlers = [
  http.get('/api/users', () => {
    return HttpResponse.json({
      data: [{ id: 15, name: 'Dravel', email: 'dravel@example.com' }],
      page: 1,
      limit: 20,
      total: 1,
    });
  }),

  http.post('/api/users', async ({ request }) => {
    const body = await request.json();
    return HttpResponse.json({ id: 16, ...body }, { status: 201 });
  }),
];
```

Fichier `src/mocks/browser.js` :

```js
import { setupWorker } from 'msw/browser';
import { handlers } from './handlers';

export const worker = setupWorker(...handlers);
```

Démarrage dans `src/main.jsx`, uniquement en développement et si la variable `VITE_USE_MOCKS` vaut `true` :

```js
async function enableMocks() {
  if (import.meta.env.DEV && import.meta.env.VITE_USE_MOCKS === 'true') {
    const { worker } = await import('./mocks/browser');
    await worker.start();
  }
}

enableMocks().then(() => {
  // rendu de l'application React habituel
});
```

Avantage : les mêmes handlers servent aussi dans les tests du Frontend.

### json-server : une fausse API à partir d'un fichier JSON

Pour un prototype rapide, `json-server` crée une API complète à partir d'un fichier `db.json`. Il ne respecte pas forcément le contrat (format d'erreur, pagination) : à réserver aux tout premiers essais.

## 3.3 Bonnes pratiques

- Les données du mock viennent des **exemples du contrat**, pas de l'imagination du développeur Frontend.
- Simuler aussi les **erreurs** (`400`, `401`, `409`, `500`) et les **temps de chargement** : l'interface doit les gérer avant l'intégration.
- Le mock est versionné dans le dépôt, pour que toute l'équipe Frontend utilise le même.
- Quand le contrat change, le mock change dans la même pull request.
- Le mock ne part jamais en production : il est activé uniquement en développement.
