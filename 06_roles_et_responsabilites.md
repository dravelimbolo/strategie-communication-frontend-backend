# 6. Qui fait quoi

## 6.1 Un tableau des responsabilités

Chaque sujet a **un responsable** clairement identifié. Les autres sont consultés ou simplement informés.

Légende :

- **Responsable** : fait le travail et prend la décision finale ;
- **Consulté** : donne son avis avant la décision ;
- **Informé** : est prévenu une fois la décision prise ;
- **Partagé** : les deux équipes travaillent ensemble.

| Sujet | Frontend | Backend |
|---|---|---|
| Interface et UX | Responsable | Informé |
| Composants React | Responsable | Informé |
| Consommation de l'API | Responsable | Consulté |
| Messages d'erreur affichés à l'utilisateur | Responsable | Consulté |
| Validation des formulaires (confort utilisateur) | Responsable | Consulté |
| Mocks | Responsable | Consulté |
| Contrat d'API | Partagé | Partagé |
| Routes de l'API | Consulté | Responsable |
| Validation serveur (sécurité) | Informé | Responsable |
| Format des erreurs de l'API | Consulté | Responsable |
| Base de données | Informé | Responsable |
| Authentification côté serveur | Consulté | Responsable |
| Configuration CORS | Consulté | Responsable |
| Documentation de l'API | Consulté | Responsable |
| Sécurité de l'API | Consulté | Responsable |
| Tests d'intégration | Partagé | Partagé |
| Intégration | Partagé | Partagé |

Le plus important est que **la responsabilité soit claire**, tout en permettant aux deux équipes de discuter des décisions qui ont un impact sur l'autre.

## 6.2 La validation se fait des deux côtés

C'est une confusion fréquente chez les débutants : « le formulaire vérifie déjà l'email, donc le Backend n'a pas besoin de le faire ».

C'est faux. N'importe qui peut appeler l'API directement (avec `curl`, Postman ou les outils du navigateur), sans passer par le formulaire. La validation du Frontend se contourne en quelques secondes.

| | Validation Frontend | Validation Backend |
|---|---|---|
| Rôle | Confort : afficher l'erreur tout de suite, sans aller-retour serveur | Sécurité : refuser toute donnée invalide |
| Contournable | Oui | Non |
| Obligatoire | Recommandée | **Toujours** |

Les deux validations appliquent **les mêmes règles**, celles écrites dans le contrat d'API (longueur minimale, format d'email, champs obligatoires).

## 6.3 Le binôme Frontend + Backend

Pour des académiciens Fullstack, il est souvent plus formateur de faire travailler **un binôme Frontend + Backend sur une même fonctionnalité**, plutôt que de confier tout le Frontend à une équipe et tout le Backend à une autre.

```text
Fonctionnalité « Inscription »
        │
   ┌────┴────┐
   │         │
Apprenant A  Apprenant B
 Frontend     Backend
   │         │
   └────┬────┘
        │
 Même contrat, même ticket,
 même Definition of Done
```

Le binôme apprend à comprendre les contraintes de l'autre et à collaborer autour d'un objectif commun. En changeant les rôles d'une fonctionnalité à l'autre, chacun pratique les deux côtés.
