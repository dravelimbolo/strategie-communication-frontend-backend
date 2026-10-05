# 8. Le contrat d'API avec OpenAPI

## 8.1 Du contrat d'équipe au contrat d'API

Le contrat d'équipe (chapitre 7) fixe les conventions de l'API : nommage, codes HTTP, format d'erreur, pagination. Il reste à décrire **chaque route** de façon précise.

Le contrat d'API doit préciser, pour chaque route :

- la méthode et le chemin : `POST /api/users` ;
- les paramètres (chemin, requête, en-têtes) ;
- le format des données envoyées ;
- le format des réponses ;
- les codes HTTP possibles ;
- les erreurs ;
- l'authentification ;
- les règles de validation.

Le contrat est écrit et validé par les **deux** équipes **avant** de coder la route. Il n'appartient ni au Frontend ni au Backend : c'est un document commun.

## 8.2 OpenAPI et Swagger

- **OpenAPI** est le **format** standard pour décrire une API REST, dans un fichier YAML ou JSON. La version actuelle est OpenAPI 3.1.
- **Swagger** est le nom historique de ce format, et aujourd'hui le nom d'une **famille d'outils** qui l'utilisent : Swagger Editor (écrire le fichier), Swagger UI (afficher une documentation interactive).

Pourquoi un format standard plutôt qu'un simple document Word ou un message ?

- il est **précis** : un champ est obligatoire ou non, un nombre a un minimum, un email a un format ;
- il est **vérifiable** par des outils : on peut détecter une erreur dans le contrat, et même vérifier que l'API respecte le contrat ;
- il est **exploitable** : il génère une documentation interactive, un faux serveur (mock), des tests.

Le contrat est un fichier versionné dans le dépôt : `docs/openapi.yaml`.

## 8.3 Écrire le contrat avant le code

Ce cours applique l'approche **design-first** (le contrat d'abord) :

```text
Besoin (ticket)
     ↓
Le contrat est écrit ou modifié (docs/openapi.yaml)
     ↓
Pull request relue par les deux équipes
     ↓
Le Backend code la route, le Frontend code l'écran avec un mock
```

L'approche inverse, **code-first**, génère le contrat à partir du code déjà écrit. Elle est plus rapide pour le Backend, mais le Frontend découvre l'API une fois qu'elle est faite : c'est justement le problème « le Frontend attend le Backend » du chapitre 2.

## 8.4 Un premier contrat

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

## 8.5 Lire le contrat

| Partie | Rôle |
|---|---|
| `openapi` | Version du format OpenAPI utilisée |
| `info` | Nom, version et licence de l'API |
| `servers` | Adresses de l'API pour chaque environnement |
| `paths` | Les routes : chaque chemin, puis chaque méthode HTTP |
| `operationId` | Nom unique de l'opération (utile pour générer du code) |
| `security: []` | La route est publique. Les routes protégées déclarent au contraire leur méthode d'authentification |
| `requestBody` | Les données envoyées par le Frontend |
| `responses` | Les réponses possibles, une par code HTTP |
| `components` | Les éléments réutilisables : schémas de données, réponses, paramètres |
| `$ref` | Une référence vers un élément de `components`, pour ne pas répéter les définitions |

On retrouve dans ce fichier les conventions du chapitre 3 : préfixe `/api`, champs en `camelCase`, code `201` pour une création, `409` pour un conflit, format d'erreur unique.

## 8.6 Ce que le contrat apporte à chaque équipe

| Frontend | Backend |
|---|---|
| Connaît exactement les données à envoyer et à recevoir | Sait exactement ce qu'il doit livrer |
| Peut travailler avec un mock généré depuis le contrat (chapitre 9) | Peut valider automatiquement les requêtes avec le contrat (chapitre 14) |
| Peut générer ses types de données (TypeScript) | Publie une documentation interactive avec Swagger UI (chapitre 14) |

Sans contrat écrit, chaque équipe imagine sa propre version de l'API, et les différences n'apparaissent qu'à l'intégration. Avec un contrat, les désaccords apparaissent **pendant la relecture du contrat**, quand il suffit de modifier quelques lignes de YAML.

## 8.7 Outils

| Outil | Usage |
|---|---|
| Swagger Editor | Écrire le contrat avec un aperçu en direct |
| Redocly CLI | Vérifier que le contrat est valide, en local et dans la CI |
| Swagger UI | Documentation interactive servie par Express |
| express-openapi-validator | Refuser automatiquement les requêtes qui ne respectent pas le contrat |
| Prism | Faux serveur (mock) généré à partir du contrat |

L'installation et la configuration de chacun de ces outils sont détaillées pas à pas dans le **chapitre 14 (bonus)**.
