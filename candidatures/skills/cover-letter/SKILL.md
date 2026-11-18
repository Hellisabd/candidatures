---
name: cover-letter
description: Rédige en français une lettre de motivation (alternance ou stage), à partir d'une offre ou en candidature spontanée à partir d'une entreprise
argument-hint: "URL de l'offre, 'last', ou 'spontanée [entreprise ou site]'"
---

# Cover Letter Skill

> **Hiérarchie des priorités** : voir `shared/references/priority-hierarchy.md` pour résoudre les conflits.

Rédige des lettres de motivation naturelles et convaincantes, au format français, qui sonnent comme écrites par la personne elle-même.

## Démarrage rapide

- `/candidatures:cover-letter` : démarre (demande une offre ou une entreprise)
- `/candidatures:cover-letter https://...` : lettre pour une offre précise
- `/candidatures:cover-letter last` : lettre pour la dernière offre enregistrée
- `/candidatures:cover-letter spontanée Acme` : candidature spontanée à une entreprise, sans offre

**Deux modes.** *Offre* : la lettre répond aux besoins d'une annonce. *Spontanée* : la lettre part d'une entreprise (ce qu'elle fait, ses technologies publiées) et de la motivation du candidat. Les règles de style et d'exactitude sont les mêmes.

## Fichiers

```
scripts/
  write-cover-letter.md       # Consignes de l'agent de rédaction
```

## Langue et marché

Tout est en **français** : ce que tu dis à l'utilisateur et ce que tu écris dans les fichiers. Avant de commencer, lis `shared/references/marche-france.md` : il décrit le contexte français (alternance, CV 1 page, lettre française) et l'emporte sur toute consigne américaine restante.

## Dossier de données

Résous le dossier de données avec `shared/references/data-directory.md`.

---

## Workflow

### Étape 0 : Vérifier les prérequis

Résous le dossier de données, puis vérifie les prérequis selon `shared/references/prerequisites.md`. Le CV est obligatoire ; le profil est recommandé mais pas bloquant.

### Étape 1 : Choisir le mode et réunir les informations

**Détermine le mode :**
- `$ARGUMENTS` commence par « spontanée » (ou « spontanee », « spontané »), ou c'est un nom d'entreprise, ou l'adresse d'un site d'entreprise qui n'est pas une offre : **mode spontané**.
- `$ARGUMENTS` est l'adresse d'une offre, ou `last` : **mode offre**.
- Cas ambigu (adresse d'une page d'accueil) : demande à l'utilisateur.

#### Mode offre

**Si `$ARGUMENTS` est « last » ou vide :**
- Regarde le dossier le plus récent dans `DATA_DIR/jobs/`
- S'il existe, lis `posting.md` et `resume.md` de ce dossier
- Confirme avec l'utilisateur pour quelle offre c'est
- Sans dossier, demande l'adresse d'une offre (ou une entreprise pour une candidature spontanée)

**Si `$ARGUMENTS` est une adresse d'offre :**
- Vérifie si un dossier existe déjà pour cette entreprise dans `DATA_DIR/jobs/`
- Si oui, lis `posting.md` et `resume.md`
- Sinon, récupère l'offre avec Claude in Chrome selon `shared/references/browser-setup.md`
- Enregistre l'offre dans `DATA_DIR/jobs/[entreprise-slug]-[date]/posting.md`

Si la page ne charge pas, demande à l'utilisateur de coller l'offre.

#### Mode spontané

0. **Cherche l'entreprise dans le tracker** (`shared/references/tracker.md`). Si une candidature est déjà en cours, dis-le à l'utilisateur avant de rédiger.
1. **Identifie l'entreprise** : nom exact, site officiel, établissement le plus proche de la ville de résidence (adresse du destinataire).
2. **Recherche des faits publics et vérifiables** sur l'entreprise : ce qu'elle fait (produits, secteur), technologies ou métiers publiés (page carrières, blog technique, offres liées), projets ou actualités **datés et sourcés**, page alternance ou carrières, et le nom d'un contact seulement s'il est publié officiellement. Utilise la recherche web ou Claude in Chrome selon `shared/references/browser-setup.md` (jamais `get_page_text` sur une page volumineuse). Retiens 4 à 6 faits utiles, pas plus.
3. **Enregistre-les** dans `DATA_DIR/jobs/[entreprise-slug]-[date]/company.md` : chaque fait avec son adresse source. N'écris aucun fait sans source.
4. **Pose une seule question à l'utilisateur** : « Quel poste ou domaine visez-vous dans cette entreprise, et qu'est-ce qui vous attire chez eux ? Dites-le avec vos mots. » Sa réponse fournit le « pourquoi cette entreprise » : sans elle, la lettre serait générique. Sans réponse utilisable, reprends le domaine des préférences et dis-le.
5. Si les faits trouvés sont insuffisants, dis-le et demande à l'utilisateur de coller ce qu'il sait de l'entreprise.

### Étape 2 : Réunir les documents

Dans le dossier de l'offre ou de l'entreprise, vérifie ce qui existe :
- Mode offre : `posting.md` (obligatoire), `resume.md` (CV adapté, facultatif mais améliore beaucoup la qualité)
- Mode spontané : `company.md` (obligatoire), CV adapté facultatif

Sans CV adapté, utilise le CV d'origine et le profil du parcours.

### Étape 3 : Rédiger la lettre

Suis le cadre de `scripts/write-cover-letter.md` (section « Mode candidature spontanée » en mode spontané). Utilise :
- Le profil du parcours (ou le CV d'origine sans profil)
- Le CV adapté (s'il existe)
- L'offre (mode offre) ou `company.md` et la réponse de l'utilisateur (mode spontané)

Lis aussi `DATA_DIR/preferences.md` pour la ville de résidence (« Fait à [ville] »), l'école, le diplôme, le rythme, la durée et la date de début. Pour une alternance : rythme, durée, date de début. Pour un stage : durée, période, convention de stage avec l'école. Si une de ces informations manque, demande-la à l'utilisateur ou ne l'écris pas.

La lettre doit :
- Être en français, avec un corps de 200 à 380 mots (1 page)
- Avoir un en-tête (expéditeur, destinataire, lieu et date) et un objet (« Objet : Candidature en alternance - [poste] » ou « Objet : Candidature de stage - [poste] » selon l'offre ; en spontané, « alternance ou stage » si les préférences acceptent les deux)
- Commencer par « Madame, Monsieur, » (ou le nom du recruteur s'il figure dans l'offre), jamais « Dear Hiring Manager »
- Finir par une formule de politesse française sobre, puis le nom
- N'utiliser que des traits d'union, jamais de tirets cadratins
- Suivre le ton classique et chaleureux du modèle du candidat (`DATA_DIR/lettre-modele-ton.md`), sans rien inventer
- Ne rien inventer ni exagérer
- Relier 2 ou 3 réalisations concrètes (projets, stages, alternances, jobs) aux besoins de l'offre, avec des chiffres seulement s'ils existent

### Étape 4 : Présenter et enregistrer

1. Enregistre la lettre dans `DATA_DIR/jobs/[entreprise-slug]-[date]/cover-letter.md` (copie de travail).

2. **Archive-la dans le dossier des lettres.** Lis la rubrique « Archivage » de `DATA_DIR/preferences.md` (valeur par défaut : `~/Documents/Candidatures/Lettres`). Crée, sans jamais écraser une archive existante (ajoute `-2` en cas de doublon), le sous-dossier `AAAA-MM-JJ_Entreprise_offre` ou `AAAA-MM-JJ_Entreprise_spontanee` (entreprise sans accents ni espaces) avec :
   - `lettre.md` : la lettre retenue. Tant que l'utilisateur n'a pas choisi, enregistre `lettre-brouillon.md`, puis renomme-la à la validation.
   - `lettre.pdf` : la lettre mise en page (A4, 1 page), créée avec Chrome en mode headless à partir d'un HTML simple (`google-chrome --headless --no-sandbox --no-pdf-header-footer --print-to-pdf=...`). Écris le HTML intermédiaire dans un dossier temporaire, **pas dans l'archive**, ou supprime-le une fois le PDF créé. Vérifie que le PDF tient sur 1 page.
   - `sources-entreprise.md` : `company.md` (mode spontané) ou `posting.md` (mode offre).
   - `cv-adapte.md` : le CV adapté, s'il existe.
   - `variantes/` : les autres versions écrites mais non retenues.

3. **Mets à jour le suivi** (`shared/references/tracker.md`) : cherche d'abord si une ligne existe déjà pour cette entreprise, sinon crée-la. Mets le statut à `sent` et la date d'envoi seulement quand l'utilisateur confirme l'envoi (avant : `todo`), puis la relance 1 = date d'envoi + 7 jours. Ajoute une note datée avec le chemin de l'archive et la version du CV joint.

4. **Avant l'envoi, vérifie la cohérence avec le CV joint** (technologies, liens, dates). Signale à l'utilisateur toute contradiction.

Présente la lettre à l'utilisateur, en français, avec :
- Le texte complet
- Une courte note sur les réalisations mises en avant et pourquoi
- Le chemin du fichier enregistré
- En mode spontané : la liste des faits sur l'entreprise utilisés, avec leur source, pour que l'utilisateur les vérifie, et un rappel : une lettre adressée à un responsable technique nommé (repéré sur LinkedIn ou le site) marche mieux qu'une adresse générique

### Étape 5 : Ajuster

Demande si l'utilisateur veut ajuster :
- Le ton (plus formel, plus direct, plus technique)
- Les réalisations mises en avant
- Des formulations précises
- La longueur

Applique les changements, enregistre à nouveau et **mets à jour `lettre.md` (et `lettre.pdf`) dans l'archive**.


---

## Format de réponse

Structure la réponse avec ces sections :

1. **Lettre de motivation** : le texte complet
2. **Notes de rédaction** : réalisations mises en avant et pourquoi, arbitrages faits
3. **Et ensuite** : proposer d'ajuster le ton ou les points mis en avant, ou d'utiliser les autres skills

---

## Autorisations nécessaires

Ajoute à `~/.claude/settings.json` :

```json
{
  "permissions": {
    "allow": [
      "Read(~/.claude/skills/**)",
      "Read(~/.candidatures/**)",
      "Write(~/.candidatures/**)",
      "Edit(~/.candidatures/**)",
      "mcp__claude-in-chrome__*"
    ]
  }
}
```
