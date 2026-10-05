# 13. Le modèle recommandé

## 13.1 Le modèle complet

Ce schéma rassemble tout le cours, dans l'ordre où les choses se mettent en place :

```text
                 PROBLÈMES RÉCURRENTS
                    (chapitre 2)
                          │
                          ▼
     Conventions · Responsabilités · Règles · Rituels
                   (chapitres 3 à 6)
                          │
                          ▼
                  CONTRAT D'ÉQUIPE
                    (chapitre 7)
                          │
                          ▼
              CONTRAT D'API : OpenAPI
                    (chapitre 8)
                          │
                          ▼
                       TICKETS
                          │
             ┌────────────┴────────────┐
             │                         │
         FRONTEND                   BACKEND
      React + mocks           Express + Swagger
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

## 13.2 Ce qui lie réellement les deux équipes

Le véritable lien entre le Frontend et le Backend n'est donc pas uniquement GitHub. Il repose sur :

```text
              ┌────────────────────┐
              │    PROJET COMMUN   │
              └─────────┬──────────┘
                        │
              ┌─────────▼──────────┐
              │  CONTRAT D'ÉQUIPE  │
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

## 13.3 Checklist de démarrage d'un projet

Avant d'écrire la première ligne de code :

**Le contrat d'équipe**

- [ ] Atelier de lancement réalisé
- [ ] `docs/conventions.md` écrit et validé
- [ ] Tableau des responsabilités rempli, référents désignés
- [ ] Règles et Definition of Done écrites
- [ ] Rituels fixés (point quotidien, point contrat, démonstration, rétrospective)
- [ ] Canaux de communication choisis
- [ ] `docs/contrat-equipe.md` approuvé par les deux référents

**Le contrat d'API**

- [ ] `docs/openapi.yaml` créé avec le format d'erreur et la pagination
- [ ] Lint du contrat (Redocly) sans erreur
- [ ] Documentation Swagger UI accessible en développement

**Le dépôt et les outils**

- [ ] Branches `main` et `develop` protégées
- [ ] Fichier `CODEOWNERS` en place
- [ ] Modèles de pull request et d'issue ajoutés
- [ ] `.env.example` dans le Frontend et le Backend
- [ ] ESLint, Prettier et `.nvmrc` identiques des deux côtés
- [ ] CI qui lance le lint et les tests sur chaque pull request
- [ ] Tableau Kanban (GitHub Project) et labels créés
- [ ] Canal de messagerie d'équipe créé
