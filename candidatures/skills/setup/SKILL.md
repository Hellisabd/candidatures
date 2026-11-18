---
name: setup
description: Mise en place unique - CV, préférences d'alternance et interview sur le parcours (marché français)
argument-hint: "'interview' to skip to the interview portion"
---

# Setup Skill

> **Priority hierarchy**: See `shared/references/priority-hierarchy.md` for conflict resolution.

Mise en place unique : CV, préférences d'alternance et interview sur le parcours, avant d'utiliser les autres skills.

## Quick Start

- `/candidatures:setup` - Full onboarding (checks what's missing, does only what's needed)
- `/candidatures:setup interview` - Just the work history interview (if resume/prefs are already done)

## File Structure

```
scripts/
  conduct-interview.md    # Work history interview guide
```

The profile template is at `shared/templates/profile.md`.

## Langue et marché

Tout est en **français** : ce que tu dis à l'utilisateur et ce que tu écris dans les fichiers. Avant de commencer, lis `shared/references/marche-france.md` : il décrit le contexte français (alternance, CV 1 page, lettre française) et l'emporte sur toute consigne américaine restante.

## Data Directory

Resolve the data directory using `shared/references/data-directory.md`. For setup, if no directory exists this is a fresh install — create it in Step 1.

---

## Workflow

### Step 0: Check What's Already Done

Resolve the data directory, then check which of these exist and have real content (not just templates): resume, preferences, linkedin-contacts.csv, profile.md, reponses-types.md.

If `$ARGUMENTS` is "interview", skip to Step 3 (but check that a resume exists first).

If everything exists, tell the user they're good to go and list the available skills. Otherwise, run only the missing phases in order.

### Step 1: CV

Demande à l'utilisateur son CV. Accepte :
- Un chemin de fichier (copie-le dans `DATA_DIR/resume/`)
- Du texte collé (enregistre-le dans `DATA_DIR/resume/resume.md`)

Confirme l'enregistrement et résume brièvement ce que tu vois (nom, formation en cours, expérience la plus récente, nombre d'expériences et de projets).

### Step 2: Préférences de recherche

Pose une seule question naturelle, en français :

> "Quel type d'alternance cherchez-vous ? Dites-moi : le poste ou le domaine visé, le diplôme préparé et l'école, les types de contrat acceptés (alternance en apprentissage ou professionnalisation, stage : ils sont mis au même niveau de priorité), la durée, le rythme école/entreprise pour l'alternance, la période pour un stage, la date de début, la ville de résidence, la zone géographique (villes, mobilité), les compétences ou outils techniques que vous maîtrisez et ceux que vous voulez apprendre (si votre métier s'y prête), et ce que vous voulez éviter."

À partir de la réponse, enregistre `DATA_DIR/preferences.md` :

```markdown
# Préférences de recherche

## Formation
- École : [école, ville]
- Diplôme préparé : [intitulé, années du cursus. N'écris l'année en cours (1re, 2e année...) que si l'utilisateur l'a dite : ne la déduis pas]

## Types de contrat acceptés (au même niveau de priorité)
### Alternance
- Contrat : [apprentissage / professionnalisation]
- Durée : [12 / 24 / 36 mois]
- Rythme : [ex. 2 semaines entreprise / 2 semaines école]
- Date de début : [mois et année]
### Stage (si accepté)
- Convention de stage avec : [école]
- Durée : [ex. 2 à 6 mois]
- Période et date de début : [dates]

## Postes visés
- [parsé de la réponse]

## Stack (compétences ou outils du métier, facultatif)
- Maîtrisée (issue du CV et confirmée par le candidat) : [ex. langages et frameworks pour un développeur, logiciels pour un autre métier]
- Souhaitée, à apprendre (jamais pratiquée ou débutant) : [ex. ...]

## Localisation
- Ville de résidence : [ville, utilisée pour « Fait à [ville] » dans les lettres]
- Zone de recherche : [villes, rayon, mobilité, télétravail accepté ou non]

## Rémunération
[alternance : grille légale selon l'âge ; noter seulement une attente particulière]

## Indispensables
- [parsé de la réponse]

## Rédhibitoires
- [parsé de la réponse]

## Appréciés
- [parsé de la réponse]

## Ton des lettres
- Ton : [classique et chaleureux / direct et sobre / autre]
- Modèle de ton : [facultatif : DATA_DIR/lettre-modele-ton.md]

## Archivage
- Dossier des lettres : [par défaut ~/Documents/Candidatures/Lettres ; un sous-dossier par candidature]

## Suivi des candidatures
- Tracker : [local (par défaut : DATA_DIR/suivi.md) ou artefact avec son adresse]
```

Crée aussi `DATA_DIR/suivi.md` à partir de `shared/templates/suivi.md` s'il n'existe pas, et le dossier des lettres. Si des éléments manquent, ce n'est pas grave : enregistre ce que tu as. Les informations de rythme, de durée et de date de début seront redemandées au moment de rédiger si elles manquent.

### Step 2bis: Ton des lettres de motivation

Demande (avec `AskUserQuestion` si disponible) : « Quel ton voulez-vous pour vos lettres de motivation ? »
- **Classique et chaleureux** : formules d'usage, motivation pour l'entreprise, 5 ou 6 paragraphes (environ 350 mots)
- **Direct et sobre** : court et factuel, 3 ou 4 paragraphes (environ 220 mots)
- **Autre** : à décrire, ou collez une lettre dont vous aimez le ton

Enregistre le choix dans `preferences.md` (rubrique « Ton des lettres »). Si l'utilisateur colle un texte, enregistre-le dans `DATA_DIR/lettre-modele-ton.md` avec la mention « registre à imiter, jamais ses faits ».

### Step 3: Contacts LinkedIn (optionnel)

Si `DATA_DIR/linkedin-contacts.csv` n'existe pas, propose :

> "Souhaitez-vous importer vos contacts LinkedIn ? Cela sert à `job-search`, qui peut alors signaler les entreprises où vous connaissez quelqu'un. Vous pouvez passer cette étape."

S'il accepte, donne ces instructions :

> **Exporter vos connexions LinkedIn :**
> 1. Allez sur linkedin.com/mypreferences/d/download-my-data
> 2. Sélectionnez « Connexions » et demandez le téléchargement
> 3. LinkedIn vous envoie un lien par e-mail (en général quelques minutes)
> 4. Téléchargez le ZIP et récupérez `Connections.csv`
> 5. Envoyez ce fichier ou son chemin ici

Enregistre le fichier dans `DATA_DIR/linkedin-contacts.csv`, puis indique le nombre de contacts importés. S'il passe l'étape, continue : elle est facultative.

### Step 4: Interview sur le parcours

Mène une interview conversationnelle pour construire le profil, en suivant `scripts/conduct-interview.md`. Passe en revue chaque expérience et chaque projet du CV, le plus récent d'abord. Pour chacun, demande :

1. « Parlez-moi de [structure ou projet] : que faisait-elle, et quelle était vraiment votre mission ? »
2. « Quelles sont vos principales réalisations ? Soyons précis, avec des chiffres si vous en avez. »
3. « Autre chose : difficultés, outils, ce que vous avez appris ? »

**Reste conversationnel.** Relance quand une réponse est vague (« Vous souvenez-vous à peu près des chiffres ? »), sans interroger. Passe plus de temps sur ce qui est récent et proche des postes visés.

À la fin, enregistre le profil dans `DATA_DIR/profile.md` avec ce format :

```markdown
# Profil du candidat

*Dernière mise à jour : [DATE]*

## Vue d'ensemble
**Nom** : [Nom]
**Formation en cours** : [diplôme, école, années]
**Recherche** : [contrat, durée, rythme, date de début]
**Compétences clés** : [2-3 phrases]
**Fil conducteur** : [ce qui relie le parcours]

---

## Expérience : [Intitulé] chez [Structure]
**Type** : [alternance / stage / emploi / bénévolat]
**Dates** : [début - fin]
**Contexte** : [ce que fait la structure, taille]

### Réalisations principales
1. **[Titre]** : [Situation, Action, Résultat avec chiffres]
2. **[Titre]** : [Situation, Action, Résultat avec chiffres]

### Autres détails
- Équipe et encadrement : [détails]
- Outils et méthodes : [détails]
- Difficultés et apprentissages : [détails]

---

## Projets
### [Nom] ([scolaire / personnel / open source])
- Rôle, technologies, résultat : [détails]

---

## Langues et engagements
- [langue : niveau] ; [associations, centres d'intérêt]

---

## Points transversaux
**Point fort** : [ce qu'il fait le mieux]
**Thèmes récurrents** : [motifs]
```

### Step 4bis: Réponses types pour les formulaires

Si `DATA_DIR/reponses-types.md` n'existe pas, crée-le à partir de `shared/templates/reponses-types.md`, du CV, des préférences et du profil, avec ces rubriques : identité et liens, réponses types (contrat, dates, rythme, mobilité, niveau d'études, langues), **années d'expérience par technologie (demande-les à l'utilisateur, ne les devine pas)**, parcours pour les champs Expérience et Formation, réponses ajoutées. Fais valider ce fichier : `/candidatures:apply` s'y réfère pour chaque formulaire.

### Step 5: Résumé

```
Tout est prêt. Voici ce que nous avons :

- CV : [nom du fichier] dans DATA_DIR/resume/
- Préférences : [résumé du poste visé, du contrat, du rythme et de la zone]
- Contacts LinkedIn : [nombre] importés (ou « passé »)
- Profil du parcours : [nombre d'expériences et de projets couverts]

Vous pouvez maintenant utiliser :
- /candidatures:tailor-resume [URL de l'offre] : adapter votre CV
- /candidatures:cover-letter [URL de l'offre] : rédiger une lettre de motivation
```

---

## Response Format

Structure the final summary output with these sections:

1. **Résumé de la mise en place** : ce qui a été configuré (CV, préférences, contacts, profil), avec quelques détails
2. **Et ensuite** : les skills que l'utilisateur peut lancer

---

## Permissions Required

Add to `~/.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "Read(~/.candidatures/**)",
      "Write(~/.candidatures/**)",
      "Edit(~/.candidatures/**)"
    ]
  }
}
```
