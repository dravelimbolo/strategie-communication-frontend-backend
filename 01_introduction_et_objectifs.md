# 1. Introduction et objectifs

## 1.1 Le problème

Quand une équipe **Frontend** et une équipe **Backend** travaillent sur la même application, les conflits viennent rarement du code lui-même. Ils viennent presque toujours d'un manque de communication :

- le Frontend attend une route qui n'existe pas encore ;
- le Backend renvoie `userName` alors que le Frontend attend `name` ;
- une route change sans prévenir et l'interface casse ;
- personne ne sait qui doit gérer une erreur ou une validation ;
- une fonctionnalité est « terminée » pour l'un, mais pas pour l'autre ;
- les désaccords techniques deviennent des désaccords entre personnes.

## 1.2 L'idée centrale

Pour travailler ensemble sans conflit, les deux équipes ne doivent pas fonctionner comme deux équipes séparées, mais autour d'un **contrat commun** : l'API.

```text
                 CONTRAT D'API
                       │
        ┌──────────────┴──────────────┐
        │                             │
     BACKEND                       FRONTEND
        │                             │
  Express / Node.js              React (Vite)
        │                             │
  ┌─────▼─────┐                 ┌─────▼─────┐
  │ API REST  │◄── HTTP/JSON ───│  Client   │
  └─────┬─────┘                 │  (fetch)  │
        │                       └─────┬─────┘
        ▼                             ▼
  Base de données                 Interface
```

Autour de ce contrat, quatre outils complètent la collaboration :

1. **Git et GitHub** : branches, pull requests, relecture du code ;
2. **les tickets** : chaque besoin est écrit et suivi ;
3. **la communication** : des échanges courts et réguliers, des décisions écrites ;
4. **une définition commune de « terminé »**.

## 1.3 Objectifs d'apprentissage

À la fin du cours, l'apprenant sait :

- écrire et faire valider un contrat d'API avant de coder ;
- travailler sur le Frontend sans attendre le Backend, grâce aux mocks ;
- organiser les branches Git et les pull requests d'une équipe Frontend + Backend ;
- définir des conventions communes et les responsabilités de chacun ;
- rédiger un ticket clair ;
- gérer un changement d'API sans casser l'autre équipe ;
- appliquer une Definition of Done partagée ;
- donner et recevoir une critique de code de façon professionnelle.

## 1.4 Prérequis

- bases de JavaScript, React et Express ;
- savoir ce qu'est une requête HTTP (méthode, URL, code de réponse, JSON) ;
- utilisation de Git (branche, commit, push, pull request) ;
- un compte GitHub.

## 1.5 Lien avec le cours de déploiement

Ce cours complète le cours [Stratégies de déploiement d'une application React + Express](https://github.com/dravelimbolo/strategies-deploiement-react-express). Le premier explique **comment livrer** l'application, celui-ci explique **comment construire ensemble** ce qui sera livré. Les deux utilisent les mêmes conventions : branches `main` et `develop`, pull requests obligatoires, CI avant toute fusion.
