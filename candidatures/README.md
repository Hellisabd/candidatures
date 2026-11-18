# candidatures

Plugin Claude Code pour candidater en France (alternance et stage). Voir le README à la racine du dépôt pour l'installation, les prérequis et les limites.

## Commandes

- `/candidatures:setup` : CV, préférences, interview sur le parcours, ton des lettres (une seule fois)
- `/candidatures:cover-letter [offre | spontanée entreprise]` : lettre de motivation, archivée avec son suivi
- `/candidatures:tailor-resume [offre]` : CV adapté à une offre
- `/candidatures:job-search` : recherche d'offres d'alternance et de stage
- `/candidatures:apply [offre]` : candidature (LinkedIn, site ou mail), avec validation avant envoi

Les données personnelles sont dans `~/.candidatures/` ; elles ne font pas partie du plugin. Les règles communes sont dans `shared/references/marche-france.md`.
