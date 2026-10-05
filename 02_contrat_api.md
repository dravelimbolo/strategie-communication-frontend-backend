# 2. Le contrat d'API

## 2.1 Rôle du contrat

Le Backend définit ce que l'application peut faire, et le Frontend consomme ces fonctionnalités. Le contrat d'API décrit précisément cette frontière.

Le contrat doit préciser :

- les routes : `GET /api/users`, `POST /api/users` ;
- les paramètres (chemin, requête, en-têtes) ;
- le format des données envoyées ;
- le format des réponses ;
- les codes HTTP ;
- le format des erreurs ;
- l'authentification ;
- les règles de validation.

Le contrat est écrit et validé par les **deux** équipes **avant** de coder. Il n'appartient ni au Frontend ni au Backend : c'est un document commun.

## 2.2 OpenAPI

**OpenAPI** (anciennement Swagger) est le format standard pour décrire une API REST. Le contrat est un fichier YAML versionné dans le dépôt, par exemple `docs/openapi.yaml`.

Exemple minimal pour la création d'un utilisateur :

```yaml
openapi: 3.1.0
info:
  title: API MyApp
  version: 1.0.0
  license:
    name: MIT
    identifier: MIT

servers:
  - url: http://localhost:3000
    description: Développement
  - url: https://api-staging.example.com
    description: Staging

paths:
  /api/users:
    post:
      operationId: createUser
      summary: Créer un utilisateur
      security: []   # route publique : aucune authentification requise
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
  schemas:
    UserInput:
      type: object
      required: [name, email]
      properties:
        name:
          type: string
          minLength: 2
          maxLength: 100
        email:
          type: string
          format: email

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
              example: EMAIL_ALREADY_USED
            message:
              type: string
              example: Cet email est déjà utilisé.

  responses:
    Error:
      description: Erreur
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
```

Quelques points à remarquer :

- `servers` liste les adresses de l'API pour chaque environnement ;
- `operationId` donne un nom unique à chaque opération (utile pour générer du code) ;
- `security: []` indique explicitement que la route est publique. Les routes protégées déclarent au contraire leur méthode d'authentification (par exemple un token JWT) ;
- `$ref` réutilise un schéma défini une seule fois dans `components`, ce qui évite les répétitions.

Outils utiles :

- **Swagger Editor** (editor.swagger.io) : écrire le fichier avec un aperçu en direct ;
- **Redocly CLI** : vérifier que le fichier est valide, y compris dans la CI :

```bash
npx @redocly/cli lint docs/openapi.yaml
```

Sur l'exemple ci-dessus, Redocly ne signale aucune erreur, seulement deux avertissements sur les adresses `localhost` et `example.com` des `servers` : ils disparaissent une fois les vraies adresses du projet renseignées.

- **swagger-ui-express** : servir une documentation interactive depuis Express (par exemple sur `/api/docs`).

## 2.3 Un format d'erreur unique

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

Codes HTTP à utiliser de façon cohérente :

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

Une erreur `500` ne doit jamais renvoyer de détail technique (pile d'appels, requête SQL) au Frontend.

## 2.4 Une pagination commune

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

## 2.5 Conventions de nommage

À fixer une fois pour toutes dans le contrat :

| Sujet | Convention conseillée |
|---|---|
| Routes | Préfixe `/api`, noms au pluriel : `/api/users`, `/api/users/:id` |
| Champs JSON | `camelCase` : `firstName`, `createdAt` |
| Dates | Format ISO 8601 en UTC : `2026-10-05T14:30:00Z` |
| Identifiants | Toujours le même type (nombre ou chaîne) dans toute l'API |
| Booléens | Préfixe `is` ou `has` : `isActive`, `hasPaid` |

## 2.6 Ce que le contrat évite

Sans contrat écrit, chaque équipe imagine sa propre version de l'API. Les différences n'apparaissent qu'au moment de l'intégration, quand elles coûtent le plus cher à corriger.

Avec un contrat, les désaccords apparaissent **pendant la relecture du contrat**, quand il suffit de modifier quelques lignes de YAML.
