# 2. Les problèmes récurrents

Avant de chercher des solutions, il faut nommer les problèmes. Ceux de ce chapitre se retrouvent dans presque tous les projets où un Frontend et un Backend sont développés par des personnes différentes.

## 2.1 Le Frontend attend le Backend

```text
Le Backend développe la route
       ↓
Le Frontend attend, ou code « à l'aveugle »
       ↓
Intégration tardive
       ↓
Problèmes découverts à la fin
       ↓
Corrections dans l'urgence
```

**Cause** : le travail est organisé en séquence au lieu d'être parallèle.
**Conséquence** : du temps perdu et des problèmes découverts au pire moment.

## 2.2 Les données ne correspondent pas

Le Backend renvoie :

```json
{ "user_name": "Dravel", "created": "05/10/2026" }
```

Le Frontend attendait :

```json
{ "name": "Dravel", "createdAt": "2026-10-05T14:30:00Z" }
```

**Cause** : aucune convention commune sur le nommage et les formats.
**Conséquence** : l'interface affiche `undefined` ou plante à l'intégration.

## 2.3 L'API change sans prévenir

Un développeur Backend renomme une route ou un champ « pour faire plus propre ». Le lendemain, l'écran du Frontend ne fonctionne plus, et personne ne sait pourquoi.

**Cause** : pas de règle sur les changements d'API.
**Conséquence** : des bugs en staging, voire en production, et une perte de confiance entre les équipes.

## 2.4 Personne ne sait qui est responsable

- « Le formulaire vérifie déjà l'email, le Backend n'a pas besoin de le faire. »
- « C'est au Backend de renvoyer un message d'erreur lisible. »
- « Ah, je pensais que c'était toi qui configurais CORS. »

**Cause** : des responsabilités jamais définies.
**Conséquence** : des tâches faites deux fois, ou pas du tout, et parfois une faille de sécurité.

## 2.5 « Terminé » ne veut pas dire la même chose

Pour le Backend, la route est terminée parce qu'elle répond. Pour le Frontend, la fonctionnalité n'est pas terminée tant que l'écran ne l'utilise pas. Pour l'utilisateur, rien n'est disponible.

**Cause** : pas de définition commune de « terminé ».
**Conséquence** : des fonctionnalités annoncées mais inutilisables.

## 2.6 « Ça marche chez moi »

Le Frontend appelle `localhost:3000` en dur, le Backend a ajouté une variable d'environnement sans le dire, les deux n'utilisent pas la même version de Node, et le navigateur bloque les appels à cause de CORS.

**Cause** : des environnements et une configuration non partagés.
**Conséquence** : des heures perdues à chercher des problèmes qui ne viennent pas du code.

## 2.7 Le code de l'autre est modifié

Un développeur Frontend corrige « rapidement » un fichier du Backend, ou fusionne directement sur `develop` sans relecture. Le Backend découvre la modification après coup.

**Cause** : pas de règles Git ni de protection des branches.
**Conséquence** : des régressions et un sentiment de dépossession.

## 2.8 Les demandes orales se perdent

« Je te l'avais dit en réunion. » « Je l'ai écrit sur WhatsApp la semaine dernière. » Personne ne retrouve la décision, et chacun s'en souvient différemment.

**Cause** : les décisions ne sont pas écrites à un endroit durable.
**Conséquence** : des malentendus et des tâches oubliées.

## 2.9 Les blocages sont signalés trop tard

Un développeur reste bloqué trois jours sans rien dire, puis annonce la veille de la livraison qu'il ne pourra pas finir.

**Cause** : pas de rituel de synchronisation régulier.
**Conséquence** : des livraisons manquées.

## 2.10 Les désaccords deviennent personnels

Une remarque sèche dans une pull request, une critique en réunion devant tout le monde, et un désaccord technique devient un conflit entre personnes.

**Cause** : pas de règles de communication.
**Conséquence** : une ambiance dégradée et des équipes qui ne se parlent plus.

## 2.11 Synthèse : chaque problème a sa réponse

| Problème | Ce qui manque | Réponse | Chapitre |
|---|---|---|---|
| Les données ne correspondent pas | Des formats communs | Conventions communes | 3 |
| « Ça marche chez moi » | Une configuration partagée | Conventions communes | 3 |
| Personne n'est responsable | Des rôles clairs | Rôles et responsabilités | 4 |
| Le code de l'autre est modifié | Des règles de travail | Règles d'équipe | 5 |
| « Terminé » diffère | Une définition commune | Definition of Done | 5 |
| Les désaccords deviennent personnels | Des règles de communication | Règles d'équipe | 5 |
| Les blocages arrivent trop tard | Des points réguliers | Rituels | 6 |
| Les demandes orales se perdent | Des décisions écrites | Rituels et communication | 6 |
| Tous les accords précédents | Un document de référence | Contrat d'équipe | 7 |
| L'API change sans prévenir | Un contrat technique | Contrat d'API OpenAPI | 8 et 12 |
| Le Frontend attend le Backend | Du travail en parallèle | Mocks | 9 |

La cause profonde est presque toujours la même : **l'absence d'accord écrit entre les équipes**. Les chapitres 3 à 6 construisent ces accords, le chapitre 7 les rassemble dans un contrat d'équipe.
