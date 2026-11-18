# Résolution du dossier de données

Toutes les données de l'utilisateur sont dans un dossier `.candidatures/`. Pour le trouver :

## Algorithme

1. Cherche `.candidatures/` dans le dossier de travail courant : utilise-le s'il existe
2. Cherche `~/.candidatures/` : utilise-le s'il existe
3. Si aucun n'existe :
   - **skill setup** : c'est une première installation, crée le dossier à l'étape 1
   - **tous les autres skills** : demande à l'utilisateur de lancer d'abord `/candidatures:setup`, puis arrête-toi

## Avertissement pour les sessions éphémères

Si aucun dossier n'est sélectionné (le dossier de travail ressemble à un chemin de session éphémère, comme `/sessions/...`), arrête-toi et dis à l'utilisateur :

> « Avant de commencer, il faut sélectionner un dossier pour que vos données soient conservées d'une session à l'autre. Choisissez « Work in a folder », sélectionnez votre dossier personnel, puis recommencez. »

Ne continue PAS sans dossier persistant.

## Arborescence de DATA_DIR

Dans les consignes des skills, `DATA_DIR` désigne le dossier `.candidatures/` trouvé ou créé.

```
DATA_DIR/
  resume/               # Ton CV (PDF ou DOCX)
  preferences.md        # Règles de tri des offres, ton des lettres, archivage, suivi
  profile.md            # Parcours issu de l'interview
  reponses-types.md     # Réponses aux formulaires de candidature
  lettre-modele-ton.md  # Modèle de ton des lettres (facultatif)
  linkedin-contacts.csv # Connexions LinkedIn (facultatif)
  suivi.md              # Suivi des candidatures (mode local)
  jobs/                 # Un dossier par offre ou entreprise
  job-history.md        # Journal des recherches de job-search
```
