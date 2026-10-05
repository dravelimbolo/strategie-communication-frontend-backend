# 6. Rituels et communication

Les rituels sont des moments d'échange réguliers et prévus à l'avance. Ils répondent aux problèmes « les blocages sont signalés trop tard » et « les demandes orales se perdent » (chapitre 2).

## 6.1 Des réunions courtes mais régulières

Pas besoin de réunions interminables. Une petite synchronisation suffit souvent :

**Frontend :**

> J'ai besoin de `GET /api/users`.

**Backend :**

> Disponible demain, réponse paginée.

**Frontend :**

> D'accord. Quelle structure ?

**Backend :**

> `data`, `page`, `limit`, `total`, comme dans nos conventions. Je l'ajoute au contrat ce soir.

**Frontend :**

> Parfait, je travaille dessus avec le mock en attendant.

Cela permet de détecter les problèmes **avant** l'intégration.

## 6.2 Les rituels recommandés

| Rituel | Fréquence | Durée | Contenu |
|---|---|---|---|
| Atelier de lancement | Une fois, au début du projet | 1 à 2 heures | Construire le contrat d'équipe (chapitre 7) |
| Point quotidien (daily) | Chaque jour | 15 minutes maximum | Ce que j'ai fait, ce que je vais faire, ce qui me bloque |
| Point contrat d'API | Une fois par semaine, ou à chaque nouvelle fonctionnalité | 30 minutes | Relire ensemble les routes à venir et les changements du contrat |
| Démonstration | À la fin de chaque itération | 30 minutes | Montrer les fonctionnalités terminées, sur l'environnement de staging |
| Rétrospective | À la fin de chaque itération | 30 à 45 minutes | Ce qui a bien marché, ce qui doit changer, une ou deux actions concrètes |

Pendant le point quotidien, on ne résout pas les problèmes : on les signale, puis les personnes concernées en discutent juste après.

## 6.3 Le bon canal pour chaque message

| Situation | Canal | Pourquoi |
|---|---|---|
| Nouvelle demande ou nouveau besoin | Ticket (issue GitHub) | Doit être suivi et retrouvable |
| Question ou remarque sur du code | Commentaire dans la pull request | Reste attaché au code concerné |
| Changement du contrat d'API | Pull request sur `docs/openapi.yaml` + message à l'autre équipe | Les deux équipes doivent valider |
| Question rapide, blocage urgent | Messagerie d'équipe (WhatsApp, Discord, Slack) | Réponse rapide |
| Sujet complexe ou désaccord | Court appel ou réunion | Plus rapide qu'une longue discussion écrite |

Règle d'or : **une décision prise à l'oral ou dans la messagerie est recopiée dans le ticket ou la pull request concernés**. Une décision qui n'est pas écrite à un endroit durable sera oubliée ou contestée.

Pour les décisions techniques importantes (choix d'une librairie, d'une méthode d'authentification), on peut tenir un court journal de décisions dans `docs/decisions/` : un fichier par décision, avec le contexte, la décision et ses conséquences. On appelle ce format un ADR (Architecture Decision Record).

## 6.4 Signaler un blocage tôt

Un blocage signalé le premier jour coûte une heure. Un blocage signalé la veille de la livraison coûte la livraison.

- Signaler dès qu'on est bloqué depuis plus d'une demi-journée.
- Décrire le blocage précisément : ce qui est attendu, ce qui se passe, ce qui a déjà été essayé.
- Ajouter le label `bloquant` sur le ticket.

## 6.5 Une relecture de code bienveillante

La relecture de code (code review) sert à améliorer le code et à partager les connaissances, pas à juger la personne.

Pour le relecteur :

- commenter le code, jamais la personne : « cette fonction pourrait être découpée » plutôt que « tu as mal écrit ça » ;
- poser des questions plutôt qu'affirmer : « que se passe-t-il si la liste est vide ? » ;
- distinguer l'obligatoire du facultatif, par exemple en préfixant les remarques mineures par « suggestion : » ;
- relever aussi ce qui est bien fait ;
- répondre aux pull requests rapidement : une pull request qui attend bloque son auteur.

Pour l'auteur :

- garder les pull requests petites : elles sont relues plus vite et mieux ;
- expliquer le contexte dans la description ;
- répondre à chaque remarque, en appliquant la correction ou en expliquant pourquoi on ne l'applique pas ;
- ne pas prendre les remarques personnellement.

## 6.6 Gérer un désaccord

Les désaccords techniques sont normaux et utiles. Pour qu'ils restent professionnels :

1. s'appuyer sur des faits : le contrat, le ticket, une mesure, une documentation officielle ;
2. proposer une solution, pas seulement critiquer ;
3. si le désaccord persiste, fixer un court délai, puis demander l'arbitrage du référent ou du mentor ;
4. une fois la décision prise, l'écrire et l'appliquer, même si ce n'était pas son choix.

Le but est d'améliorer le projet, pas de chercher un coupable.
