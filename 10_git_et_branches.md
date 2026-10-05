# 10. Git, branches et pull requests

Ce chapitre met en pratique, dans Git et GitHub, les conventions (chapitre 3) et les règles (chapitre 5) du contrat d'équipe.

## 10.1 Organisation des branches

Les deux équipes suivent une organisation de type **Git Flow** :

```text
main ─────────────────────────────────────────► production
  │
  └── develop ────────────────────────────────► staging
        │
        ├── feature/front-login
        ├── feature/front-dashboard
        │
        ├── feature/back-auth
        ├── feature/back-users
        └── feature/back-payments
```

- `main` : code en production. On n'y pousse jamais directement.
- `develop` : branche d'intégration, déployée sur l'environnement de staging.
- `feature/*` : une branche par fonctionnalité, **créée depuis `develop`**, avec un préfixe `front-` ou `back-` pour savoir tout de suite quelle équipe travaille dessus.
- `fix/*` : correction d'un bug, créée depuis `develop`.
- `hotfix/*` : correction urgente en production, créée depuis `main`, puis fusionnée dans `main` et `develop`.

Chaque développeur travaille sur sa propre branche. Les branches sont courtes : quelques jours au maximum. Plus une branche vit longtemps, plus les conflits de fusion sont nombreux.

## 10.2 Le chemin d'une modification

```text
Développeur
    ↓
Branche feature/*
    ↓
Pull Request vers develop
    ↓
CI (lint, tests, build)   +   Relecture du code (code review)
    ↓
Fusion dans develop
    ↓
Déploiement automatique en staging
    ↓
Tests d'intégration et validation
    ↓
Pull Request develop vers main
    ↓
Production
```

La CI et la relecture se font en parallèle : un relecteur ne perd pas de temps sur une pull request dont la CI est rouge.

Le déploiement en staging et en production est détaillé dans le cours [Stratégies de déploiement](https://github.com/dravelimbolo/strategies-deploiement-react-express) (chapitre 10).

## 10.3 Ce qui protège vraiment le travail des autres

Les branches seules n'empêchent pas un développeur de modifier le code de l'autre équipe : sur sa branche, n'importe qui peut modifier n'importe quel fichier. Trois mécanismes GitHub appliquent réellement la règle « ne pas modifier le travail de l'autre sans discussion ».

### La protection des branches

Dans Settings > Branches (ou Rules), pour `main` et `develop` :

- pull request obligatoire, aucun push direct ;
- CI verte obligatoire ;
- au moins une relecture approuvée ;
- force push et suppression interdits.

### Le fichier CODEOWNERS

Le fichier `.github/CODEOWNERS` désigne les responsables de chaque partie du code (les référents du chapitre 4) :

```text
# Le Frontend est relu par le référent Frontend
/frontend/                @referent-frontend

# Le Backend est relu par le référent Backend
/backend/                 @referent-backend

# Le contrat d'API et le contrat d'équipe sont relus par les deux
/docs/openapi.yaml        @referent-frontend @referent-backend
/docs/contrat-equipe.md   @referent-frontend @referent-backend
```

On y met des noms d'utilisateurs GitHub (`@pseudo`) ou, dans une organisation GitHub, des équipes (`@organisation/equipe-frontend`).

En activant **Require review from Code Owners** dans la protection de branche, une modification du dossier `backend/` ne peut pas être fusionnée sans l'accord de l'équipe Backend. Une modification du contrat exige l'accord des deux équipes.

### Les pull requests

Une pull request est le lieu officiel de discussion sur le code : les questions, les remarques et les décisions y restent écrites et retrouvables.

## 10.4 Modèle de pull request

Un fichier `.github/pull_request_template.md` pré-remplit chaque pull request :

```markdown
## Objet

Lien vers le ticket : #

## Changements

-

## Impact sur l'autre équipe

- [ ] Aucun
- [ ] Le contrat d'API change (fichier `docs/openapi.yaml` modifié, autre équipe prévenue)

## Vérifications

- [ ] Lint et tests passent en local
- [ ] Testé avec le mock ou l'API de staging
- [ ] Documentation à jour
```

La case « Impact sur l'autre équipe » oblige à se poser la question avant chaque fusion.

## 10.5 Messages de commit

Les deux équipes utilisent la convention **Conventional Commits**, avec la partie concernée entre parenthèses :

```text
feat(front): ajouter le formulaire de connexion
feat(api): ajouter la route POST /api/users
fix(api): renvoyer 409 quand l'email est déjà utilisé
docs(contrat): ajouter la pagination sur GET /api/users
test(front): couvrir l'affichage des erreurs de validation
chore: mettre à jour les dépendances
```

| Type | Usage |
|---|---|
| `feat` | Nouvelle fonctionnalité |
| `fix` | Correction de bug |
| `docs` | Documentation ou contrat |
| `test` | Ajout ou modification de tests |
| `refactor` | Réorganisation du code sans changement de comportement |
| `chore` | Maintenance (dépendances, configuration) |

L'historique Git devient lisible par les deux équipes : on voit immédiatement ce qui touche l'API.

## 10.6 Monorepo ou deux dépôts

- **Monorepo** (`frontend/`, `backend/` et `docs/` dans le même dépôt) : une seule pull request peut modifier le contrat, le Backend et le Frontend ensemble. Recommandé pour une petite équipe ou un projet d'académiciens.
- **Deux dépôts** : chaque équipe est plus autonome, mais le contrat doit être partagé et versionné avec soin.

Ce choix est détaillé dans le cours [Stratégies de déploiement](https://github.com/dravelimbolo/strategies-deploiement-react-express) (chapitres 2 à 4).
