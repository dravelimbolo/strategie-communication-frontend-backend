# 1. Introduction et définitions

## 1.1 De quoi parle ce cours

Ce cours explique comment une équipe **Frontend** et une équipe **Backend** peuvent travailler ensemble sur la même application sans conflit.

La démarche part de l'humain pour aller vers la technique :

```text
Comprendre les problèmes récurrents
              ↓
Se mettre d'accord : conventions, responsabilités, règles, rituels
              ↓
Écrire ces accords : le contrat d'équipe
              ↓
Formaliser la partie technique : le contrat d'API (OpenAPI)
              ↓
Outiller la collaboration : mocks, Git, tickets
```

On ne commence pas par l'outil. Un fichier OpenAPI ne sert à rien si les deux équipes ne se sont pas d'abord mises d'accord sur leur façon de travailler.

## 1.2 Définitions

**Frontend** : la partie de l'application qui s'exécute dans le navigateur et que l'utilisateur voit. Dans ce cours : React, construit avec Vite.

**Backend** : la partie qui s'exécute sur le serveur. Elle applique les règles métier, accède à la base de données et protège les données. Dans ce cours : Node.js et Express.

**API** (Application Programming Interface) : l'ensemble des routes que le Backend met à disposition du Frontend, par exemple `GET /api/users`. C'est le point de rencontre entre les deux équipes.

**Collaboration** : la capacité de deux équipes à avancer vers un objectif commun sans se bloquer, ni se contredire, ni défaire le travail de l'autre.

**Communication** : l'ensemble des échanges qui rendent la collaboration possible : réunions, tickets, messages, pull requests, documentation.

**Convention** : une règle de forme décidée ensemble pour que tout le monde fasse pareil, par exemple « les champs JSON sont en `camelCase` ».

**Contrat d'équipe** : le document écrit qui rassemble tous les accords entre les équipes : conventions, responsabilités, règles, rituels.

**Contrat d'API** : la partie technique du contrat d'équipe. Il décrit précisément les routes, les données échangées, les réponses et les erreurs. On l'écrit au format **OpenAPI**.

## 1.3 Le schéma de base

```text
                 CONTRAT D'ÉQUIPE
           (conventions, rôles, règles, rituels)
                         │
                   CONTRAT D'API
                      (OpenAPI)
                         │
        ┌────────────────┴────────────────┐
        │                                 │
     BACKEND                           FRONTEND
        │                                 │
  Express / Node.js                  React (Vite)
        │                                 │
  ┌─────▼─────┐                     ┌─────▼─────┐
  │ API REST  │◄──── HTTP/JSON ─────│  Client   │
  └─────┬─────┘                     │  (fetch)  │
        │                           └─────┬─────┘
        ▼                                 ▼
  Base de données                     Interface
```

## 1.4 Objectifs d'apprentissage

À la fin du cours, l'apprenant sait :

- reconnaître les problèmes récurrents entre une équipe Frontend et une équipe Backend ;
- définir des conventions communes, des responsabilités claires, des règles et des rituels ;
- les rassembler dans un contrat d'équipe écrit et validé ;
- écrire un contrat d'API au format OpenAPI ;
- installer et configurer Swagger pour documenter et valider l'API ;
- travailler sur le Frontend sans attendre le Backend, grâce aux mocks ;
- organiser Git, les pull requests et les tickets d'une équipe ;
- faire évoluer une API sans casser le travail de l'autre équipe.

## 1.5 Prérequis

- bases de JavaScript, React et Express ;
- savoir ce qu'est une requête HTTP (méthode, URL, code de réponse, JSON) ;
- utilisation de Git (branche, commit, push, pull request) ;
- un compte GitHub.

## 1.6 Lien avec le cours de déploiement

Ce cours complète le cours [Stratégies de déploiement d'une application React + Express](https://github.com/dravelimbolo/strategies-deploiement-react-express). Le premier explique **comment livrer** l'application, celui-ci explique **comment la construire ensemble**. Les deux utilisent les mêmes conventions : branches `main` et `develop`, pull requests obligatoires, CI avant toute fusion.
