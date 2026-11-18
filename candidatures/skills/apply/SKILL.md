---
name: apply
description: Postule à une offre d'alternance ou de stage (candidature simplifiée LinkedIn, site carrière ou plateforme, ou mail) : prépare le CV et la lettre, remplit le formulaire, demande une validation avant l'envoi, puis met à jour le tracker.
argument-hint: "URL de l'offre, plusieurs URL, 'last' ou 'current' (formulaire déjà ouvert)"
---

# Skill de candidature (France)

Passe d'une offre à une candidature **envoyée et notée dans le tracker**, sans rien oublier (pièce jointe, relance J+7). Rien ne part sans validation.

## Démarrage rapide

- `/candidatures:apply https://...` : postule à une offre
- `/candidatures:apply url1 url2 url3` : plusieurs offres, un seul récapitulatif à valider
- `/candidatures:apply last` : dernière offre enregistrée
- `/candidatures:apply current` : formulaire déjà ouvert dans l'onglet actif

## Dossier de données

Résous le dossier de données avec `shared/references/data-directory.md`.

## Langue et règles

Tout est en français. Lis `shared/references/marche-france.md` (alternance et stage au même niveau, honnêteté sur les projets) et `shared/references/tracker.md`.

## Données (une seule source, jamais dans ce fichier)

Lis dans `DATA_DIR` : `preferences.md`, `profile.md`, `reponses-types.md` (identité, liens, réponses types aux formulaires, parcours pour les champs Expérience et Formation) et `resume/*` (les CV). Aucune donnée personnelle n'est écrite dans ce skill : si une information manque, voir « Cas d'arrêt ».

## Workflow

### Étape 0 : Prérequis et canal
Vérifie les prérequis (`shared/references/prerequisites.md`). Pour chaque offre, détermine le **canal** :
- **LinkedIn, candidature simplifiée** (bouton « Candidature simplifiée ») ;
- **Site externe** (site carrière, Workday, Taleo, SmartRecruiters, Greenhouse, Lever, JobTeaser, Welcome to the Jungle, Indeed, France Travail) : y compris un bouton LinkedIn « Postuler » qui redirige ;
- **Mail** (l'offre donne une adresse).

### Étape 1 : Lire l'offre
Récupère l'offre (page web, sinon navigateur si la page est dynamique ou demande une connexion). Relève : entreprise, ville, intitulé, stack, type de contrat (alternance ou stage), durée ou rythme, date de début, contact, mode de candidature. Enregistre-la dans `DATA_DIR/jobs/[entreprise]-[date]/posting.md`.

### Étape 2 : Doublon et hors cible
- **Tracker** : cherche l'entreprise (`shared/references/tracker.md`). Déjà envoyée pour ce poste : signale-le et passe à l'offre suivante. Fiche `todo` : elle sera mise à jour, pas dupliquée. Autre candidature en cours chez la même entreprise : dis-le avant d'aller plus loin.
- **Hors cible** (barème de `shared/references/fit-scoring.md`) : CDI senior, hors zone sans télétravail, début incompatible, ni alternance ni stage. Dis-le en une ligne et ne postule pas, sauf si l'utilisateur insiste.

### Étape 3 : Repérer le formulaire
Ouvre le formulaire (voir « ATS » plus bas) et repère, sans rien remplir : champ CV, champ lettre (fichier ou texte), champs inhabituels. Cela fixe les documents à préparer.

### Étape 4 : Préparer les documents
- **CV** : choisis la version dans `DATA_DIR/resume/` (alternance seule, ou « alternance ou stage » pour un stage ou une spontanée) et, si l'offre le justifie, adapte-le avec `/candidatures:tailor-resume`. Récupère le PDF en local.
- **Lettre** : seulement si le formulaire ou le mail en demande une. Utilise `/candidatures:cover-letter` (ton, rôle réel dans chaque projet, archivage) : ne réécris pas de lettre ici. Si un fichier est demandé, produis `lettre.pdf`.
- **Message court** (LinkedIn simplifiée, champ « message ») : 4 à 5 phrases propres à l'offre, avec un détail précis de l'offre, un projet relié à leur stack, et les modalités selon le contrat visé. Mêmes règles d'honnêteté que la lettre.

### Étape 5 : Plan de remplissage
Scanne tous les champs (lecture seule) et propose une réponse pour chacun, dans cet ordre :
1. `reponses-types.md` et le parcours ;
2. réponses déjà enregistrées dans `DATA_DIR/reponses-types.md` (section « Réponses ajoutées ») ;
3. valeurs par défaut sûres (nom légal = nom, signature = nom complet, conditions générales = accepter en le signalant).

**Cas d'arrêt : laisse la candidature ouverte et demande à l'utilisateur** : prétentions salariales, adresse postale, date de naissance, nationalité ou titre de séjour, années d'expérience sur une technologie absente de `reponses-types.md`, test technique ou questionnaire de personnalité, handicap (choisis « je ne souhaite pas répondre » si l'option existe), compte, mot de passe, code de vérification ou CAPTCHA, lien vers un site qui ne ressemble pas à l'entreprise ou à une plateforme connue. Une fois la réponse obtenue, propose de l'ajouter à `reponses-types.md`.

### Étape 6 : Validation avant l'envoi (obligatoire)
Présente **en un seul message** : entreprise, poste, canal, lien ; CV choisi et **pièces jointes réellement attachées** (nom de fichier visible dans le formulaire ou le mail) ; texte complet de la lettre ou du message ; réponses aux questions libres ; réponses sensibles (expérience en années, mobilité).
- N'envoie qu'après un « oui » explicite.
- Si, juste avant l'envoi, ce qui est attaché ou saisi diffère de ce qui a été présenté, corrige puis redemande le « oui » : l'accord porte sur ce qui a été montré.
- **Plusieurs offres LinkedIn d'un coup** : présente le plan de chaque offre (CV, message) dans un seul récapitulatif, obtiens un « oui » pour le lot, puis envoie une par une. Arrête-toi à la première question inattendue.

### Étape 7 : Remplir et envoyer
Remplis avec `scripts/fill-page.md` (ou directement pour un mail). Règles communes :
- Refuse les cookies non essentiels. Ne coche aucune case optionnelle (newsletter, alertes, vivier de talents) sauf demande de l'utilisateur. Décoche « Suivre l'entreprise » sur LinkedIn.
- Si le site propose « remplir à partir du CV », utilise-le puis corrige chaque champ pré-rempli d'après le parcours (les dates sont souvent fausses).
- Mois et année toujours saisis dans les champs Expérience et Formation. N'ajoute aucune ligne absente du parcours.
- Fichiers : utilise l'outil d'upload du navigateur. S'il ne peut pas envoyer ce type de fichier, donne le chemin à l'utilisateur et demande-lui de l'envoyer lui-même.
- **Comptes candidat** (Workday, Taleo, SmartRecruiters…) : remplis tout ce qui ne demande pas de compte, puis demande à l'utilisateur de créer le compte ou de se connecter lui-même (mot de passe, code, CAPTCHA). Ne saisis jamais de mot de passe.
- **Mail** : envoie uniquement depuis l'adresse de `reponses-types.md` (le webmail indiqué dans `reponses-types.md`, session déjà ouverte). Objet = intitulé exact de l'offre + « - candidature [alternance ou stage] ». Joins le CV et la lettre, vérifie que les deux apparaissent, valide (étape 6), envoie, puis vérifie le message dans les éléments envoyés, pièces jointes comprises.
- Après l'envoi, vérifie la confirmation (page de confirmation, ou mail). Sans confirmation visible, la candidature n'est pas considérée comme envoyée.

### Étape 8 : Enregistrer
Juste après l'envoi confirmé, mets à jour le tracker (`shared/references/tracker.md`) : fiche (nouvelle ou mise à jour avec `if_version`), `status: "sent"`, `sentDate`, `channel` réel, `relance1` = date + 7 jours, `prio` en **nombre**, et une note datée : « JJ/MM : candidature envoyée via [canal] avec [version du CV] ». Archive la lettre dans le dossier des lettres. Fais un récapitulatif en 2 ou 3 lignes : où, avec quoi, date de relance.

## ATS : notes d'interaction

Voir `shared/references/ats-patterns.md` (Greenhouse, Lever, Workday). Pour Taleo, SmartRecruiters, JobTeaser, Welcome to the Jungle : aucun schéma établi. À la première rencontre, note ici ce qui marche (sélecteurs, pièges) pour ne pas le redécouvrir.

## Erreurs déjà commises

- Mail envoyé **sans le CV** : l'étape 6 contrôle la pièce jointe réellement présente, pas l'intention.
- `prio` enregistré en texte (`"3"`) : toujours un nombre.
- Réponse « 3 ans d'expérience » donnée sur une technologie vue en un seul projet : demander, ne pas supposer.

---

## Autorisations nécessaires

Ajoute à `~/.claude/settings.json` :

```json
{
  "permissions": {
    "allow": [
      "Read(~/.candidatures/**)",
      "Write(~/.candidatures/**)",
      "Edit(~/.candidatures/**)",
      "mcp__claude-in-chrome__*"
    ]
  }
}
```
