# Critères d'adéquation (alternance, France)

Barème commun pour évaluer l'adéquation entre une offre et le candidat. Applique aussi `shared/references/marche-france.md`.

## Processus d'évaluation

### 1. Vérifier d'abord les rédhibitoires

Classe immédiatement l'offre en **Skip** si UN des rédhibitoires de `preferences.md` est présent :
- Type d'entreprise ou de mission exclu
- Lieu hors de la zone ou du rayon accepté
- Offre qui n'est ni une alternance ni un stage (CDI, CDD), quand les préférences acceptent l'alternance et le stage
- Niveau de diplôme demandé incompatible avec la formation du candidat
- Alternance : rythme incompatible avec celui de l'école, ou date de début à plus de 3 mois de la date souhaitée
- Stage : durée ou période incompatibles avec ce que l'école autorise ou avec les préférences

### 2. Noter les indispensables

Compte les critères indispensables de `preferences.md` remplis :
- Tous remplis : passer aux appréciés
- La plupart : adéquation Medium possible
- Peu : adéquation Low probable

Pour un stage dont l'offre ne dit rien d'un rythme alterné avec l'école, note « rythme alterné possible ? » dans les notes et n'écarte pas l'offre. Alternance et stage sont au **même niveau** : un stage compatible n'est jamais noté plus bas qu'une alternance équivalente. Parmi les indispensables courants : durée compatible, rythme compatible (alternance) ou période compatible (stage), rayon autour de la ville de résidence, domaine visé.

### 3. Noter les appréciés

Pour les offres qui passent les indispensables :
- **Stack maîtrisée** (compétences ou outils du CV confirmés par le candidat : langages pour un développeur, logiciels pour d'autres métiers) présente dans l'offre
- **Stack souhaitée** (Rust, Go, etc.) présente dans l'offre : bonus, mais jamais comptée comme compétence acquise
- Domaine d'intérêt (systèmes embarqués, réseau, énergie, environnement, etc.)
- Encadrement et possibilité de poursuite en CDI
- Taille et type d'entreprise selon les préférences

### 4. Attribuer la note

| Note | Critères |
|------|----------|
| **High** | Aucun rédhibitoire + tous les indispensables + au moins 2 appréciés |
| **Medium** | Aucun rédhibitoire + la plupart des indispensables, OU tous les indispensables mais peu d'appréciés |
| **Low** | Aucun rédhibitoire mais des écarts importants sur les indispensables |
| **Skip** | Au moins un rédhibitoire |

## Règle sur la stack

Une technologie que le candidat n'a jamais pratiquée (par exemple Rust ou Go) ne rend pas une offre mauvaise : si elle est affichée comme « souhaitée » dans les préférences, c'est un point en faveur. Mais elle ne doit jamais être présentée comme une compétence acquise dans le CV ou la lettre.
