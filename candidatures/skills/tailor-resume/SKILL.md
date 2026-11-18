---
name: tailor-resume
description: Adapte votre CV en français à une offre précise (alternance, stage, premier emploi)
argument-hint: "URL de l'offre, ou texte de l'offre collé"
---

# Skill d'adaptation du CV

> **Hiérarchie des priorités** : voir `shared/references/priority-hierarchy.md` pour résoudre les conflits.

Crée des CV français adaptés, sur 1 page, qui montrent clairement que vous êtes le bon candidat pour une offre précise.

## Démarrage rapide

- `/candidatures:tailor-resume` : démarre (demande l'offre)
- `/candidatures:tailor-resume https://...` : adapte le CV à une offre précise
- On peut aussi coller directement le texte de l'offre

## Fichiers

```
scripts/
  tailor-resume.md        # Consignes de l'agent d'adaptation du CV
```

Le modèle de profil est dans `shared/templates/profile.md`.

## Langue et marché

Tout est en **français** : ce que tu dis à l'utilisateur et ce que tu écris dans les fichiers. Avant de commencer, lis `shared/references/marche-france.md` : il décrit le contexte français (alternance, CV 1 page, lettre française) et l'emporte sur toute consigne américaine restante.

## Dossier de données

Résous le dossier de données avec `shared/references/data-directory.md`.

---

## Workflow

### Étape 0 : Vérifier les prérequis

Résous le dossier de données, puis vérifie les prérequis selon `shared/references/prerequisites.md`. Le CV est obligatoire ; le profil est fortement recommandé. Si l'utilisateur continue sans profil, retiens qu'il faudra lui présenter toutes les hypothèses à vérifier (étape 3c).

Si `$ARGUMENTS` est l'adresse ou le texte d'une offre, passe à l'étape 1. Sinon, demande l'offre.

### Étape 1 : Lire l'offre

Accepte l'adresse d'une offre ou son texte collé par l'utilisateur.

Pour une adresse, utilise Claude in Chrome selon `shared/references/browser-setup.md`. Si la page ne charge pas ou n'est pas lisible, demande à l'utilisateur de coller le texte de l'offre.

Relève :
- **Intitulé** et niveau (stage, alternance, junior)
- **Entreprise** et ce qu'elle fait
- **Missions** : ce que le poste implique au quotidien
- **Exigences** : qualifications indispensables
- **Souhaités** : qualifications appréciées
- **Mots-clés** : termes du métier, outils, méthodes cités
- **Contexte d'équipe** : encadrement, taille de l'équipe, interlocuteurs
- **Contrat visé** : alternance ou stage, durée, rythme, date de début
- **Taille et stade** de l'entreprise, s'ils sont indiqués

**Crée le dossier de l'offre** `DATA_DIR/jobs/[entreprise-slug]-[date]/` et enregistre l'offre analysée dans `posting.md`.

### Étape 2 : Analyser l'adéquation

Avant d'écrire, rapproche le parcours du candidat de l'offre :

1. **Niveau et contrat** : confirme que l'offre correspond à la recherche du candidat (alternance, stage ou emploi, durée, rythme, localisation, niveau d'études demandé). Signale tout écart (offre en CDI alors que la recherche est une alternance, rythme incompatible, niveau Bac+5 demandé pour un Bac+3).

2. **Correspondance avec les exigences** : pour chaque exigence de l'offre, trouve la preuve la plus solide dans le profil :
   - Expérience directe (« a développé une API REST en TypeScript » → l'offre demande du back-end TypeScript)
   - Expérience proche (« a écrit un serveur HTTP en C++ » → l'offre demande de la programmation réseau)
   - Compétence transférable (« a tenu seul le calendrier de publication d'un compte » → l'offre demande de l'organisation)

3. **Écarts** : note les exigences sans correspondance claire. Ne les invente PAS : cherche plutôt une expérience voisine qui montre la capacité.

4. **Mots-clés** : repère le vocabulaire de l'offre à reprendre dans le CV, quand il est exact.

5. **Fil conducteur** : formule en 2 ou 3 phrases pourquoi cette personne est le bon choix. Quel est le lien entre ses expériences ?

### Étape 3 : Générer le CV adapté

Crée le CV adapté en suivant ces principes :

**Structure (1 page, voir `shared/references/marche-france.md`) :**
- **En-tête** : nom, ville, e-mail, téléphone, LinkedIn, GitHub ou portfolio (comme l'original), puis une ligne de titre (poste visé, nature du contrat alternance ou stage, rythme et date de début pour une alternance, durée et période pour un stage) avec uniquement des informations fournies
- **Accroche** : 2 phrases qui positionnent le candidat pour CE poste, pas une phrase générique
- **Formation** : diplôme en cours en premier, avec les enseignements liés à l'offre s'ils figurent dans les sources
- **Expériences** : alternances, stages, emplois, avec des points réécrits, réordonnés et mis en avant selon l'offre
- **Projets** : 2 ou 3 projets les plus proches de l'offre
- **Compétences** : réorganisées pour commencer par ce que demande l'offre
- **Langues** : niveau CECRL ou test, si fourni
- **Centres d'intérêt et engagements** : 1 ligne, concrète

**Principes pour les points :**
- Commence chaque expérience par les points les plus pertinents pour l'offre
- Reformule les points avec le vocabulaire de l'offre quand il est exact
- Ajoute des chiffres et des résultats mesurables (tirés du profil), seulement s'ils existent
- Retire ou atténue les points sans rapport avec ce poste
- Ajoute les points du profil absents du CV d'origine mais utiles pour cette offre
- Chaque point commence par un verbe d'action
- Chaque point montre : ce que tu as fait, comment, avec quel résultat

**Calibrage du niveau :**
- Alternance ou stage : mets en avant les projets concrets, les technologies pratiquées, la capacité à apprendre vite et le travail en équipe
- Première expérience (alternance précédente, CDD, CDI junior) : mets en avant l'autonomie, les livrables et l'impact mesurable
- Ne présente jamais un projet scolaire comme une mission d'entreprise

**Règles d'écriture (CRITIQUE : phrases courtes, mots simples, en français) :**
- Écris comme une personne claire et directe, pas comme un modèle de langage. Phrases courtes, mots simples.
- Une idée par phrase. Si une phrase relie deux idées sans rapport par « et », coupe-la.
- Jamais de tirets cadratins ni demi-cadratins. Utilise la virgule, le point, les deux-points, le point-virgule ou les parenthèses.
- Varie la structure des phrases : tous les points ne suivent pas le même schéma.
- Pas de proposition d'introduction. Mauvais : « Fort d'une solide expertise en développement, j'ai conçu... ». Bon : « Conçu... ».
- Pas d'adjectifs empilés. Mauvais : « une approche transverse, orientée données et centrée client ». Choisis-en un.
- Pas de phrases creuses : « en démontrant une capacité à », « en mettant en avant une expertise de », « fort d'une expérience de », « afin de piloter », « s'étendant sur », « en tirant parti de », « en utilisant »
- Pas d'empilement de noms : « identification et exécution d'opportunités produit pilotées par l'IA » : dis simplement ce que tu as fait
- L'accroche : 2 phrases COURTES, chacune de moins de 20 mots, sans phrase fleuve qui enchaîne plusieurs compétences par des virgules et des « et ».

**Règles strictes d'exactitude (CRITIQUE) :**
- N'utilise QUE des informations explicitement écrites dans le CV ou dans le profil
- Ne suppose JAMAIS le modèle économique (B2B ou B2C), le type de revenus ou la taille de l'entreprise, sauf s'ils sont indiqués
- N'étends JAMAIS le périmètre au-delà de ce qui est écrit (par exemple « participé au développement » ne devient pas « responsable du développement »)
- N'ajoute JAMAIS une responsabilité, une compétence ou un domaine que le candidat n'a pas mentionné
- Ne suppose JAMAIS un partenariat avec d'autres équipes s'il n'est pas listé
- Quand le CV est ambigu, utilise un langage prudent ou omets le détail
- Pour présenter une expérience autrement, reformule seulement ce qui EST là, n'invente jamais ce qui n'y est PAS

**À ne pas faire :**
- Ne fabrique pas d'expérience ni de compétence que le candidat n'a pas
- N'utilise pas de grands mots creux qu'aucune expérience précise ne soutient
- Ne dépasse pas 1 page
- Ne change ni les intitulés ni les dates
- Ne retire ni la formation ni les alternances ou stages (les trous se remarquent). Les petits jobs peuvent être réduits à une ligne pour tenir sur 1 page, en le signalant
- Ne suppose rien sur l'entreprise, le périmètre ou les responsabilités du candidat qui ne soit pas explicitement documenté

### Étape 3b : Relecture critique (OBLIGATOIRE, avant de présenter)

Avant de montrer le CV à l'utilisateur, relis chaque ligne et corrige tout ce qui sonne comme généré. Phrase par phrase, demande-toi :

1. **Cette phrase en fait-elle trop ?** Si elle contient plus d'une proposition séparée par des virgules, coupe-la en phrases ou en points distincts.
2. **Une vraie personne dirait-elle cela ?** Lis-la à voix haute. Si elle sonne comme un post LinkedIn ou une réponse de chatbot, réécris-la.
3. **Y a-t-il du remplissage ?** Supprime toute expression qui n'apporte pas d'information. « En démontrant une capacité à identifier et mener des projets d'IA de l'idée à la production » devient « A construit un produit d'IA, de l'idée à la mise en ligne ».
4. **Y a-t-il des mots-clés empilés ?** « Leadership transverse, orienté données et centré client » : garde celui qui compte pour cette offre et donne un exemple concret.
5. **L'accroche est-elle maîtrisée ?** 2 phrases maximum, moins de 20 mots chacune, jamais plus de 2 choses énumérées.

**Tournures de texte généré à supprimer :**
- « Je combine X avec Y, Z et le W nécessaire pour... » : sépare en phrases distinctes
- « ...démontrant [qualité abstraite] » : supprime ou remplace par le résultat réel
- « ...couvrant [longue liste] » : garde les 1 ou 2 éléments les plus pertinents
- « A piloté [action], [action] et [action] sur [périmètre] » : une action par point
- Un point de plus de 2 lignes essaie probablement d'en faire trop : coupe-le
- Un gérondif accroché en fin de phrase : « ...en livrant X tout en maintenant Y » : deux phrases

**Test :** après réécriture, relis l'accroche et les 3 premiers points. Si une phrase demande plus d'une respiration à voix haute, elle est trop longue : raccourcis-la.

**Sortie :**

Enregistre le CV adapté dans `DATA_DIR/jobs/[entreprise-slug]-[date]/resume.md`.

Présente le CV à l'utilisateur avec une courte explication :

```
Voici votre CV adapté pour [Poste] chez [Entreprise].

**Principales modifications :**
- [Ce qui a été réordonné ou mis en avant, et pourquoi]
- [Ce qui a été reformulé, et pourquoi]
- [Ce qui a été ajouté à partir de votre profil]

**Le fil conducteur :** [2-3 phrases : pourquoi vous êtes le bon candidat]

Le CV est enregistré dans : DATA_DIR/jobs/[dossier]/resume.md
```

### Étape 3c : Vérifier les hypothèses (si aucun profil)

Si aucun profil du parcours n'était disponible, présente à l'utilisateur la liste de toutes les hypothèses faites :

```
Avant de finaliser, voici les hypothèses que j'ai faites. Corrigez
celles qui sont fausses :

1. [Structure] : j'ai supposé [X]. C'est exact ?
2. [Périmètre de la mission] : j'ai décrit votre périmètre ainsi : [Y]. Exact ?
3. [Technologies ou contexte] : j'ai présenté cela comme [Z]. Correct ?
...
```

Attends que l'utilisateur vérifie ou corrige avant de finaliser. Applique toutes les corrections au CV ET enregistre-les dans `DATA_DIR/profile.md` pour qu'elles durent.

### Étape 4 : Ajuster

Demande si l'utilisateur veut ajuster quelque chose :
- Le ton (plus technique, plus orienté résultats)
- L'accent (mettre davantage en avant certaines expériences ou compétences)
- La longueur (tenir sur 1 page, détailler certains points)
- Des points précis à reformuler

Applique les changements et enregistre à nouveau.

### Étape 5 : Mettre à jour le profil (TOUJOURS)

**Chaque fois que l'utilisateur corrige un détail factuel**, mets à jour `DATA_DIR/profile.md` immédiatement :
- Corrections sur la structure (ex. « l'entreprise est une ESN, pas un éditeur de logiciel »)
- Corrections de périmètre (ex. « j'ai participé au développement, je ne l'ai pas dirigé »)
- Corrections de responsabilités (ex. « je n'ai pas géré le déploiement »)
- Toute autre précision sur les rôles, les équipes ou les réalisations

Cela évite de refaire les mêmes erreurs sur les CV suivants. Si le profil est encore un modèle vide, crée-en un nouveau avec ce que l'utilisateur t'a dit jusque-là. Utilise la structure de `shared/templates/profile.md`, mais ne remplis que ce que tu sais avec certitude.

---

## Format de réponse

Structure la réponse avec ces sections :

1. **CV adapté** : le texte complet du CV
2. **Notes d'adaptation** : les principaux changements (points réordonnés, parties réécrites, éléments ajoutés depuis le profil) et le fil conducteur
3. **Et ensuite** : proposer d'ajuster le ton ou l'accent, ou de rédiger une lettre avec `/candidatures:cover-letter`

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
