# 5. Les règles d'équipe

Les conventions disent **comment écrire** le code. Les règles disent **comment travailler** ensemble. Elles répondent aux problèmes « le code de l'autre est modifié », « terminé ne veut pas dire la même chose » et « les désaccords deviennent personnels » (chapitre 2).

## 5.1 Les 5 règles fondamentales

### 1. Ne pas modifier le travail de l'autre sans discussion

Chaque équipe conserve son périmètre, et les changements ayant un impact sur l'autre équipe sont communiqués avant d'être faits. Le fichier `CODEOWNERS` rend cette règle automatique (chapitre 10).

### 2. L'API est un contrat entre le Frontend et le Backend

Les deux équipes se mettent d'accord sur les données et les comportements avant de coder, et le contrat écrit fait foi.

### 3. Tout changement d'API est communiqué et documenté

Un changement de route, de structure JSON, de code HTTP ou d'authentification peut casser le Frontend. Il passe par une pull request sur le contrat, validée par les deux équipes (chapitre 12).

### 4. Les pull requests sont obligatoires

Le code est relu avant d'être intégré, afin de limiter les erreurs et de partager les connaissances. Personne ne pousse directement sur `main` ou `develop`.

### 5. Critiquer le code et le processus, pas la personne

Les désaccords techniques restent professionnels. Le but est d'améliorer le projet, pas de chercher un responsable.

## 5.2 Les règles complémentaires

- **Une demande qui n'est pas écrite dans un ticket n'existe pas.** Les demandes orales se perdent.
- **Une décision prise à l'oral est recopiée par écrit** dans le ticket ou la pull request concernés.
- **Un blocage est signalé dès qu'il dure plus d'une demi-journée.**
- **Les pull requests restent petites** et sont relues dans la journée.
- **Une variable d'environnement ajoutée** est documentée dans `.env.example` dans la même pull request.
- **On ne laisse jamais `develop` cassée** : si une fusion casse la CI, la correction passe avant tout le reste.

## 5.3 La Definition of Done

Une fonctionnalité n'est pas terminée simplement parce que :

> « J'ai codé mon morceau. »

La Definition of Done (DoD) est la règle commune qui dit quand une fonctionnalité est **vraiment** terminée.

```text
┌──────────────────────────────────────┐
│        FONCTIONNALITÉ TERMINÉE       │
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

Bonnes pratiques :

- la DoD est la même pour le Frontend et le Backend ;
- un ticket ne passe dans la colonne « Terminé » que si **tous** les points sont validés ;
- la DoD peut évoluer lors des rétrospectives, mais jamais pendant une fonctionnalité en cours ;
- la démonstration de fin d'itération (chapitre 6) se fait sur staging : c'est la preuve que la DoD est respectée.
