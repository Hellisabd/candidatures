# Prérequis par skill

Vérifie que les fichiers de données nécessaires existent avant de continuer. S'il en manque un d'obligatoire, affiche le message correspondant et arrête-toi.

## Fichiers nécessaires

| Fichier | setup | job-search | tailor-resume | cover-letter | apply |
|---------|:-----:|:----------:|:-------------:|:------------:|:-----:|
| `DATA_DIR/resume/*` | — | Obligatoire | Obligatoire | Obligatoire | Obligatoire |
| `DATA_DIR/preferences.md` | — | Obligatoire | — | — | — |
| `DATA_DIR/profile.md` | — | — | Recommandé | Recommandé | Recommandé |
| `DATA_DIR/linkedin-contacts.csv` | — | Facultatif | — | — | — |
| `DATA_DIR/reponses-types.md` | — | — | — | — | Obligatoire |

## Messages d'échec

- **CV manquant** : « Lancez d'abord `/candidatures:setup` pour ajouter votre CV. »
- **Préférences manquantes** : « Lancez d'abord `/candidatures:setup` pour configurer votre CV et vos préférences. »
- **Réponses types manquantes (apply)** : « Lancez `/candidatures:setup` : il crée le fichier des réponses aux formulaires. »
- **Profil manquant (tailor-resume)** : préviens que le CV sera fondé sur le seul texte du CV et demandera plus de corrections. Recommande de lancer d'abord `/candidatures:setup interview`. Laisse l'utilisateur continuer s'il le souhaite.
- **Profil manquant (cover-letter)** : préviens que la lettre sera fondée sur le seul CV. Recommande de lancer d'abord `/candidatures:setup interview`. Continue quand même.
