# 7. Le contrat d'équipe

## 7.1 Qu'est-ce que le contrat d'équipe

Les chapitres 3 à 6 ont permis de se mettre d'accord sur :

- les **conventions** (chapitre 3) ;
- les **responsabilités** (chapitre 4) ;
- les **règles** et la Definition of Done (chapitre 5) ;
- les **rituels** et les canaux de communication (chapitre 6).

Le **contrat d'équipe** rassemble tous ces accords dans un seul document écrit, validé par les deux équipes. C'est la référence commune : en cas de doute ou de désaccord, on relit le contrat.

Le contrat entre les équipes n'est donc pas d'abord un fichier technique. C'est d'abord un **accord humain et organisationnel**. Le contrat d'API (OpenAPI, chapitre 8) en est la partie technique, écrite dans un format que les outils peuvent vérifier.

```text
                 CONTRAT D'ÉQUIPE
                docs/contrat-equipe.md
                          │
     ┌─────────────┬──────┴───────┬──────────────┐
     │             │              │              │
 Conventions  Responsabilités  Règles + DoD   Rituels
     │
     └──► Conventions d'API ──► CONTRAT D'API
                                docs/openapi.yaml
                                (chapitre 8)
```

## 7.2 Modèle de contrat d'équipe

Fichier `docs/contrat-equipe.md` :

```markdown
# Contrat d'équipe : projet MyApp

Version 1.0, validé le 05/10/2026 par l'équipe Frontend et l'équipe Backend.

## 1. Équipes et rôles
- Équipe Frontend : Awa (référente), Kevin
- Équipe Backend : Dravel (référent), Grâce
- Gardien du contrat d'API : Grâce
- Arbitrage des désaccords : le mentor

## 2. Conventions
Voir docs/conventions.md (code, API, Git, outils de qualité).
Résumé API : préfixe /api, champs en camelCase, dates ISO 8601,
format d'erreur unique, pagination page/limit/total.

## 3. Responsabilités
Tableau Responsable / Consulté / Informé / Partagé (voir chapitre 4 du cours).

## 4. Règles
1. Ne pas modifier le travail de l'autre sans discussion.
2. Le contrat d'API fait foi.
3. Tout changement d'API passe par une pull request validée par les deux équipes.
4. Pull request obligatoire, relue dans la journée.
5. Critiquer le code, pas la personne.
Toute demande passe par un ticket. Tout blocage de plus d'une demi-journée est signalé.

## 5. Definition of Done
(liste complète)

## 6. Rituels
- Point quotidien : 9h00, 15 minutes
- Point contrat d'API : lundi 14h00, 30 minutes
- Démonstration et rétrospective : vendredi de fin d'itération

## 7. Canaux de communication
- Demandes : issues GitHub
- Code : pull requests
- Urgences : groupe WhatsApp de l'équipe
- Toute décision orale est recopiée dans le ticket concerné.

## 8. Environnements
URL locales, de staging et de production.

## 9. Évolution du contrat
Le contrat peut être modifié en rétrospective, par une pull request
approuvée par les deux référents.
```

## 7.3 Construire le contrat : l'atelier de lancement

Le contrat d'équipe se construit **ensemble**, lors d'un atelier au tout début du projet. Il ne doit pas être imposé par une seule équipe.

Déroulé conseillé (1 à 2 heures) :

1. **10 minutes** : chacun cite un problème vécu dans un projet précédent (le chapitre 2 donne des exemples).
2. **20 minutes** : se mettre d'accord sur les conventions (code, API, Git).
3. **15 minutes** : remplir ensemble le tableau des responsabilités.
4. **15 minutes** : choisir les règles et la Definition of Done.
5. **10 minutes** : fixer les rituels (jours, heures, durées) et les canaux de communication.
6. **10 minutes** : relire le document complet et lister les points encore ouverts.

Le document est ensuite proposé dans une pull request et **approuvé par les deux référents** avant le début du développement.

## 7.4 Faire vivre le contrat

- Le contrat est versionné dans le dépôt, comme le code.
- Il est relu en rétrospective : ce qui ne fonctionne pas est modifié.
- Toute modification passe par une pull request approuvée par les deux référents.
- Un nouvel arrivant lit le contrat d'équipe en premier.

## 7.5 Du contrat d'équipe au contrat d'API

Une fois les conventions d'API décidées (nommage, codes HTTP, format d'erreur, pagination), il reste à décrire **chaque route** de façon précise et vérifiable. C'est le rôle du contrat d'API, écrit au format OpenAPI : c'est l'objet du chapitre suivant.
