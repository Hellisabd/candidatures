---
name: job-search
description: Recherche d'offres d'alternance et de stage en France correspondant à votre CV et à vos préférences (La Bonne Alternance, Welcome to the Jungle, LinkedIn, Indeed)
argument-hint: "mot-clé de recherche"
---

# Skill de recherche d'offres (alternance et stage, France)

> **Priority hierarchy**: See `shared/references/priority-hierarchy.md` for conflict resolution.

Recherche d'offres d'alternance **et de stage** par automatisation du navigateur, sur des sites français. Les deux sont au même niveau de priorité si les préférences les acceptent.

## Langue et marché

Tout est en **français**. Lis `shared/references/marche-france.md` avant de commencer : il l'emporte sur toute consigne américaine restante.

## Quick Start

- `/candidatures:job-search` : recherche avec les termes par défaut tirés de vos préférences
- `/candidatures:job-search Rust` : recherche avec un mot-clé précis

## File Structure

```
scripts/
  evaluate-jobs.md     # Sous-agent d'évaluation des offres
assets/
  templates/           # Modèles de format
```

## Data Directory

Resolve the data directory using `shared/references/data-directory.md`.

---

## Sources de recherche

Dans cet ordre de priorité. Passe à la source suivante seulement si la précédente donne peu de résultats ou est inaccessible.

1. **La Bonne Alternance** : `https://labonnealternance.apprentissage.beta.gouv.fr/recherche`. Service officiel de l'État, gratuit, sans connexion. Filtres par métier, lieu et rayon. Il regroupe des offres de France Travail, d'Indeed et d'employeurs directs. **Il ne liste que des alternances, pas de stages.**
2. **Welcome to the Jungle** : `https://www.welcometothejungle.com/fr/jobs`. Filtrer sur le type de contrat « alternance » ou « stage » et la localisation.
3. **LinkedIn Jobs** : `https://www.linkedin.com/jobs/`. Utiliser uniquement la session déjà ouverte de l'utilisateur. Ne jamais saisir d'identifiants. Filtrer sur le lieu et les mots-clés « alternance » ou « stage ».
4. **Indeed France** : `https://fr.indeed.com`. Mots-clés « alternance » ou « stage » + métier, lieu et rayon.

*JobTeaser* demande une connexion via l'école et est très utilisé pour les stages : à ajouter seulement si l'utilisateur le demande.

**Pour les stages**, utilise les sources 2 à 4 (La Bonne Alternance n'en liste pas).

**Règles de prudence sur les sites :**
- Les adresses de recherche et les filtres changent : ouvre la page, utilise les filtres de l'interface, puis réutilise l'adresse obtenue pour les recherches suivantes.
- Reste raisonnable : 30 résultats par recherche au maximum, pas de rafale de requêtes.
- Page de connexion, captcha ou blocage : n'insiste pas. Dis-le à l'utilisateur et passe à la source suivante.
- Tu ne crées aucun compte et ne postules à rien dans ce skill.

---

## Workflow

### Step 0: Vérifier les prérequis

Résous le dossier de données, puis vérifie les prérequis selon `shared/references/prerequisites.md`. Le CV et les préférences sont obligatoires.

### Step 1: Charger le contexte

Lis :
- `DATA_DIR/resume/*` (CV)
- `DATA_DIR/preferences.md` (préférences, dont stack, rythme, durée, date de début, ville et rayon)
- `DATA_DIR/job-history.md` (pour éviter les doublons)
- `DATA_DIR/linkedin-contacts.csv` (s'il existe, pour repérer les contacts dans les entreprises)

Termes de recherche, dans cet ordre :
1. `$ARGUMENTS` s'il est fourni
2. Postes visés et stack recherchée dans les préférences, combinés avec « alternance » **et** « stage » quand les deux sont acceptés (ex. « alternance développeur C++ », « stage développeur C++ », « stage systèmes embarqués », « alternance Rust »)

### Step 2: Recherche dans le navigateur

Utilise Claude in Chrome selon `shared/references/browser-setup.md`. Pour chaque terme et chaque source, applique les filtres : alternance et/ou stage selon les préférences, localisation (ville de résidence + rayon des préférences), date de publication récente si disponible.

**Extraction des résultats : IMPORTANT.** N'utilise JAMAIS `get_page_text` sur une page de résultats : elle renvoie toute la page et sature le contexte.

Extrais plutôt les offres avec `javascript_tool`, en ne récupérant que les données structurées :

```javascript
// Extraire les offres visibles (à adapter au site)
Array.from(document.querySelectorAll('[class*="job"], [class*="offer"], [class*="card"], article, li'))
  .slice(0, 30)
  .map(el => el.innerText.trim())
  .filter(t => t.length > 20 && t.length < 500)
  .join('\n---\n')
```

Si le sélecteur ne correspond pas, prends une capture pour comprendre la structure, puis écris un sélecteur ciblé pour ce site. Le but est d'extraire uniquement les lignes d'offres (intitulé, entreprise, lieu, contrat, date), jamais la page entière. En dernier recours, `read_page` (jamais `get_page_text`).

**Note :** Pour chaque offre, relève aussi quand c'est affiché : le type de contrat (alternance ou stage), la durée, la date de début ou la période, le rythme d'alternance, le niveau de diplôme demandé.

### Step 3: Évaluer les offres

Note chaque offre selon `shared/references/fit-scoring.md`, par rapport au CV et aux préférences.

### Step 4: Enregistrer l'historique

Ajoute TOUTES les offres à `DATA_DIR/job-history.md`, avec le format de `assets/templates/job-entry.md`.

### Step 5: Retrouver l'URL de l'employeur et enregistrer les meilleures offres

Pour chaque offre **High** :
1. Remonte jusqu'à l'offre d'origine (site carrière de l'entreprise, ou annonce France Travail ou Indeed si c'est la source)
2. Note l'URL directe de l'offre
3. Extrais la description avec `javascript_tool` (ex. `document.querySelector('[class*="description"], [class*="content"], article, main')?.innerText`). N'utilise PAS `get_page_text` : les pages d'entreprises ont de gros pieds de page et des offres liées qui saturent le contexte.
4. Enregistre dans `DATA_DIR/jobs/[entreprise-slug]-[date]/posting.md`, avec l'URL de l'offre en haut

Pour les offres **Medium**, essaie de retrouver l'URL mais n'enregistre pas toute l'offre.

Si tu ne retrouves pas le lien direct, note le nom de l'entreprise pour que l'utilisateur la retrouve. Ne montre jamais de lien d'agrégateur (La Bonne Alternance, Indeed, etc.) quand l'offre d'origine existe.

### Step 6: Présenter les résultats

Montre seulement les NOUVELLES offres High/Medium absentes de l'historique.

Si les contacts LinkedIn sont chargés, croise le nom de chaque entreprise avec la colonne « Company » du CSV (correspondance approximative : « Acme » correspond à « Acme S.A.S. »). En cas de correspondance, indique le nom et le poste du contact.

```markdown
## Meilleures offres du [DATE]

### 1. [Intitulé] chez [Entreprise]
- **Adéquation** : High
- **Contrat** : [alternance ou stage], [durée], rythme [ex. 4 sem. / 2 sem.] pour une alternance, début [mois et année]
- **Lieu** : [ville], à [X] km de [ville de résidence]
- **Stack** : [technologies de l'offre, avec celles du candidat et celles à apprendre]
- **Pourquoi** : [raison]
- **Réseau** : Vous connaissez [Prénom Nom] ([poste]) chez [Entreprise]
- **Postuler** : [URL directe]
```

Omets la ligne « Réseau » s'il n'y a aucun contact dans l'entreprise.

### Step 7: Étapes suivantes

Après les résultats, indique à l'utilisateur :
- Pour adapter le CV : `/candidatures:tailor-resume [URL de l'offre]`
- Pour rédiger la lettre : `/candidatures:cover-letter [URL de l'offre]`
- Pour déposer la candidature : `/candidatures:apply [URL de l'offre]` (plusieurs URL possibles, un seul récapitulatif à valider)

**IMPORTANT** : N'essaie PAS d'adapter un CV, d'écrire une lettre ou de remplir une candidature toi-même. Ce sont des skills distincts avec leurs propres consignes.

### Step 8: Apprendre des retours

Si l'utilisateur donne un retour, mets à jour `DATA_DIR/preferences.md` :
- « Pas d'ESN » : ajouter aux rédhibitoires
- « Je préfère l'embarqué » : ajouter aux appréciés
- « Pas plus de 40 km » : mettre à jour le rayon

---

## Response Format

1. **Meilleures offres** : liste des offres High/Medium avec entreprise, poste, adéquation, contrat et rythme, lieu, contacts du réseau et URL directe
2. **Étapes suivantes** : proposer `/candidatures:tailor-resume` et `/candidatures:cover-letter` pour les meilleures offres

---

## Permissions Required

Add to `~/.claude/settings.json`:

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
