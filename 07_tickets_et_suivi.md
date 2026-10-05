# 7. Les tickets comme outil de collaboration

## 7.1 Écrire plutôt que reprocher

Au lieu de dire :

> « Le Backend n'a pas fait ce qu'on avait demandé. »

On crée un ticket qui décrit précisément le besoin. Si le résultat ne correspond pas, on compare au ticket : la discussion porte sur un document, pas sur une personne.

Une règle simple : **une demande qui n'est pas écrite dans un ticket n'existe pas**.

## 7.2 Un bon ticket

Exemple de ticket (issue GitHub) :

```text
#142 : Ajouter la route de création d'utilisateur

Équipe : Backend
Lié à : #140 (formulaire d'inscription, Frontend)

Route
POST /api/users

Authentification
Aucune (route publique d'inscription)

Corps de la requête
{
  "name": "Dravel",
  "email": "dravel@example.com"
}

Règles de validation
- name : obligatoire, 2 à 100 caractères
- email : obligatoire, format email valide, unique

Réponse 201
{
  "id": 15,
  "name": "Dravel",
  "email": "dravel@example.com"
}

Erreurs (format d'erreur commun)
400 VALIDATION_ERROR    : données invalides
409 EMAIL_ALREADY_USED  : email déjà utilisé
500 INTERNAL_ERROR      : erreur serveur

Critères d'acceptation
- [ ] Route conforme au contrat docs/openapi.yaml
- [ ] Tests des cas 201, 400 et 409
- [ ] Déployée en staging
```

Le Frontend sait exactement ce qu'il doit utiliser, et le Backend sait exactement ce qu'il doit livrer.

Un bon ticket contient toujours :

- un titre clair, qui commence par un verbe ;
- l'équipe concernée ;
- les liens vers les tickets liés ;
- la description précise du besoin (ou le lien vers la partie du contrat) ;
- des **critères d'acceptation** vérifiables.

## 7.3 Modèles d'issues

GitHub permet de proposer des modèles dans `.github/ISSUE_TEMPLATE/` : un modèle « Nouvelle route d'API », un modèle « Bug », un modèle « Fonctionnalité Frontend ». Chaque ticket est alors créé avec les bonnes rubriques déjà présentes.

## 7.4 Labels

Des labels communs permettent de filtrer les tickets :

| Label | Usage |
|---|---|
| `frontend` | Concerne l'équipe Frontend |
| `backend` | Concerne l'équipe Backend |
| `contrat-api` | Modifie le contrat : les deux équipes doivent valider |
| `bug` | Comportement incorrect |
| `bloquant` | Empêche une autre équipe d'avancer |

## 7.5 Un tableau Kanban partagé

Un **GitHub Project** affiche tous les tickets des deux équipes sur un même tableau :

```text
┌──────────┬──────────┬──────────┬──────────┬──────────┐
│ À faire  │ En cours │ En revue │ Staging  │ Terminé  │
├──────────┼──────────┼──────────┼──────────┼──────────┤
│ #145     │ #142     │ #140     │ #138     │ #130     │
│ #146     │ #143     │          │          │ #131     │
└──────────┴──────────┴──────────┴──────────┴──────────┘
```

- Chacun voit en temps réel où en est l'autre équipe.
- Un ticket n'arrive dans « Terminé » que s'il respecte la Definition of Done (chapitre 10).
- Une pull request mentionne son ticket (`Closes #142`) : le ticket se ferme automatiquement à la fusion.
