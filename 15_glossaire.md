# 15. Glossaire

**ADR** : Architecture Decision Record, court document qui garde la trace d'une décision technique (contexte, décision, conséquences).

**API** : Application Programming Interface, ensemble des routes qu'un Backend met à disposition du Frontend.

**Atelier de lancement** : réunion de début de projet où les équipes construisent ensemble leur contrat d'équipe.

**Backend** : partie de l'application qui s'exécute sur le serveur (ici Node.js et Express).

**Changement cassant** : modification de l'API qui empêche un client existant de fonctionner (champ supprimé ou renommé, type changé...).

**CI** : Continuous Integration, vérification automatique (lint, tests, build) de chaque pull request.

**Code-first** : approche où le contrat d'API est généré à partir du code déjà écrit.

**Code HTTP** : nombre renvoyé par le serveur pour indiquer le résultat d'une requête (`200` succès, `404` introuvable, `500` erreur serveur...).

**Code review (relecture de code)** : lecture d'une pull request par un autre développeur avant sa fusion.

**CODEOWNERS** : fichier GitHub qui désigne les personnes ou équipes responsables de chaque partie du code, et dont l'accord est requis pour fusionner.

**Contrat d'API** : description écrite et validée de l'API (routes, données, réponses, erreurs), au format OpenAPI. C'est la partie technique du contrat d'équipe.

**Contrat d'équipe** : document écrit qui rassemble les accords entre les équipes : conventions, responsabilités, règles, rituels.

**Convention** : règle de forme décidée ensemble pour que tout le monde fasse pareil (nommage, format, outils).

**Conventional Commits** : convention d'écriture des messages de commit (`feat:`, `fix:`, `docs:`...).

**CORS** : Cross-Origin Resource Sharing, mécanisme par lequel un serveur autorise un site d'une autre origine à l'appeler depuis le navigateur.

**Critères d'acceptation** : conditions vérifiables qui permettent de dire qu'un ticket est réalisé.

**Daily (point quotidien)** : réunion de 15 minutes maximum où chacun indique son avancement et ses blocages.

**Definition of Done (DoD)** : liste commune des conditions à remplir pour qu'une fonctionnalité soit considérée comme terminée.

**Design-first** : approche où le contrat d'API est écrit et validé avant le code. C'est celle recommandée dans ce cours.

**Endpoint (route)** : combinaison d'une méthode HTTP et d'une URL, par exemple `POST /api/users`.

**express-openapi-validator** : middleware Express qui refuse automatiquement les requêtes qui ne respectent pas le contrat OpenAPI.

**Frontend** : partie de l'application qui s'exécute dans le navigateur (ici React avec Vite).

**Intégration** : moment où le Frontend utilise l'API réelle à la place du mock.

**Issue (ticket)** : élément de suivi GitHub qui décrit une tâche, un besoin ou un bug.

**JSON** : format texte utilisé pour échanger des données entre le Frontend et le Backend.

**Kanban** : tableau en colonnes (À faire, En cours, En revue, Terminé) qui montre l'avancement des tickets.

**Middleware** : fonction Express exécutée sur chaque requête avant d'arriver à la route (lecture du JSON, validation, gestion des erreurs).

**Mock** : simulation de l'API qui renvoie des données d'exemple, pour travailler sans attendre l'API réelle.

**MSW** : Mock Service Worker, outil qui intercepte les requêtes du navigateur pour renvoyer des réponses simulées.

**OpenAPI** : format standard (YAML ou JSON) de description d'une API REST, anciennement appelé Swagger.

**Pagination** : découpage d'une longue liste en pages (`page`, `limit`, `total`).

**Prism** : outil qui démarre un faux serveur à partir d'un fichier OpenAPI.

**Pull Request** : demande de fusion d'une branche, avec discussion et relecture.

**Redocly CLI** : outil en ligne de commande qui vérifie qu'un contrat OpenAPI est valide.

**Référent** : membre d'une équipe qui relit ses pull requests et la représente dans les décisions.

**REST** : style d'API basé sur des ressources (`/users`) et les méthodes HTTP (`GET`, `POST`, `PATCH`, `DELETE`).

**Rétrospective** : réunion de fin d'itération pour améliorer la façon de travailler.

**Rituel** : moment d'échange régulier et prévu à l'avance (point quotidien, démonstration, rétrospective).

**Staging** : environnement de préproduction, où le Frontend et le Backend sont intégrés et validés.

**Swagger Editor** : éditeur en ligne qui affiche la documentation d'un contrat OpenAPI pendant qu'on l'écrit.

**Swagger UI** : page web interactive générée à partir du contrat, qui permet de lire et d'essayer chaque route.

**Versionnement d'API** : coexistence de plusieurs versions de l'API (`/api/v1`, `/api/v2`) pour migrer sans casser.

**YAML** : format texte lisible basé sur l'indentation, utilisé pour écrire les contrats OpenAPI.
