# 11. Le modèle recommandé et les règles d'équipe

## 11.1 Le modèle pour une équipe React + Express + GitHub + CI/CD

```text
                     BESOIN PRODUIT
                           │
                           ▼
                        TICKETS
                           │
                           ▼
                     CONTRAT D'API
                        OpenAPI
                           │
              ┌────────────┴────────────┐
              │                         │
          FRONTEND                   BACKEND
       React + mocks                 Express
              │                         │
              └────────────┬────────────┘
                           │
                     Pull Requests
                           │
              Relecture de code  +  CI
                           │
                        develop
                           │
                  Staging : intégration
                     et validation
                           │
                         main
                           │
                       Production
```

## 11.2 Les 5 règles d'équipe

### 1. Ne pas modifier le travail de l'autre sans discussion

Chaque équipe conserve son périmètre (rendu concret par le fichier `CODEOWNERS`), et les changements ayant un impact sur l'autre équipe sont communiqués avant d'être faits.

### 2. L'API est un contrat entre le Frontend et le Backend

Les deux équipes se mettent d'accord sur les données et les comportements avant de coder, et le contrat écrit fait foi.

### 3. Tout changement d'API est communiqué et documenté

Un changement de route, de structure JSON, de code HTTP ou d'authentification peut casser le Frontend. Il passe par une pull request sur le contrat, validée par les deux équipes (chapitre 9).

### 4. Les pull requests sont obligatoires

Le code est relu avant d'être intégré, afin de limiter les erreurs et de partager les connaissances.

### 5. Critiquer le code et le processus, pas la personne

Les désaccords techniques restent professionnels. Le but est d'améliorer le projet, pas de chercher un responsable.

## 11.3 Ce qui lie réellement les deux équipes

Le véritable lien entre le Frontend et le Backend n'est donc pas uniquement GitHub. Il repose sur :

```text
              ┌────────────────────┐
              │    PROJET COMMUN   │
              └─────────┬──────────┘
                        │
       ┌────────────────┼────────────────┐
       │                │                │
       ▼                ▼                ▼
  CONTRAT D'API      TICKETS       COMMUNICATION
       │                │                │
       └────────────────┼────────────────┘
                        │
                        ▼
                       GIT
                        │
                        ▼
               RELECTURE DE CODE
                        │
                        ▼
                      CI/CD
                        │
                        ▼
              TESTS D'INTÉGRATION
                        │
                        ▼
                 FONCTIONNALITÉ
                    TERMINÉE
```

## 11.4 Checklist de démarrage d'un projet

Avant d'écrire la première ligne de code :

- [ ] Dépôt créé, branches `main` et `develop` protégées
- [ ] Fichier `CODEOWNERS` en place
- [ ] Modèles de pull request et d'issue ajoutés
- [ ] `docs/conventions.md` écrit et validé par les deux équipes
- [ ] `docs/openapi.yaml` créé avec le format d'erreur et la pagination
- [ ] `.env.example` dans le Frontend et le Backend
- [ ] ESLint, Prettier et `.nvmrc` identiques des deux côtés
- [ ] CI qui lance le lint et les tests sur chaque pull request
- [ ] Tableau Kanban (GitHub Project) et labels créés
- [ ] Definition of Done écrite
- [ ] Rituels fixés (point quotidien, point contrat, démonstration, rétrospective)
- [ ] Canal de messagerie d'équipe créé
