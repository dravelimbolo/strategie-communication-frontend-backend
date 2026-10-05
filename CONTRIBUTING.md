# Contribuer au cours

Merci de votre intérêt pour ce cours. Les corrections (faute, exemple erroné, lien cassé) et les propositions d'amélioration sont les bienvenues.

## Signaler un problème

Ouvrez une **issue** en précisant :

- le fichier et la section concernés (par exemple `02_contrat_api.md`, section 2.3) ;
- ce qui est incorrect ou peu clair ;
- si possible, la correction proposée et une source (documentation officielle).

Pour une question de compréhension, précisez ce que vous avez essayé et ce que vous n'avez pas compris.

## Organisation des branches

Le dépôt suit une organisation inspirée de **Git Flow**, la même que celle enseignée au chapitre 4 :

| Branche | Rôle |
|---|---|
| `main` | Version publiée du cours. On n'y pousse jamais directement. |
| `develop` | Branche d'intégration : les contributions y sont fusionnées avant publication. |
| `feature/<sujet>` | Nouveau contenu ou amélioration, créée depuis `develop`. |
| `fix/<sujet>` | Correction (erreur, faute, lien), créée depuis `develop`. |
| `hotfix/<sujet>` | Correction urgente du contenu publié, créée depuis `main`, puis fusionnée dans `main` et `develop`. |
| `release/<version>` | Préparation d'une version publiée, créée depuis `develop` puis fusionnée dans `main`. |

Les noms de branches sont en minuscules, avec des tirets entre les mots : `feature/chapitre-graphql`, `fix/exemple-msw`.

## Proposer une modification

Pour les contributeurs externes, le fonctionnement est celui du **GitHub Flow** : une branche courte, une pull request, une relecture, une fusion.

1. Forker le dépôt.
2. Créer une branche depuis `develop` :

```bash
git checkout develop
git pull
git checkout -b fix/exemple-msw
```

3. Faire les modifications et les commiter (voir les conventions ci-dessous).
4. Pousser la branche sur votre fork et ouvrir une **pull request vers `develop`** (jamais vers `main`).
5. Décrire dans la pull request ce qui change et pourquoi.

Le mainteneur relit, demande éventuellement des ajustements, puis fusionne. Le passage de `develop` vers `main` est fait par le mainteneur lors de la publication d'une nouvelle version.

## Messages de commit

Les messages suivent la convention **Conventional Commits**, en français :

```text
<type>: <description courte à l'infinitif>
```

| Type | Usage |
|---|---|
| `docs` | Ajout ou modification de contenu du cours |
| `fix` | Correction d'une erreur (exemple, explication) |
| `chore` | Maintenance du dépôt (licence, `.gitignore`, organisation) |
| `ci` | Workflows GitHub Actions du dépôt |

Exemples :

```text
docs: ajouter un exemple de pagination au chapitre 2
fix: corriger l'exemple MSW du chapitre 3
chore: mettre à jour le .gitignore
```

Un commit correspond à une modification cohérente. Éviter les commits du type « modifs » ou « wip ».

## Règles de rédaction

- Écrire en français, avec un vocabulaire accessible aux débutants. Tout nouveau terme technique est ajouté au glossaire (chapitre 12).
- Pas d'emoji.
- Pas de tiret cadratin ni de tiret utilisé comme ponctuation : utiliser les deux points, une virgule ou une nouvelle phrase.
- Les exemples de code doivent être **exacts et testés** : les apprenants les copient tels quels.
- Indiquer le langage de chaque bloc de code (`yaml`, `json`, `js`, `bash`, `text`...).
- Utiliser les conventions du cours : routes préfixées par `/api`, champs JSON en `camelCase`, format d'erreur du chapitre 2.
- Garder la cohérence entre les chapitres : si une modification touche une notion présente ailleurs, mettre à jour les autres chapitres et la checklist de démarrage (chapitre 11).
- Ne jamais inclure de vraie donnée personnelle, de secret ou de nom de domaine personnel dans un exemple.

## Licence

En contribuant, vous acceptez que votre contribution soit publiée sous la licence du dépôt : [CC BY 4.0](LICENSE).
