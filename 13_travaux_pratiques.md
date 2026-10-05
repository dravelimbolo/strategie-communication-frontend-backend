# 13. Travaux pratiques

Les TP se font **en binôme** : un apprenant Frontend, un apprenant Backend. Les rôles sont inversés à chaque nouvelle fonctionnalité.

Projet support : une petite application de gestion d'utilisateurs (liste, création, modification, suppression).

## TP 1 : mettre en place le dépôt

Objectif : un dépôt prêt pour travailler à deux.

1. Créer un monorepo avec `frontend/` (Vite + React) et `backend/` (Express).
2. Créer la branche `develop` et protéger `main` et `develop` (chapitre 4 section 4.3).
3. Ajouter `CODEOWNERS`, le modèle de pull request et `CONTRIBUTING.md` avec la Definition of Done.
4. Ajouter `docs/conventions.md` et les fichiers `.env.example`.
5. Créer un GitHub Project avec les colonnes du chapitre 7 et les labels.

Vérification : une tentative de push direct sur `develop` est refusée par GitHub.

## TP 2 : écrire le contrat ensemble

Objectif : un contrat validé par les deux apprenants avant de coder.

1. Écrire ensemble `docs/openapi.yaml` pour les routes :
   - `GET /api/users` (paginée) ;
   - `GET /api/users/:id` ;
   - `POST /api/users` ;
   - `PATCH /api/users/:id` ;
   - `DELETE /api/users/:id`.
2. Utiliser le format d'erreur commun du chapitre 2.
3. Vérifier le fichier avec `npx @redocly/cli lint docs/openapi.yaml`.
4. Ouvrir la pull request du contrat : chacun relit et approuve celle de l'autre.

Vérification : le contrat est fusionné dans `develop` et le lint ne signale aucune erreur.

## TP 3 : travailler en parallèle

Objectif : avancer sans s'attendre.

- **Apprenant Frontend** : démarrer le mock avec Prism ou MSW (chapitre 3), puis construire la liste et le formulaire de création, avec la gestion des erreurs `400` et `409` et l'état de chargement.
- **Apprenant Backend** : implémenter les routes conformément au contrat, avec la validation serveur et les tests (`supertest`).

Chacun crée un ticket par tâche et une branche `feature/front-...` ou `feature/back-...`.

Vérification : le Frontend fonctionne entièrement avec le mock, le Backend passe ses tests, sans qu'aucun des deux n'ait attendu l'autre.

## TP 4 : relecture croisée

Objectif : pratiquer une relecture de code bienveillante.

1. Chaque apprenant relit la pull request de l'autre.
2. Le relecteur laisse au moins une question, une suggestion et un point positif (chapitre 8 section 8.5).
3. L'auteur répond à chaque remarque.

Vérification : chaque pull request contient une discussion constructive avant la fusion.

## TP 5 : intégration

Objectif : remplacer le mock par l'API réelle.

1. Désactiver le mock (`VITE_USE_MOCKS=false`).
2. Lancer le Frontend et le Backend ensemble (avec le proxy Vite du chapitre 5).
3. Noter chaque écart entre le contrat et la réalité, ouvrir un ticket pour chacun, et les corriger.

Vérification : toutes les fonctionnalités marchent avec l'API réelle. Moins il y a eu d'écarts, mieux le contrat a été respecté.

## TP 6 : gérer un changement cassant

Objectif : modifier l'API sans casser le Frontend.

1. Le mentor demande de renommer `name` en `fullName`.
2. Appliquer la procédure du chapitre 9 : ticket, pull request sur le contrat, période de transition avec les deux champs, migration du Frontend, suppression de l'ancien champ.

Vérification : à aucune étape l'application n'est cassée sur `develop`.

## TP 7 : réécrire un mauvais ticket

Objectif : savoir rédiger une demande claire.

Réécrire ce ticket selon le modèle du chapitre 7 :

```text
Faire l'API des utilisateurs, comme d'habitude, merci.
```

Vérification : le ticket réécrit contient la route, les données, les réponses, les erreurs et des critères d'acceptation.

## TP 8 : démonstration et rétrospective

Objectif : clore une itération comme une vraie équipe.

1. Faire la démonstration des fonctionnalités terminées au mentor, en vérifiant chaque point de la Definition of Done.
2. Animer une rétrospective de 30 minutes : ce qui a bien marché, ce qui a posé problème, deux actions concrètes pour la suite.
3. Écrire le résultat dans `docs/retrospectives/`.

Vérification : chaque ticket présenté respecte la Definition of Done du chapitre 10.
