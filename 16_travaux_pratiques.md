# 16. Travaux pratiques

Les TP se font **en binôme** : un apprenant Frontend, un apprenant Backend. Les rôles sont inversés à chaque nouvelle fonctionnalité.

Projet support : une petite application de gestion d'utilisateurs (liste, création, modification, suppression).

Les TP suivent l'ordre du cours : d'abord l'accord entre les équipes, ensuite la technique.

## TP 1 : identifier les problèmes récurrents

Objectif : partir de l'expérience réelle.

1. Chaque apprenant raconte un problème vécu dans un projet de groupe précédent.
2. Le binôme classe ces problèmes dans le tableau de synthèse du chapitre 2.
3. Pour chacun, il note ce qui a manqué (convention, responsabilité, règle, rituel).

Vérification : le binôme sait expliquer, pour chaque problème, quel chapitre du cours y répond.

## TP 2 : atelier de lancement et contrat d'équipe

Objectif : écrire le contrat d'équipe avant tout code.

1. Suivre le déroulé de l'atelier du chapitre 7 (1 heure).
2. Rédiger `docs/conventions.md` : conventions techniques, d'API et Git (chapitre 3).
3. Remplir le tableau des responsabilités et désigner les référents (chapitre 4).
4. Choisir les règles et écrire la Definition of Done (chapitre 5).
5. Fixer les rituels et les canaux de communication (chapitre 6).
6. Rassembler le tout dans `docs/contrat-equipe.md` en suivant le modèle du chapitre 7.

Vérification : le mentor relit le contrat d'équipe ; chaque section est remplie et chaque apprenant peut l'expliquer.

## TP 3 : mettre en place le dépôt

Objectif : un dépôt qui applique le contrat d'équipe.

1. Créer un monorepo avec `frontend/` (Vite + React), `backend/` (Express) et `docs/`.
2. Créer la branche `develop` et protéger `main` et `develop` (chapitre 10).
3. Ajouter `CODEOWNERS` avec les référents, le modèle de pull request et `CONTRIBUTING.md`.
4. Ajouter les fichiers `.env.example` et `.nvmrc`.
5. Proposer le contrat d'équipe dans une pull request, approuvée par les deux apprenants.
6. Créer un GitHub Project avec les colonnes du chapitre 11 et les labels.

Vérification : une tentative de push direct sur `develop` est refusée par GitHub.

## TP 4 : écrire le contrat d'API et installer Swagger

Objectif : un contrat OpenAPI validé, documenté et appliqué.

1. Écrire ensemble `docs/openapi.yaml` pour les routes :
   - `GET /api/users` (paginée) ;
   - `GET /api/users/:id` ;
   - `POST /api/users` ;
   - `PATCH /api/users/:id` ;
   - `DELETE /api/users/:id`.
2. Respecter les conventions d'API du contrat d'équipe (format d'erreur, pagination, nommage).
3. Suivre le chapitre 14 : `redocly.yaml`, script `lint:api`, Swagger UI sur `/api/docs`.
4. Ouvrir la pull request du contrat : chacun relit et approuve celle de l'autre.

Vérification : `npm run lint:api` affiche « Your API description is valid » et toutes les routes apparaissent dans `http://localhost:3000/api/docs`.

## TP 5 : travailler en parallèle

Objectif : avancer sans s'attendre.

- **Apprenant Frontend** : démarrer le mock Prism (chapitre 14 section 14.7), puis construire la liste et le formulaire de création, avec la gestion des erreurs `400` et `409` et l'état de chargement.
- **Apprenant Backend** : ajouter `express-openapi-validator` (chapitre 14 section 14.6), puis implémenter les routes conformément au contrat, avec des tests (`supertest`).

Chacun crée un ticket par tâche et une branche `feature/front-...` ou `feature/back-...`.

Vérification : le Frontend fonctionne entièrement avec le mock, le Backend passe ses tests, sans qu'aucun des deux n'ait attendu l'autre.

## TP 6 : relecture croisée

Objectif : pratiquer une relecture de code bienveillante.

1. Chaque apprenant relit la pull request de l'autre.
2. Le relecteur laisse au moins une question, une suggestion et un point positif (chapitre 6 section 6.5).
3. L'auteur répond à chaque remarque.

Vérification : chaque pull request contient une discussion constructive avant la fusion.

## TP 7 : intégration

Objectif : remplacer le mock par l'API réelle.

1. Arrêter le mock et lancer le Frontend sans `API_PROXY_TARGET`.
2. Lancer le Backend et le Frontend ensemble.
3. Noter chaque écart entre le contrat et la réalité, ouvrir un ticket pour chacun, et les corriger.

Vérification : toutes les fonctionnalités marchent avec l'API réelle. Moins il y a eu d'écarts, mieux le contrat a été respecté.

## TP 8 : gérer un changement cassant

Objectif : modifier l'API sans casser le Frontend.

1. Le mentor demande de renommer `name` en `fullName`.
2. Appliquer la procédure du chapitre 12 : ticket, pull request sur le contrat, période de transition avec les deux champs, migration du Frontend, suppression de l'ancien champ.

Vérification : à aucune étape l'application n'est cassée sur `develop`.

## TP 9 : réécrire un mauvais ticket

Objectif : savoir rédiger une demande claire.

Réécrire ce ticket selon le modèle du chapitre 11 :

```text
Faire l'API des utilisateurs, comme d'habitude, merci.
```

Vérification : le ticket réécrit contient la route, les données, les réponses, les erreurs et des critères d'acceptation.

## TP 10 : démonstration et rétrospective

Objectif : clore une itération comme une vraie équipe.

1. Faire la démonstration des fonctionnalités terminées au mentor, en vérifiant chaque point de la Definition of Done.
2. Animer une rétrospective de 30 minutes : ce qui a bien marché, ce qui a posé problème, deux actions concrètes pour la suite.
3. Mettre à jour le contrat d'équipe si une règle doit changer (pull request approuvée par les deux référents).

Vérification : chaque ticket présenté respecte la Definition of Done du chapitre 5.
