# candidatures

Plugin [Claude Code](https://claude.ai/code) pour candidater en France à une **alternance ou à un stage** : un profil construit avec toi, des CV adaptés, des lettres de motivation (sur offre ou spontanées), la recherche d'offres et l'aide au remplissage des formulaires.

Tout est en français et suit les usages français : CV d'une page, lettre au bon format, vocabulaire de l'alternance et du stage, candidature spontanée.

## Ce que ça fait

| Commande | Rôle |
|---|---|
| `/candidatures:setup` | À faire une fois : ton CV, tes préférences, une interview sur ton parcours, le ton de tes lettres |
| `/candidatures:cover-letter [offre ou spontanée entreprise]` | Lettre de motivation, archivée dans un dossier avec son suivi |
| `/candidatures:tailor-resume [offre]` | CV adapté à une offre |
| `/candidatures:job-search` | Recherche d'offres d'alternance et de stage (La Bonne Alternance, Welcome to the Jungle, LinkedIn, Indeed) |
| `/candidatures:apply [offres]` | Candidature (LinkedIn, site carrière ou mail) avec ta validation avant chaque envoi |

## Prérequis

- Claude Code.
- Pour `job-search` et `apply` : l'extension **Claude in Chrome** (ces deux commandes pilotent le navigateur). `setup`, `cover-letter` et `tailor-resume` n'en ont pas besoin.

## Installation

Depuis GitHub (remplace `<compte>/<depot>`) :

```
/plugin marketplace add <compte>/<depot>
/plugin install candidatures@mes-plugins
```

Ou depuis un dossier sur ton disque :

```
claude plugin marketplace add /chemin/vers/claude-plugins
claude plugin install candidatures@mes-plugins
```

Puis `/reload-plugins`, et lance `/candidatures:setup`.

## Tes données

Elles restent **sur ton ordinateur**, dans `~/.candidatures/` : préférences, profil, CV, réponses types aux formulaires, offres enregistrées et suivi. Elles ne font pas partie du plugin. Le plugin n'envoie rien de lui-même : chaque candidature demande ton « oui » avant l'envoi.

## Le suivi des candidatures

Par défaut, un simple fichier `~/.candidatures/suivi.md` (une ligne par candidature : statut, relance, contact, notes). Pas besoin de plus.

*Option pour utilisateurs avancés :* le suivi peut être tenu dans un artefact Claude avec une base de données (collection `candidatures`). Il suffit de l'indiquer dans la rubrique « Suivi des candidatures » de `preferences.md`. Les conventions sont dans `candidatures/shared/references/tracker.md`.

## Le ton de tes lettres

Au setup, tu choisis : **classique et chaleureux**, **direct et sobre**, ou ton propre style (tu peux coller une lettre dont tu aimes le ton). Dans tous les cas, la règle ne change pas : rien d'inventé, ton rôle réel dans chaque projet, et aucune compétence présentée comme acquise si tu ne l'as jamais pratiquée.

## Limites

- Fait pour la **France**.
- Conçu et testé par son auteur, d'abord pour un profil d'informatique. Les consignes sont générales, mais les exemples et les sites de recherche n'ont pas été éprouvés sur d'autres métiers.
- Les sites de recherche d'offres changent : certains peuvent bloquer l'automatisation. Le plugin s'arrête alors et passe à la source suivante.

## Crédits et licence

Licence MIT (voir `LICENSE` et `NOTICE`). Plugin dérivé d'un projet open source, largement réécrit pour le marché français.
