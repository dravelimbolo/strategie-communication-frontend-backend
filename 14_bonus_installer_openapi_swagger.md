# 14. Bonus : installer et configurer OpenAPI et Swagger

Ce chapitre met en place, pas à pas, tous les outils autour du contrat d'API. Chaque commande et chaque fichier ont été testés : vous pouvez les reproduire tels quels.

## 14.1 Ce que l'on va mettre en place

| Étape | Outil | Résultat |
|---|---|---|
| 1 | Éditeur (Swagger Editor ou VS Code) | Écrire le contrat confortablement |
| 2 | `docs/openapi.yaml` | Le contrat d'API complet |
| 3 | Redocly CLI | Vérifier que le contrat est valide |
| 4 | Swagger UI (`swagger-ui-express`) | Documentation interactive sur `/api/docs` |
| 5 | `express-openapi-validator` | Le Backend refuse automatiquement les requêtes non conformes |
| 6 | Prism | Le Frontend travaille avec un mock généré depuis le contrat |
| 7 | GitHub Actions | Le contrat est vérifié à chaque pull request |
| 8 | `openapi-typescript` (optionnel) | Types générés pour un Frontend en TypeScript |

Arborescence visée (monorepo) :

```text
projet/
├── docs/
│   └── openapi.yaml          contrat d'API
├── redocly.yaml              configuration du lint du contrat
├── backend/
│   ├── src/
│   │   ├── app.js            Swagger UI + validation
│   │   └── server.js
│   └── package.json
└── frontend/
    ├── vite.config.js        proxy vers l'API réelle ou le mock
    └── package.json
```

Prérequis : Node.js 20 ou plus récent, un Backend Express et un Frontend Vite déjà créés. Les exemples ont été testés avec Node.js 22, Express 5, swagger-ui-express 5, express-openapi-validator 5, Redocly CLI 2 et Prism 5.

## 14.2 Étape 1 : choisir un éditeur

Deux possibilités :

- **Swagger Editor en ligne** (editor.swagger.io) : coller le contenu du fichier à gauche, la documentation s'affiche à droite et les erreurs sont signalées en direct. Pratique pour apprendre. Pensez à recopier le résultat dans `docs/openapi.yaml` : l'éditeur en ligne ne sauvegarde pas dans votre dépôt.
- **VS Code** avec une extension OpenAPI (par exemple l'extension Redocly ou « OpenAPI (Swagger) Editor ») : autocomplétion, aperçu et détection des erreurs directement dans le projet.

Règles de base du YAML, source de la plupart des erreurs des débutants :

- l'indentation se fait avec **2 espaces**, jamais avec des tabulations ;
- l'indentation a un sens : un élément décalé change de parent ;
- les codes HTTP sont écrits entre guillemets : `'201'` ;
- un `#` commence un commentaire.

## 14.3 Étape 2 : écrire le contrat complet

Fichier `docs/openapi.yaml`. Il reprend l'exemple du chapitre 8 et ajoute la liste paginée, l'authentification et les paramètres réutilisables :

```yaml
openapi: 3.1.0
info:
  title: API MyApp
  version: 1.0.0
  description: Contrat d'API partagé entre l'équipe Frontend et l'équipe Backend.
  license:
    name: MIT
    identifier: MIT

servers:
  - url: http://localhost:3000
    description: Développement
  - url: https://api-staging.example.com
    description: Staging

tags:
  - name: Utilisateurs
    description: Gestion des utilisateurs

security:
  - bearerAuth: []

paths:
  /api/users:
    get:
      operationId: listUsers
      summary: Lister les utilisateurs
      tags: [Utilisateurs]
      parameters:
        - $ref: '#/components/parameters/Page'
        - $ref: '#/components/parameters/Limit'
      responses:
        '200':
          description: Liste paginée des utilisateurs
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserList'
        '401':
          $ref: '#/components/responses/Error'

    post:
      operationId: createUser
      summary: Créer un utilisateur
      tags: [Utilisateurs]
      security: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UserInput'
      responses:
        '201':
          description: Utilisateur créé
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          $ref: '#/components/responses/Error'
        '409':
          $ref: '#/components/responses/Error'

components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

  parameters:
    Page:
      name: page
      in: query
      required: false
      schema:
        type: integer
        minimum: 1
        default: 1
    Limit:
      name: limit
      in: query
      required: false
      schema:
        type: integer
        minimum: 1
        maximum: 100
        default: 20

  schemas:
    UserInput:
      type: object
      required: [name, email]
      additionalProperties: false
      properties:
        name:
          type: string
          minLength: 2
          maxLength: 100
          example: Dravel
        email:
          type: string
          format: email
          example: dravel@example.com

    User:
      type: object
      required: [id, name, email]
      properties:
        id:
          type: integer
          example: 15
        name:
          type: string
          example: Dravel
        email:
          type: string
          format: email
          example: dravel@example.com

    UserList:
      type: object
      required: [data, page, limit, total]
      properties:
        data:
          type: array
          items:
            $ref: '#/components/schemas/User'
        page:
          type: integer
          example: 1
        limit:
          type: integer
          example: 20
        total:
          type: integer
          example: 1

    Error:
      type: object
      required: [error]
      properties:
        error:
          type: object
          required: [code, message]
          properties:
            code:
              type: string
              example: VALIDATION_ERROR
            message:
              type: string
              example: Les données envoyées sont invalides.
            details:
              type: array
              items:
                type: object
                properties:
                  field:
                    type: string
                  message:
                    type: string

  responses:
    Error:
      description: Erreur au format commun
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
```

Ce qui est nouveau par rapport au chapitre 8 :

| Élément | Explication |
|---|---|
| `tags` | Regroupe les routes par thème dans la documentation Swagger |
| `security` (à la racine) | Par défaut, **toutes** les routes exigent un token JWT |
| `security: []` (sur `POST /api/users`) | Exception : l'inscription est publique |
| `securitySchemes.bearerAuth` | Décrit l'authentification : en-tête `Authorization: Bearer <token>` |
| `parameters` | Paramètres réutilisables `page` et `limit`, avec minimum, maximum et valeur par défaut |
| `additionalProperties: false` | Refuse tout champ non prévu. Empêche par exemple un utilisateur d'envoyer `"role": "admin"` à l'inscription |
| `UserList` | La pagination commune du chapitre 3 (`data`, `page`, `limit`, `total`) |

## 14.4 Étape 3 : vérifier le contrat avec Redocly

Redocly CLI détecte les erreurs du contrat (référence `$ref` cassée, champ obligatoire manquant, mauvaise structure).

Fichier `redocly.yaml`, à la racine du projet :

```yaml
extends:
  - recommended

rules:
  no-server-example.com: off
```

- `extends: recommended` active les règles recommandées ;
- la règle `no-server-example.com` est désactivée, car les adresses `example.com` et `localhost` du contrat sont volontaires dans ce cours.

Installation dans le Backend, et script npm :

```bash
cd backend
npm install --save-dev @redocly/cli
npm pkg set scripts.lint:api="redocly lint ../docs/openapi.yaml --config ../redocly.yaml"
```

Lancement :

```bash
npm run lint:api
```

Résultat attendu :

```text
Woohoo! Your API description is valid.
```

Si une erreur apparaît, Redocly indique la ligne et la colonne concernées.

## 14.5 Étape 4 : publier la documentation avec Swagger UI

Swagger UI affiche le contrat sous forme de page web interactive : chaque route, ses paramètres, ses réponses, et un bouton **Try it out** pour l'appeler directement.

Installation :

```bash
cd backend
npm install swagger-ui-express yaml
```

- `swagger-ui-express` sert la page Swagger UI depuis Express ;
- `yaml` lit le fichier `openapi.yaml` et le transforme en objet JavaScript.

Début du fichier `backend/src/app.js` :

```js
const fs = require('fs');
const path = require('path');
const express = require('express');
const YAML = require('yaml');
const swaggerUi = require('swagger-ui-express');

const app = express();
app.use(express.json());

// 1. Chargement du contrat
const openapiPath = process.env.OPENAPI_PATH
  ? path.resolve(process.env.OPENAPI_PATH)
  : path.join(__dirname, '..', '..', 'docs', 'openapi.yaml');
const openapiDocument = YAML.parse(fs.readFileSync(openapiPath, 'utf8'));

// 2. Documentation interactive (hors production)
if (process.env.NODE_ENV !== 'production') {
  app.get('/api/openapi.json', (req, res) => res.json(openapiDocument));
  app.use('/api/docs', swaggerUi.serve, swaggerUi.setup(openapiDocument, {
    customSiteTitle: 'API MyApp',
    swaggerOptions: { persistAuthorization: true },
  }));
}

// 3. Route technique, hors contrat
app.get('/health', (req, res) => res.json({ status: 'ok' }));
```

Explications :

- `openapiPath` : par défaut, le contrat est lu dans `docs/openapi.yaml` à la racine du monorepo (`backend/src` puis deux dossiers plus haut). La variable `OPENAPI_PATH` permet de donner un autre chemin, utile en production (section 14.11) ;
- la documentation n'est servie **qu'en dehors de la production** : inutile d'exposer publiquement la liste de toutes les routes ;
- `/api/openapi.json` expose le contrat brut, pratique pour d'autres outils (Postman peut l'importer) ;
- `customSiteTitle` : titre de l'onglet du navigateur ;
- `persistAuthorization: true` : le token saisi dans Swagger UI est conservé quand on recharge la page.

Utilisation :

1. démarrer le Backend : `node src/server.js` ;
2. ouvrir `http://localhost:3000/api/docs` ;
3. pour une route protégée, cliquer sur **Authorize**, coller un token JWT, valider ;
4. ouvrir une route, cliquer sur **Try it out**, remplir les champs, puis **Execute** : la requête réelle et sa réponse s'affichent.

## 14.6 Étape 5 : valider automatiquement les requêtes

`express-openapi-validator` compare chaque requête au contrat. Si elle ne le respecte pas (champ manquant, email mal formé, champ en trop, `limit` supérieur à 100...), elle est refusée **avant** d'arriver dans votre code. La validation serveur (chapitre 4) est alors appliquée automatiquement, avec exactement les règles du contrat.

Installation :

```bash
cd backend
npm install express-openapi-validator
```

Suite du fichier `backend/src/app.js` :

```js
const OpenApiValidator = require('express-openapi-validator');

// 4. Validation automatique des requêtes à partir du contrat
app.use(
  OpenApiValidator.middleware({
    apiSpec: openapiPath,
    validateRequests: true,
    validateResponses: false,
    validateSecurity: false,
    ignorePaths: /^\/(health|api\/docs|api\/openapi\.json)/,
  })
);

// 5. Routes de l'application
const users = [{ id: 15, name: 'Dravel', email: 'dravel@example.com' }];

app.get('/api/users', (req, res) => {
  const page = Number(req.query.page) || 1;
  const limit = Number(req.query.limit) || 20;
  res.json({ data: users, page, limit, total: users.length });
});

app.post('/api/users', (req, res) => {
  if (users.some((u) => u.email === req.body.email)) {
    return res.status(409).json({
      error: { code: 'EMAIL_ALREADY_USED', message: 'Cet email est déjà utilisé.' },
    });
  }
  const user = { id: users.length + 15, ...req.body };
  users.push(user);
  res.status(201).json(user);
});

// 6. Transformation des erreurs du validateur au format commun
app.use((err, req, res, next) => {
  if (err.status && err.errors) {
    return res.status(err.status).json({
      error: {
        code: err.status === 404 ? 'NOT_FOUND' : 'VALIDATION_ERROR',
        message: err.status === 404 ? 'Route introuvable.' : 'Les données envoyées sont invalides.',
        details: err.errors.map((e) => ({ field: e.path, message: e.message })),
      },
    });
  }
  console.error(err);
  res.status(500).json({ error: { code: 'INTERNAL_ERROR', message: 'Erreur interne.' } });
});

module.exports = app;
```

Fichier `backend/src/server.js` :

```js
const app = require('./app');

const port = process.env.PORT || 3000;
app.listen(port, () => {
  console.log(`API sur http://localhost:${port}, documentation sur http://localhost:${port}/api/docs`);
});
```

Les données sont gardées en mémoire pour l'exemple : dans un vrai projet, elles viennent de la base de données.

### L'ordre des middlewares est important

1. `express.json()` en premier : le validateur a besoin du corps de la requête déjà lu ;
2. la documentation et `/health` **avant** le validateur, et exclus avec `ignorePaths` : ces routes ne sont pas décrites dans le contrat ;
3. le validateur ;
4. les routes de l'application ;
5. le gestionnaire d'erreurs **en dernier** (une fonction Express à 4 paramètres).

### Les options

| Option | Valeur | Pourquoi |
|---|---|---|
| `apiSpec` | Chemin du contrat | Le validateur lit le même fichier que Swagger UI |
| `validateRequests` | `true` | Vérifie paramètres, corps et formats des requêtes |
| `validateResponses` | `false` | Vérifier aussi les réponses du Backend est utile en développement et dans les tests, mais coûte en performance en production |
| `validateSecurity` | `false` | L'authentification JWT est vérifiée par votre propre middleware, pas par le validateur |
| `ignorePaths` | Expression régulière | Routes techniques hors contrat |

Le gestionnaire d'erreurs transforme les erreurs du validateur au **format d'erreur commun** du chapitre 3, pour que le Frontend les traite comme toutes les autres.

### Vérifier avec curl

Création valide :

```bash
curl -H 'Content-Type: application/json' -d '{"name":"Awa","email":"awa@example.com"}' http://localhost:3000/api/users
```

```json
{"id":16,"name":"Awa","email":"awa@example.com"}
```

Email mal formé, réponse `400` :

```bash
curl -H 'Content-Type: application/json' -d '{"name":"Awa","email":"pas-un-email"}' http://localhost:3000/api/users
```

```json
{"error":{"code":"VALIDATION_ERROR","message":"Les données envoyées sont invalides.","details":[{"field":"/body/email","message":"must match format \"email\""}]}}
```

Champ non prévu, réponse `400` grâce à `additionalProperties: false` :

```bash
curl -H 'Content-Type: application/json' -d '{"name":"Awa","email":"a@b.com","role":"admin"}' http://localhost:3000/api/users
```

```json
{"error":{"code":"VALIDATION_ERROR","message":"Les données envoyées sont invalides.","details":[{"field":"/body/role","message":"must NOT have additional properties"}]}}
```

Paramètre hors limites, réponse `400` :

```bash
curl "http://localhost:3000/api/users?limit=500"
```

```json
{"error":{"code":"VALIDATION_ERROR","message":"Les données envoyées sont invalides.","details":[{"field":"/query/limit","message":"must be <= 100"}]}}
```

Route absente du contrat, réponse `404` :

```bash
curl http://localhost:3000/api/inconnue
```

```json
{"error":{"code":"NOT_FOUND","message":"Route introuvable.","details":[{"field":"/api/inconnue","message":"not found"}]}}
```

Conséquence importante : **une route qui n'est pas dans le contrat n'est pas accessible**. Le Backend ne peut plus ajouter une route « en douce » : il doit d'abord l'ajouter au contrat, donc passer par une pull request relue par le Frontend.

## 14.7 Étape 6 : le mock Prism pour le Frontend

Installation dans le Frontend, et script npm :

```bash
cd frontend
npm install --save-dev @stoplight/prism-cli
npm pkg set scripts.mock="prism mock ../docs/openapi.yaml"
```

Lancement :

```bash
npm run mock
```

Le mock écoute sur `http://127.0.0.1:4010` et répond avec les exemples du contrat :

```bash
curl -H 'Authorization: Bearer test' http://127.0.0.1:4010/api/users
```

```json
{"data":[{"id":15,"name":"Dravel","email":"dravel@example.com"}],"page":1,"limit":20,"total":1}
```

Prism respecte aussi le contrat pour les erreurs : sans en-tête `Authorization`, `GET /api/users` répond `401` ; un corps invalide sur `POST /api/users` répond `400`.

Pour que le Frontend utilise le mock ou l'API réelle sans changer son code, le proxy de Vite lit sa cible dans une variable. Fichier `frontend/vite.config.js` :

```js
import { defineConfig, loadEnv } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig(({ mode }) => {
  const env = loadEnv(mode, process.cwd(), '');

  return {
    plugins: [react()],
    server: {
      proxy: {
        '/api': env.API_PROXY_TARGET || 'http://localhost:3000',
      },
    },
  };
});
```

- avec l'API réelle : `npm run dev` ;
- avec le mock : lancer `npm run mock` dans un terminal, puis `API_PROXY_TARGET=http://127.0.0.1:4010 npm run dev` dans un autre.

## 14.8 Étape 7 : vérifier le contrat dans la CI

Job à ajouter dans `.github/workflows/ci.yml` :

```yaml
  contrat-api:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      - uses: actions/setup-node@v5
        with:
          node-version-file: .nvmrc
      - run: npx @redocly/cli lint docs/openapi.yaml --config redocly.yaml
```

Une pull request qui casse le contrat (YAML invalide, référence introuvable) est bloquée avant la relecture. Le cours de déploiement (chapitre 6) détaille le reste du workflow CI.

## 14.9 Étape 8 (optionnel) : générer les types du Frontend

Si le Frontend est écrit en TypeScript, `openapi-typescript` génère les types directement depuis le contrat :

```bash
npx openapi-typescript docs/openapi.yaml -o frontend/src/api/schema.d.ts
```

Le fichier généré contient par exemple le type `User` avec `id`, `name` et `email`. Si le contrat change, on régénère : TypeScript signale aussitôt chaque endroit du Frontend à adapter.

## 14.10 Alternative : l'approche code-first

Avec `swagger-jsdoc`, le contrat est écrit en commentaires au-dessus de chaque route, puis assemblé automatiquement :

```js
/**
 * @openapi
 * /api/users:
 *   get:
 *     summary: Lister les utilisateurs
 *     responses:
 *       200:
 *         description: Liste paginée des utilisateurs
 */
app.get('/api/users', listUsers);
```

Cette approche garde la documentation près du code, mais le contrat n'existe qu'une fois la route codée : le Frontend ne peut pas travailler en parallèle, et la relecture du contrat par les deux équipes devient difficile. Ce cours recommande donc l'approche **design-first** (un fichier `openapi.yaml` écrit avant le code).

## 14.11 En production

- La documentation Swagger UI est désactivée (`NODE_ENV=production`), mais le validateur a toujours besoin du contrat.
- Si seul le dossier `backend/` est envoyé sur le serveur (c'est le cas dans le cours de déploiement), copier le contrat dans le Backend avant l'envoi, par exemple dans le workflow de déploiement :

```bash
cp docs/openapi.yaml backend/openapi.yaml
```

- Puis indiquer son emplacement dans le fichier d'environnement du serveur :

```env
OPENAPI_PATH=openapi.yaml
```

Le chemin est relatif au dossier depuis lequel l'application est lancée (le dossier `current` de la release, géré par PM2).

## 14.12 Organiser un gros contrat

Quand le contrat grossit, on peut le découper en plusieurs fichiers reliés par des `$ref` (par exemple `docs/paths/users.yaml`), puis les rassembler en un seul fichier avec :

```bash
npx @redocly/cli bundle docs/openapi.yaml -o docs/dist/openapi.yaml
```

Pour un projet d'apprentissage, un seul fichier suffit largement.

## 14.13 Dépannage

| Symptôme | Cause probable | Solution |
|---|---|---|
| `YAMLParseError` au démarrage | Indentation incorrecte ou tabulation | Vérifier l'indentation (2 espaces) à la ligne indiquée |
| `ENOENT: no such file or directory ... openapi.yaml` | Mauvais chemin du contrat | Vérifier `openapiPath` ou `OPENAPI_PATH` |
| Redocly signale une erreur sur un `$ref` | Faute de frappe dans une référence | Comparer le chemin du `$ref` avec le nom dans `components` |
| `/api/docs` répond 404 | Application lancée avec `NODE_ENV=production` | Lancer en développement, sans `NODE_ENV=production` |
| Toutes les nouvelles routes répondent 404 `NOT_FOUND` | Route absente du contrat | Ajouter la route dans `docs/openapi.yaml` |
| 400 `must NOT have additional properties` | Le Frontend envoie un champ non prévu | Retirer le champ, ou l'ajouter au contrat après accord |
| Prism répond 401 | En-tête `Authorization` absent sur une route protégée | Envoyer `Authorization: Bearer <token>` (n'importe quelle valeur avec le mock) |
| `Cannot find module 'swagger-ui-express'` | Dépendance non installée | `npm install` dans le dossier `backend/` |

## 14.14 Récapitulatif

Scripts du Backend (`backend/package.json`) :

| Script | Commande | Rôle |
|---|---|---|
| `lint:api` | `redocly lint ../docs/openapi.yaml --config ../redocly.yaml` | Vérifier le contrat |

Scripts du Frontend (`frontend/package.json`) :

| Script | Commande | Rôle |
|---|---|---|
| `mock` | `prism mock ../docs/openapi.yaml` | Démarrer le mock sur le port 4010 |

Adresses utiles en développement :

| Adresse | Contenu |
|---|---|
| `http://localhost:3000/api/docs` | Documentation Swagger UI |
| `http://localhost:3000/api/openapi.json` | Contrat brut au format JSON |
| `http://127.0.0.1:4010` | Mock Prism |
| `http://localhost:5173` | Frontend Vite |
