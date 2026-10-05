# 10. Une définition commune de « terminé »

## 10.1 Le problème

Une fonctionnalité n'est pas terminée simplement parce que :

> « J'ai codé mon morceau. »

Si le Backend considère sa route terminée alors que le Frontend ne l'a pas encore intégrée, la fonctionnalité n'est pas utilisable. Chacun pense avoir fini, mais l'utilisateur n'a rien.

## 10.2 La Definition of Done

Une fonctionnalité est **terminée** lorsque :

```text
┌──────────────────────────────────────┐
│          FONCTIONNALITÉ TERMINÉE     │
├──────────────────────────────────────┤
│ Contrat d'API validé et à jour       │
│ Backend développé                    │
│ Tests Backend OK                     │
│ Frontend développé                   │
│ Tests Frontend OK                    │
│ Erreurs et chargements gérés         │
│ API réelle intégrée (mock retiré)    │
│ Relecture de code OK                 │
│ CI OK                                │
│ Déployée et vérifiée en staging      │
│ Documentation à jour                 │
│ Critères d'acceptation du ticket OK  │
└──────────────────────────────────────┘
```

C'est ce qu'on appelle une **Definition of Done** (DoD).

## 10.3 Bonnes pratiques

- La DoD est écrite dans le dépôt (par exemple dans `CONTRIBUTING.md`) et connue de tous.
- Elle est la même pour le Frontend et le Backend.
- Un ticket ne passe dans la colonne « Terminé » du tableau Kanban que si **tous** les points sont validés.
- La DoD peut évoluer lors des rétrospectives, mais jamais pendant une fonctionnalité en cours.
- La démonstration de fin d'itération (chapitre 8) se fait sur staging : c'est la preuve que la DoD est respectée.
