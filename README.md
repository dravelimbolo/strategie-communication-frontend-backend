<div align="center">

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Inter&weight=900&size=40&duration=2800&pause=1200&color=E8590C&center=true&vCenter=true&width=700&height=90&lines=Strat%C3%A9gie+de+communication;Frontend+%2B+Backend)](https://github.com/dravelimbolo/strategie-communication-frontend-backend)

**`Cours · Collaboration · Contrat d'API · Git Flow · Code review · Agile`**

_Faire travailler une équipe Frontend React et une équipe Backend Express ensemble, sans conflit._

<br/>

[![Portfolio](https://img.shields.io/badge/-dravelimbolo.com-111111?style=for-the-badge&logo=safari&logoColor=white)](https://dravelimbolo.com)
[![LinkedIn](https://img.shields.io/badge/-LinkedIn-111111?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/dravel-imbolo)
[![GitHub](https://img.shields.io/badge/-GitHub-111111?style=for-the-badge&logo=github&logoColor=white)](https://github.com/dravelimbolo)
[![Email](https://img.shields.io/badge/-contact@dravelimbolo.com-111111?style=for-the-badge&logo=gmail&logoColor=white)](mailto:contact@dravelimbolo.com)

<br/>

[![Akieni Academy](https://img.shields.io/badge/Akieni_Academy-mentorat-E8590C.svg)](#mentorat)
[![Licence CC BY 4.0](https://img.shields.io/badge/licence-CC_BY_4.0-E8590C.svg)](LICENSE)
[![PRs bienvenues](https://img.shields.io/badge/PRs-bienvenues-E8590C.svg)](CONTRIBUTING.md)

</div>

---

<div align="center">

```
Contrat d'API   ·   Mocks   ·   Git Flow   ·   Tickets   ·   Definition of Done
```

_Le lien entre le Frontend et le Backend n'est pas seulement GitHub : c'est un contrat, des tickets et une communication claire._

</div>

---

## Stack technique

<table align="center">
<tr>
  <td align="center">
    <strong>Frontend</strong><br/>
    <img src="https://skillicons.dev/icons?i=react,vite,js" />
  </td>
  <td align="center">
    <strong>Backend</strong><br/>
    <img src="https://skillicons.dev/icons?i=nodejs,express" />
  </td>
  <td align="center">
    <strong>Collaboration & Outils</strong><br/>
    <img src="https://skillicons.dev/icons?i=git,github,githubactions,postman" />
  </td>
</tr>
</table>

---

## Le modèle de collaboration

```
Ticket  ──>  Contrat d'API (OpenAPI)  ──>  Frontend + mocks  |  Backend
        ──>  Pull Requests + relecture + CI  ──>  develop (staging)  ──>  main (production)
```

```
strategie-communication-frontend-backend/
├── 01_introduction_et_objectifs.md        # Le problème, l'idée centrale, les objectifs
├── 02_contrat_api.md                      # OpenAPI · format d'erreur · pagination · nommage
├── 03_travailler_en_parallele.md          # Mocks avec Prism, MSW, json-server
├── 04_git_et_branches.md                  # Git Flow · CODEOWNERS · modèle de PR · commits
├── 05_conventions_communes.md             # Conventions · .env.example · proxy Vite · CORS
├── 06_roles_et_responsabilites.md         # Qui fait quoi · validation des deux côtés · binôme
├── 07_tickets_et_suivi.md                 # Bon ticket · labels · tableau Kanban
├── 08_rituels_et_communication.md         # Daily · canaux · relecture bienveillante · désaccords
├── 09_gerer_les_changements_api.md        # Changements cassants · transition · versionnement
├── 10_definition_of_done.md               # Définition commune de « terminé »
├── 11_modele_recommande_et_regles.md      # Modèle complet · 5 règles · checklist de démarrage
├── 12_glossaire.md                        # Termes expliqués simplement
├── 13_travaux_pratiques.md                # 8 TP en binôme Frontend + Backend
├── assets/                                # Logo Akieni Academy
├── CONTRIBUTING.md                        # Guide de contribution
├── LICENSE                                # Licence CC BY 4.0
└── README.md
```

### Ce que tu vas apprendre

| Notion | Mise en oeuvre dans le cours |
|---|---|
| **Contrat d'API** | Fichier OpenAPI validé par les deux équipes avant de coder, format d'erreur unique |
| **Travail en parallèle** | Le Frontend avance avec des mocks (Prism, MSW) pendant que le Backend code l'API |
| **Git en équipe** | Git Flow, protection des branches, `CODEOWNERS`, modèle de pull request, Conventional Commits |
| **Responsabilités** | Tableau « qui fait quoi », validation côté Frontend et côté Backend |
| **Suivi** | Tickets clairs avec critères d'acceptation, labels, tableau Kanban GitHub |
| **Communication** | Rituels courts, bon canal pour chaque message, relecture de code bienveillante |
| **Évolution de l'API** | Changements compatibles ou cassants, période de transition, versionnement |
| **Qualité** | Une Definition of Done commune aux deux équipes |

---

## Par où commencer

> **Prérequis :** bases de JavaScript, React et Express · notions de HTTP et JSON · Git et GitHub

| Étape | Chapitres | Objectif |
|---|---|---|
| **1** | [01](01_introduction_et_objectifs.md) | Comprendre pourquoi les équipes entrent en conflit |
| **2** | [02](02_contrat_api.md) et [03](03_travailler_en_parallele.md) | Écrire un contrat et travailler avec des mocks |
| **3** | [04](04_git_et_branches.md) à [07](07_tickets_et_suivi.md) | Organiser Git, les conventions, les rôles et les tickets |
| **4** | [08](08_rituels_et_communication.md) à [10](10_definition_of_done.md) | Communiquer, faire évoluer l'API, définir « terminé » |
| **5** | [11](11_modele_recommande_et_regles.md) puis [13](13_travaux_pratiques.md) | Appliquer le modèle complet en binôme |

> Le [glossaire](12_glossaire.md) explique simplement chaque terme technique.

### Cours associé

Ce cours complète **[Stratégies de déploiement d'une application React + Express](https://github.com/dravelimbolo/strategies-deploiement-react-express)** : le premier explique comment livrer l'application, celui-ci comment la construire ensemble.

---

## Mentorat

<table align="center">
<tr>
  <td align="center">
    <img src="assets/akieni-academy-logo.png" alt="Akieni Academy" width="110" />
  </td>
  <td>
    <strong>Suivi Individuel Mentorat · Akieni Academy</strong><br/><br/>
    Mentor référent : <strong>Dravel-Ameguste IMBOLO</strong><br/>
    Contact : <a href="mailto:contact@dravelimbolo.com">contact@dravelimbolo.com</a>
  </td>
</tr>
</table>

---

## Contributions

<table style="border-spacing:0; font-size:13px; width:100%;">
<tr>
  <th style="padding:8px 12px;">Rôle</th>
  <th style="padding:8px 12px;">Personne</th>
  <th style="padding:8px 12px;">Contribution</th>
</tr>
<tr>
  <td style="padding:8px 12px;"><strong>Auteur & mainteneur</strong></td>
  <td style="padding:8px 12px;"><a href="https://github.com/dravelimbolo">Dravel IMBOLO</a></td>
  <td style="padding:8px 12px;">Conception du cours · rédaction · travaux pratiques · mentorat</td>
</tr>
</table>

Les contributions sont les bienvenues. Avant d'ouvrir une pull request, lis le **[guide de contribution](CONTRIBUTING.md)** : branches Git Flow, commits conventionnels et pull requests vers `develop`.

---

## Licence

Distribué sous licence **Creative Commons Attribution 4.0 International (CC BY 4.0)**. Voir le fichier [LICENSE](LICENSE).

Tu peux partager et adapter ce cours, à condition de citer l'auteur et de fournir un lien vers la licence.

Copyright (c) 2026 **Dravel IMBOLO**.

---

<div align="center">

<br/>

_"Transformons des idées en applications fonctionnelles, robustes et scalables."_

<br/>

</div>
