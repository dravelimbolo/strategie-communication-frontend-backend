# 12. Gérer les changements d'API

## 12.1 Pourquoi c'est sensible

Un changement de route, de structure JSON, de code HTTP ou d'authentification peut casser le Frontend, parfois sans que le Backend ne s'en aperçoive. C'est le problème « l'API change sans prévenir » du chapitre 2, et la règle 3 du contrat d'équipe (chapitre 5) y répond.

## 12.2 Changement compatible ou cassant

| Changement | Compatible | Cassant |
|---|---|---|
| Ajouter une nouvelle route | Oui | |
| Ajouter un champ dans une réponse | Oui | |
| Ajouter un paramètre facultatif | Oui | |
| Supprimer ou renommer un champ de réponse | | Oui |
| Changer le type d'un champ (`"15"` au lieu de `15`) | | Oui |
| Rendre obligatoire un champ qui était facultatif dans une requête | | Oui |
| Supprimer ou renommer une route | | Oui |
| Changer un code HTTP ou le format des erreurs | | Oui |
| Changer la méthode d'authentification | | Oui |

Un changement **compatible** peut être fait à tout moment, en mettant à jour le contrat.

Un changement **cassant** suit toujours une procédure.

## 12.3 Procédure pour un changement cassant

```text
1. Ouvrir un ticket avec le label contrat-api
        ↓
2. Proposer la modification du contrat dans une pull request
        ↓
3. Validation par les deux équipes (CODEOWNERS)
        ↓
4. Période de transition : ancienne et nouvelle version disponibles
        ↓
5. Le Frontend migre vers la nouvelle version
        ↓
6. Suppression de l'ancienne version
```

Exemple : renommer `name` en `fullName`.

1. Le Backend renvoie **les deux** champs pendant la transition, et le contrat marque l'ancien champ comme obsolète (`deprecated: true`) :

```json
{
  "id": 15,
  "name": "Dravel",
  "fullName": "Dravel"
}
```

2. Le Frontend passe à `fullName`.
3. Une fois le Frontend déployé en production, le Backend supprime `name`.

À aucun moment l'application n'est cassée.

## 12.4 Versionner l'API

Pour un changement important touchant beaucoup de routes, on crée une nouvelle version :

```text
Frontend v1  →  /api/v1/users
Frontend v2  →  /api/v2/users   (la v1 reste disponible pendant la migration)
```

On annonce une date de fin pour l'ancienne version, et on s'y tient.

## 12.5 Côté Frontend : être tolérant

Le Frontend ignore les champs qu'il ne connaît pas et ne plante pas si un champ facultatif est absent. Ainsi, l'ajout d'un champ par le Backend ne le casse jamais.

## 12.6 Vérifier automatiquement

Pour aller plus loin, la CI peut comparer le contrat de la pull request à celui de `develop` et signaler automatiquement les changements cassants, avec des outils comme `oasdiff`. Le validateur présenté au chapitre 14 vérifie aussi, à chaque requête, que l'API réelle respecte le contrat.
