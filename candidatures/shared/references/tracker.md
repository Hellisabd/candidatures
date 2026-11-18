# Suivi des candidatures

Le suivi est la source unique des statuts, des relances et des contacts. Lis la rubrique « Suivi des candidatures » de `DATA_DIR/preferences.md` :
- `Tracker : local` (par défaut) : le fichier `DATA_DIR/suivi.md`.
- `Tracker : artefact [URL]` (option avancée) : un artefact Claude avec base de données (voir « Mode artefact »).

## Règles (valables dans les deux modes)

1. **Avant d'écrire ou de proposer une entreprise**, cherche-la dans le suivi : par entreprise et par lien, et aussi par un nom proche (« Acme » peut s'appeler « Acme Lyon »). Si une candidature est déjà en cours (`sent`, `r1`, `r2`), dis-le : ne suggère pas une nouvelle candidature froide et ne contredis pas les messages déjà envoyés.
2. **Même candidature déjà notée** : mets la ligne à jour en **ajoutant** à la colonne Notes, sans écraser. **Candidature différente chez la même entreprise** (autre poste, autre contrat, spontanée en plus d'une offre) : nouvelle ligne.
3. **Écris `sent` seulement après un envoi confirmé.** Une ligne `todo` peut être créée dès qu'une offre est repérée.
4. **Relance 1** = date d'envoi + 7 jours.
5. N'écris que ce que l'utilisateur a demandé ou validé.

## Mode local : `DATA_DIR/suivi.md`

Un tableau Markdown, une ligne par candidature (modèle : `shared/templates/suivi.md`) :

| Colonne | Contenu |
|---|---|
| Entreprise | nom de l'entreprise |
| Poste | intitulé ou domaine visé |
| Contrat | `alternance` ou `stage` (ou autre contrat visé) |
| Canal | `offre`, `LinkedIn`, `spontanée` ou `réseau` |
| Statut | `todo` (repérée, pas envoyée), `sent` (envoyée), `r1` (première relance faite), `r2` (deuxième relance), `entretien`, `ref` (refus), `offre` (proposition reçue) |
| Prio | 1 (haute) à 3 (basse) |
| Envoi, Relance 1, Relance 2 | dates AAAA-MM-JJ |
| Contact | nom et fonction, ou « aucun » |
| Lien | adresse de l'offre ou du profil |
| Notes | phrases datées, la plus récente à la fin (« JJ/MM : ... ») |

Pour modifier : lis le fichier, change la ligne concernée, réécris-le avec l'outil d'édition. Ne supprime jamais une ligne.

## Mode artefact (option avancée)

Pour qui tient son suivi dans un artefact Claude avec une collection `candidatures`. Outil : `ArtifactData` (`query`, `get`, `set`, `update`). Toute modification d'une fiche existante demande `if_version` (la version lue).

- **id** : entreprise en minuscules avec des tirets ; suffixe pour une deuxième candidature (`-stage`, `-spontanee`).
- **Champs** : `company`, `city`, `role`, `channel`, `status`, `prio` (un **nombre**), `sentDate`, `relance1`, `relance2` (AAAA-MM-JJ), `contact`, `contactInfo`, `interviewDate`, `link`, `notes`, `createdAt`, `updatedAt` (millisecondes depuis 1970).
- **Valeurs** : reprends celles déjà présentes dans l'artefact (ex. pour `status`, `todo`, `sent`, `r1`, `ref`) et n'en invente pas d'autres sans demander. Lis quelques fiches avant d'écrire pour respecter les conventions.
- Les fiches sont partagées et durables : écris uniquement ce qui est validé.
