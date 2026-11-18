---
name: tailor-resume
description: Adapte votre CV en français à une offre précise (alternance, stage, premier emploi)
argument-hint: "job URL"
---

# Resume Tailoring Skill

> **Priority hierarchy**: See `shared/references/priority-hierarchy.md` for conflict resolution.

Crée des CV français adaptés, sur 1 page, qui montrent clairement que vous êtes le bon candidat pour une offre précise.

## Quick Start

- `/candidatures:tailor-resume` - Start the flow (will ask for a job URL)
- `/candidatures:tailor-resume https://...` - Tailor resume for a specific job posting

## File Structure

```
scripts/
  tailor-resume.md        # Resume tailoring subagent prompt
```

The profile template is at `shared/templates/profile.md`.

## Langue et marché

Tout est en **français** : ce que tu dis à l'utilisateur et ce que tu écris dans les fichiers. Avant de commencer, lis `shared/references/marche-france.md` : il décrit le contexte français (alternance, CV 1 page, lettre française) et l'emporte sur toute consigne américaine restante.

## Data Directory

Resolve the data directory using `shared/references/data-directory.md`.

---

## Workflow

### Step 0: Check Prerequisites

Resolve the data directory, then check prerequisites per `shared/references/prerequisites.md`. Resume is required; profile is strongly recommended. If the user proceeds without a profile, set a flag to present all assumptions for verification (see Step 3a below).

If `$ARGUMENTS` is a URL, continue to Step 1.
Otherwise, ask for a job URL.

### Step 1: Get Job Details

Accept a job URL from the user (from `$ARGUMENTS` or by asking).

Use Claude in Chrome MCP tools to fetch the job posting per `shared/references/browser-setup.md`.

Parse and extract:
- **Job title** and level (IC vs. manager, seniority)
- **Company** name and what they do
- **Responsibilities** - what the job actually involves day-to-day
- **Requirements** - must-have qualifications
- **Nice-to-haves** - preferred qualifications
- **Keywords** - industry terms, tools, methodologies mentioned
- **Team context** - who they report to, team size, cross-functional partners
- **Company stage/size** indicators

**Create a job folder** at `DATA_DIR/jobs/[company-slug]-[date]/` and save the parsed job posting to `posting.md`.

If the page can't be loaded or parsed, ask the user to paste the job description directly.

### Step 2: Analyze Match

Before writing, map the candidate's experience to the job:

1. **Niveau et contrat** : confirme que l'offre correspond à la recherche du candidat (alternance, stage ou emploi, durée, rythme, localisation, niveau d'études demandé). Signale tout écart (offre en CDI alors que la recherche est une alternance, rythme incompatible, niveau Bac+5 demandé pour un Bac+3).

2. **Requirement mapping**: For each job requirement, identify the strongest evidence from the work history profile:
   - Direct experience ("Led SEO strategy" → job asks for SEO experience)
   - Analogous experience ("Scaled marketplace from 1M to 10M users" → job asks for growth experience)
   - Transferable skills ("Managed 30-person team" → job asks for leadership)

3. **Gap identification**: Note any requirements where the candidate has no clear match. These should NOT be fabricated - instead, find adjacent experience that demonstrates capability.

4. **Keyword alignment**: Identify the job posting's language and terminology to mirror in the resume.

5. **Compelling narrative**: Determine the 2-3 sentence story of why this person is the obvious choice. What's the throughline?

### Step 3: Generate Tailored Resume

Create the tailored resume following these principles:

**Structure (1 page, voir `shared/references/marche-france.md`) :**
- **En-tête** : nom, ville, e-mail, téléphone, LinkedIn, GitHub ou portfolio (comme l'original), puis une ligne de titre (poste visé, nature du contrat alternance ou stage, rythme et date de début pour une alternance, durée et période pour un stage) avec uniquement des informations fournies
- **Accroche** : 2 phrases qui positionnent le candidat pour CE poste, pas une phrase générique
- **Formation** : diplôme en cours en premier, avec les enseignements liés à l'offre s'ils figurent dans les sources
- **Expériences** : alternances, stages, emplois, avec des points réécrits, réordonnés et mis en avant selon l'offre
- **Projets** : 2 ou 3 projets les plus proches de l'offre
- **Compétences** : réorganisées pour commencer par ce que demande l'offre
- **Langues** : niveau CECRL ou test, si fourni
- **Centres d'intérêt et engagements** : 1 ligne, concrète

**Bullet point principles:**
- Lead each role with the bullets most relevant to the target job
- Rewrite bullets to mirror the job posting's language where authentic
- Include metrics and quantified impact (from work history profile)
- Remove or de-emphasize bullets that aren't relevant to this specific role
- Add bullets from the work history profile that weren't on the original resume but ARE relevant to this job
- Each bullet should start with a strong action verb
- Each bullet should show: what you did → how you did it → what the impact was

**Calibrage du niveau :**
- Alternance ou stage : mets en avant les projets concrets, les technologies pratiquées, la capacité à apprendre vite et le travail en équipe
- Première expérience (alternance précédente, CDD, CDI junior) : mets en avant l'autonomie, les livrables et l'impact mesurable
- Ne présente jamais un projet scolaire comme une mission d'entreprise

**Règles d'écriture (CRITIQUE : phrases courtes, mots simples, en français) :**
- Write like a sharp executive, not a language model. Short sentences. Plain words.
- Every sentence gets one idea. If a sentence has "and" connecting two unrelated clauses, split it.
- Never use emdashes. Use commas, periods, colons, semicolons, or parentheses instead.
- Vary sentence structure. Not every bullet should follow the exact same pattern.
- No preamble clauses. Bad: "Leveraging deep expertise in marketplace dynamics, led..." Good: "Led..."
- No stacking adjectives. Bad: "cross-functional, data-driven, customer-centric approach". Pick one.
- No filler phrases: "demonstrating ability to", "showcasing expertise in", "with a track record of", "needed to drive", "spanning", "leveraging", "utilizing"
- No compound noun piles: "AI-driven product opportunity identification and execution" — just say what you did
- Summaries must be 2-3 SHORT sentences. Each sentence under 20 words. No run-on sentences connecting multiple capabilities with commas and "and".

**Strict accuracy rules (CRITICAL):**
- ONLY use information explicitly stated on the resume or in the work history profile
- NEVER assume business model (B2B vs B2C), revenue type, or company stage unless stated
- NEVER infer scope beyond what's written (e.g., don't add "P&L ownership" if resume says "revenue targets")
- NEVER add responsibilities, skills, or functional areas the candidate didn't mention
- NEVER assume cross-functional partnerships that aren't listed
- When the resume is ambiguous, use conservative language or omit the detail entirely
- If you need to frame experience differently for the target role, only reframe what IS there, never invent what ISN'T

**What NOT to do:**
- Don't fabricate experience or skills the candidate doesn't have
- Don't use generic buzzwords that aren't backed by specific experience
- Ne dépasse pas 1 page
- Don't change job titles or dates
- Ne retire ni la formation ni les alternances ou stages (les trous se remarquent). Les petits jobs peuvent être réduits à une ligne pour tenir sur 1 page, en le signalant
- Don't assume anything about the candidate's business, scope, or responsibilities that isn't explicitly documented

### Step 3b: Critique and Rewrite (MANDATORY — do this before presenting)

Before showing the resume to the user, review every line and fix AI-sounding writing. Go sentence by sentence and ask:

1. **Is this sentence doing too much?** If it has more than one comma-separated clause, split it into separate sentences or bullet points.
2. **Would a real person say this?** Read it out loud. If it sounds like a LinkedIn post or a ChatGPT response, rewrite it.
3. **Is there filler?** Cut any phrase that doesn't add information. "Demonstrating ability to identify and execute on AI-driven product opportunities from ideation through production" → "Built an AI product from idea to production."
4. **Are there stacked buzzwords?** "Cross-functional, data-driven, customer-centric leadership" → pick the one that matters for this job and give a concrete example.
5. **Is the summary under control?** Max 3 sentences. Each under 20 words. No sentence should list more than 2 things.

**Common AI patterns to kill:**
- "I combine X with Y, Z, and the W needed to..." → Split into separate statements
- "...demonstrating [abstract quality]" → Delete or replace with the actual result
- "...spanning [long list]" → Pick the most relevant 1-2 items
- "Led [action], [action], and [action] across [scope]" → One action per bullet
- Any bullet over 2 lines is probably trying to do too much — split it
- Gerund clauses tacked onto the end: "...delivering X while maintaining Y" → Two sentences

**Test:** After rewriting, re-read the summary and first 3 bullets. If any sentence takes more than one breath to read out loud, it's too long. Shorten it.

**Output:**

Save the tailored resume to `DATA_DIR/jobs/[company-slug]-[date]/resume.md`

Present the resume to the user with a brief explanation:

```
Voici votre CV adapté pour [Poste] chez [Entreprise].

**Principales modifications :**
- [Ce qui a été réordonné ou mis en avant, et pourquoi]
- [Ce qui a été reformulé, et pourquoi]
- [Ce qui a été ajouté à partir de votre profil]

**Le fil conducteur :** [2-3 phrases : pourquoi vous êtes le bon candidat]

Le CV est enregistré dans : DATA_DIR/jobs/[dossier]/resume.md
```

### Step 3a: Verify Assumptions (if no profile exists)

If no work history profile was available, present the user with a list of every assumption made:

```
Avant de finaliser, voici les hypothèses que j'ai faites. Corrigez
celles qui sont fausses :

1. [Structure] : j'ai supposé [X]. C'est exact ?
2. [Périmètre de la mission] : j'ai décrit votre périmètre ainsi : [Y]. Exact ?
3. [Technologies ou contexte] : j'ai présenté cela comme [Z]. Correct ?
...
```

Wait for the user to verify or correct before finalizing. Apply all corrections to the resume AND save them to `DATA_DIR/profile.md` so they persist.

### Step 4: Iterate

Demande si l'utilisateur veut ajuster quelque chose :
- Le ton (plus technique, plus orienté résultats)
- L'accent (mettre davantage en avant certaines expériences ou compétences)
- La longueur (tenir sur 1 page, détailler certains points)
- Des points précis à reformuler

Apply changes and re-save.


### Step 5: Update Profile (ALWAYS)

**Every time the user corrects a factual detail**, update `DATA_DIR/profile.md` immediately:
- Corrections sur la structure (ex. « l'entreprise est une ESN, pas un éditeur de logiciel »)
- Corrections de périmètre (ex. « j'ai participé au développement, je ne l'ai pas dirigé »)
- Corrections de responsabilités (ex. « je n'ai pas géré le déploiement »)
- Any other clarification about roles, teams, or accomplishments

This prevents the same mistakes on future resumes. If the profile is still a blank template, create a new one with whatever the user has told you so far. Use the structure from `shared/templates/profile.md` but fill in only what you know for certain.

---

## Response Format

Structure user-facing output with these sections:

1. **Tailored Resume** — the full resume text
2. **Tailoring Notes** — key changes made (reordered bullets, rewritten sections, added content from profile) and the narrative pitch
3. **What's Next** — suggest iterating on tone/emphasis, or writing a cover letter with `/candidatures:cover-letter`

---

## Permissions Required

Add to `~/.claude/settings.json`:

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
